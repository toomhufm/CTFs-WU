Quan sát file pcap đề bài cho, ta có thể thấy các packet chứa thông tin mousemove là tọa độ (x,y). Vì vậy mình có ý tưởng là lấy các tọa độ này và vẽ lại đường đi của nó.

![image](https://user-images.githubusercontent.com/92283038/184271558-a65501fc-59a3-45f2-943b-5a8824e4c988.png)


Sử dụng tshark để lấy data của các packet HTTP

```
tshark -Y 'http.request.method == GET' -r whacking-the-froggers.pcap > get.txt
```

Script để tách tọa độ từ data và vẽ hình: 
```py
import turtle

with open('get.txt','r') as file:
    f = file.readlines()
a = []
for i in f:
    x = (i.split("x=")[1].split("&")[0])
    y = (i.split("y=")[1].split("&")[0])
    a.append([x,y])


s = turtle.getscreen()
t = turtle.Turtle()  
for i in range(len(a)):
    x = int(a[i][0])
    y = int(a[i][1])
    t.goto(x,y)
turtle.mainloop() 
```

Đây là ảnh có được, trông như chữ bị ngược. Lật ngược lại thì ta có flag là ``` corctf{LILYXOX}```

![image](https://user-images.githubusercontent.com/92283038/184271935-9c9f0f41-4655-4236-a3e8-64d3881b0af7.png)



