# EXIF - Metadata

Statement
Our sad friend pepo got lost! Can you find where he is ?

Kiếm tra metada của ảnh bằng exiftool thì thấy có phần tọa độ 
```
GPS Latitude                    : 43 deg 17' 56.27" N
GPS Longitude                   : 5 deg 22' 49.38" E
GPS Position                    : 43 deg 17' 56.27" N, 5 deg 22' 49.38" E
```

Tìm tọa đồ trên GoogleMap thì ta sẽ biết được thành tên thành phố 

![image](https://user-images.githubusercontent.com/92283038/180264174-169b4d4b-e609-40d9-a6fd-a501e4f29777.png)

# Dot and next line

Statement
“Rien de trop est un point dont on parle sans cesse et qu’on n’observe point.”

![journal](https://user-images.githubusercontent.com/92283038/180264420-e8e3a92d-aa8d-4486-a6d6-d97c029b6e5d.jpg)

Một bài khá là bịp =)))) tên bài là Dot and next line nên mình cứ nhìn mãi mấy cái chấm với dòng tiếp theo nhưng không ra gì cả. Theo như trên forum thì có một vài user đã 
hint là đừng để ý next line, mà hãy heading up. Sau một hồi thì mình cũng đoán ra được. Tại vị trí mỗi dấu chấm, ta sẽ lấy kí tự phía trên nó và có được flag. 

# Steganomobile

Statement
After extraction of mobile data, the searcher, investigator have get this sequence of numbers. Maybe a phone number ?

``` 222-33-555-555-7-44-666-66-33 ```

Ai ngày xưa hay xài đt cục gạch thì chắc đoán ra được rồi ha =))) Đem so với bàn phím điện thoại cục gạch là ra flag 

# Twitter Secret Messages

Statement
We suspect that this tweet hides a rendezvous point. Help us to find it.

``` 
Ｃhｏose  a  jοｂ  yоu  lονｅ,  and  you  ｗіｌl  ｎeｖｅｒ  have  tο  ｗｏrk  a  day  in  yοur  lіfｅ．                        

```

