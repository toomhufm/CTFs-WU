# Love Runs Out 
# Category : Forensics
## Description :
My ex-girlfriend wanted to delete all of our photos on my computer. So, she hired a hacker to attack my machine and remove all the files. Can you help me analyze the memory dump and find out what happened?

File: https://drive.google.com/file/d/1LIIrR_cepIZlvoJHy_b8zmd45J48LweC/view?usp=sharing

Flag's format: HCMUS-CTF{PID_ProcessName_Port_ProcessExecuteTime(Day of the week-Month-Day-Hour:Min:Sec-Years)_Attacker'sIP_Attacker'sPort}

Ex: HCMUS-CTF{100_explorer.exe_4444_Wed-May-25-12:00:00-2022_192.168.1.1_34215}

## Solution

Đầu tiên sử dụng connscan

    @t00m ➜ ~ vol.py -f LoveRunsOut --profile=WinXPSP2x86 connscan
    Volatility Foundation Volatility Framework 2.6.1
    Offset(P)  Local Address             Remote Address            Pid
    ---------- ------------------------- ------------------------- ---
    0x05558b38 172.30.1.6:80             1.226.182.38:59495        1124

Ở đây ta thấy một connection đến địa chỉ **1.226.182.38** và port **59495** bởi process có PID là **1124**

    @t00m ➜ ~ vol.py -f LoveRunsOut --profile=WinXPSP2x86 pstree
    Volatility Foundation Volatility Framework 2.6.1
    Name                                                  Pid   PPid   Thds   Hnds Time
    -------------------------------------------------- ------ ------ ------ ------ ----
     0xff2b6180:nc.exe                                   1124   1724      3     64 2012-11-02 09:06:48 UTC+0000
    . 0xff3586b8:cmd.exe                                  164   1124      1     39 2012-11-02 09:06:49 UTC+0000
    .. 0xff349868:cmd.exe                                 616    164      1     38 2012-11-02 09:08:02 UTC+0000
 Sử dụng pstree để tìm process khả nghi thì thấy nc.exe có PID là 1124 và có process là cmd.exe. Vì chương trình này gọi cmd để thực hiện gì đó nên rõ ràng đây là process mà chúng ta cần tìm
 
 Cuối cùng là tìm port của process, mình sử dụng **sockets** 
 
     @t00m ➜ ~ vol.py -f LoveRunsOut --profile=WinXPSP2x86 sockets
    Volatility Foundation Volatility Framework 2.6.1
    Offset(V)       PID   Port  Proto Protocol        Address         Create Time
    ---------- -------- ------ ------ --------------- --------------- -----------
    0xff23ed08     1124     80      6 TCP             0.0.0.0         2012-11-02 09:06:48 UTC+0000
    0xff344d80      732    500     17 UDP             0.0.0.0         2012-11-02 09:04:44 UTC+0000
    0xff236e98        4    138     17 UDP             172.30.1.6      2012-11-02 09:04:38 UTC+0000
Và chúng ta có port là **80**

Ghép các mảnh ghép lại và ta có flag : __HCMUS-CTF{1124_nc.exe_80_Fri-Nov-02-09:06:48-2012_1.226.182.38_59495}__
