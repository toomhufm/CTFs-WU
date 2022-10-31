# Scrambled Packets

To distribute our orders fairly, we have developed our own load balancing system. A few hackers must have had some fun and ordered flags.

Information
LB1, LB2 and LB3 are all Load Balancers.
LB1 has the IP 1.1.1.1 and uses the Round Robin Algorithm.
LB2 has the IP 1.1.1.2 and uses the Round Robin Algorithm.
LB3 has the IP 1.1.1.3 and uses the Round Robin Algorithm.
The Nodes A-Z represent characters in the Flag.

Flag Format
Wrap your answer in flag{}

![image](https://user-images.githubusercontent.com/92283038/198947644-b12d6f14-3d27-43b0-bb1d-4844be7d6917.png)


# Solution 

Mở file pcapng lên ta sẽ thấy các gói tin TCP như sau, để ý sẽ thấy có một vài gói khác thường, có độ dài là 60 (đa số các gói có độ dài là 56).
![image](https://user-images.githubusercontent.com/92283038/198947807-00643f0d-e321-4fd6-9d00-12c902bda7ba.png)

Vì vậy ta sẽ filter các gói có độ dài là 60 ra bằng filter ``` data.len == 4```. Sau đó extract phần data này ra bằng tshark để xử lý.

```
tshark -r orders.pcapng -Y "data.len == 4" > data.txt
```

![image](https://user-images.githubusercontent.com/92283038/198948197-f4239af4-a3fd-4ceb-84c7-e7b0c710dced.png)

Ở đây ta chỉ cần trường **No.** (thứ tự của gói tin) để xác định data được gửi nằm ở node nào. Theo như sơ đồ đề bài cho, data từ LB1 sẽ đi qua LB2/LB3 theo thuật toán 
Round Robin -> Tức là kí tự đầu tiên qua LB2 thì kí tự tiếp theo sẽ phải đi qua LB3 và cứ lần lượt như vậy.

__Script Solve__

```py
with open('data.txt','r') as file:
    data = file.readlines()

numb = []

for i in data:
    numb.append(int(i.split(' ')[0]))

LB2 = "ABCDEFGHIJKLM" # LB2 có 13 node con [ A -> M ]
LB3 = "IJKLMNOPQRSTUVWXYZ" # LB3 có 18 node con [ I -> Z ]

c = 0 # dùng để xác định data nằm ở LB2 hay LB3 
lb2 = 0 # index LB2
lb3 = 0 # index LB3 

for i in range(1,30001):
    if(c == 0):
        if i in numb:
            print(LB2[lb2],end="")
        lb2 += 1
        lb2 %= 13
    else:
        if i in numb:
            print(LB3[lb3],end="")
        lb3 +=1
        lb3 %= 18
    c += 1
    c %= 2
 ```
 
 ```
flag{YUMMYYUMMYSCRAMBLEDPACKETS}
```