Mình có tìm ra [tool](https://holloway.nz/steg/) này để decode, đây là 1 kiểu mã hóa tin nhắn bằng Unicode homoglyphs để giấu đi thông điệp bí mật 

# Yellow dots

Statement
You attend an interview for a forensic investigator job and they give you a challenge to solve as quickly as possible (having the Internet).
They ask you to find the date of printing as well as the serial number of the printer in this document.
You remain dubitative and accept the challenge.

[chall images](http://challenge01.root-me.org/steganographie/ch18/ch18.png)

Theo như docs mà tác giả cung cấp thì mỗi bản in từ máy in sẽ có những chấm tròn nhỏ chứa 1 số dữ liệu như ngày tháng và serial của máy in. Để tìm được mấy chấm nhỏ
này thì mình xài photoshop.

Đây là mấy chấm mình tìm được 
![image](https://user-images.githubusercontent.com/92283038/180265832-3b866c41-41dc-4bf2-9e7d-62f36270a424.png)

Do không thấy tool decode nên mình xem docs rồi tự decode bằng tay 
![image](https://user-images.githubusercontent.com/92283038/180265943-8f76cbec-c357-4ee9-ad2d-90322d664d3f.png)

# EXIF - Thumbnail


Statement
Find the password hidden in this image in JPG format.

[chall image](http://challenge01.root-me.org/steganographie/ch10/ch10.jpg)

Kiểm tra bằng binwalk thì thấy trong file ảnh này còn chứa 1 số ảnh khác nữa 
```
@t00m ➜ ~ binwalk ch10.jpg

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             JPEG image data, JFIF standard 1.01
30            0x1E            TIFF image data, big-endian, offset of first image directory: 8
202           0xCA            JPEG image data, JFIF standard 1.01
232           0xE8            TIFF image data, big-endian, offset of first image directory: 8
404           0x194           JPEG image data, JFIF standard 1.01
```

Giờ thì mình extract ra 

```
@t00m ➜ ~ binwalk --dd=".*" ch10.jpg

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             JPEG image data, JFIF standard 1.01
30            0x1E            TIFF image data, big-endian, offset of first image directory: 8
202           0xCA            JPEG image data, JFIF standard 1.01
232           0xE8            TIFF image data, big-endian, offset of first image directory: 8
404           0x194           JPEG image data, JFIF standard 1.01
```

Đem lên [CyberChef](https://gchq.github.io/CyberChef/) là sẽ xem được ảnh, CyberChef mãi đỉnh =)) 

# WAV - Spectral analysis

Statement
Interesting mix.

Bài này khá đơn giản, tải file về cho vào [Sonic Visualiser](https://www.sonicvisualiser.org) xem spectrogram là thấy flag

# APNG - Just A PNG

Statement
Your joking colleague challenges you to find the message hidden in this animation.

Mình disassemble cái APNG này bằng [tool này](http://apngdis.sourceforge.net)

![image](https://user-images.githubusercontent.com/92283038/180267393-cf243502-edcb-4c2d-80f2-5d6ee6f7c67c.png)

Chúng ta sẽ có thời gian delay giữa các khung hình

```
@t00m ➜ ~ cat *txt
delay=70/10
delay=76/10
delay=65/10
delay=71/10
delay=58/10
delay=80/10
delay=51/10
delay=80/10
delay=111/10
delay=70/10
delay=82/10
delay=111/10
delay=71/10
```

Trông như là các mã ascii, mình viết 1 script python để decode 

```py
for i in range(1,14):
    with open('apngframe{:02d}.txt'.format(i),'r')  as f:
        file = f.readline()
        res = int((file[6:].split('/')[0]))
        print(chr(res),end="")
```

# PDF - Embedded

Statement
Find the hidden information in this PDF file.

Mình kiểm tra file bằng [peedf](https://github.com/jesparza/peepdf)

```
@t00m ➜ ~ python2 peepdf.py -m ../epreuve_BAC_2004.pdf
Warning: PyV8 is not installed!!
Warning: pylibemu is not installed!!

File: epreuve_BAC_2004.pdf
MD5: 89d00a355489034dd39ddea2b426fb46
SHA1: be1edb8e45be559388dad27128595daac3d1fae9
SHA256: 579d71aa13b0bf20289bb6beabdbeccff5ccc2776d38a11808e8bed6650d1650
Size: 2388166 bytes
Version: 1.3
Binary: True
Linearized: False
Encrypted: False
Updates: 0
Objects: 78
Streams: 25
URIs: 0
Comments: 0
Errors: 0
```

Sau đó mình xài pdf-parser để xem content của các Object thì phát hiện một số ảnh. File này khi vào xem chỉ hiện thị 12 ảnh nhưng mình thấy có nhiều hơn,
nên là mình quyết định copy hex code rồi lên CyberChef xem =))) Kết quả là thấy flag

Mình thấy flag ở Object 77

![image](https://user-images.githubusercontent.com/92283038/180269094-41235363-72dc-498d-aff4-61f7e62982f0.png)

# PNG - Least Significant Bit | PNG - Pixel Indicator Technique | PNG - Pixel Value Differencing 

3 bài này là các dạng giấu dữ liệu thường gặp trong Stegno, nhưng đề cho khá đơn giản chỉ cần xài tool là ra flag nên mình không viết nhiều :> 

# Crypt-art

Statement
A police unit intercepted a message from a terrorist group. This message may contain a secret key used to encrypt other communications. They need you to decrypt it !

Bài cho chúng ta hint là 

``` "a language where the programs are works of modern art’’ ``` 

Search thì thấy đây là một ngôn ngữ lập trình bủh bủh lmao, chương trình của chúng ta sẽ trông như những Pixel Art tên là Piet 

Ok, đầu tiên mình check qua file đề cho. 

```
@t00m ➜ ~ strings ch8.ppm
70 50
  Hi! Welcome to
esoteric programming!
The encrypted pass is:
EPCQFBXKWURQCTXOIPMNV
 Bienvenue a la program
mation esoterique!
Le pass encrypte est
EPCQFBXKWURQCTXOIPMNV
``` 

Oh chúng ta có 1 đoạn thông điệp bị mã hóa. 

Còn đây là file đề cho, đơn giản là một chương trình Piet, chúng ta chỉ cần biên dịch và xem output là gì

![ch8](https://user-images.githubusercontent.com/92283038/180269949-640ff052-86cb-4abf-b39c-1250f6b68438.png)

Mình sử dụng 1 [tool này](https://www.bertnase.de/npiet/) để biên dịch chương trình. Chúng ta có output là ```key is EYJFRGTT```

Chúng ta có 1 đoạn ciphertext và key, đây có vẻ như là mã hóa vinegar cipher, dùng tool online decode để lấy flag
