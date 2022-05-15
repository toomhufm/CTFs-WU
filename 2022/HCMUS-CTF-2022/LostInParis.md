# Name : LostInParis 
# Category : Forensics
## Description : 
Vì giải đã kết thúc và đóng chall nên mình không còn description gốc. Tóm tắt lại là đề yêu cầu chúng ta recovery lại windows password, email password và twitter password

**Flag format** : HCMUS-CTF{windows-pass_email-pass_twitter-pass}


### Solution : 

Đầu tiên giải nén file đính kèm của đề bài và xài lệnh file để kiểm tra file 

    unrar e LostInParis.rar
    file forensic

Chúng ta có kết quả là 1 file data 

    forensic: data
 
 Mình sử dụng lệnh strings để xem thử thì thấy đây là 1 file memory dump của Windows 

    WDMClassesOfDriver
    AMLIEvalData1
    C:\WINDOWS\system32\DRIVERS\ACPI.sys[ACPIMOFResource]
    WDMClassesOfDriver
    AMLIEvalData1_TypeGroup1
    C:\WINDOWS\system32\DRIVERS\ACPI.sys[ACPIMOFResource]
    MS_SystemInformation
    WMIProv
    Guid
    {98A2B9D7-94DD-496a-847E-67A5557A59F2}
    
 ### Volatility 
 Mình sẽ sử dụng [volatility](https://github.com/volatilityfoundation/volatility) để phân tích file này

Đầu tiên là kiểm tra imageinfo bằng lệnh

    vol.py -f forensic imageinfo
    ┌──(t00m㉿toomhufm)-[/mnt/e/CyberSec/HCMUS]
    └─$ vol.py -f forensic imageinfo
    Volatility Foundation Volatility Framework 2.6.1
    INFO    : volatility.debug    : Determining profile based on KDBG search...
              Suggested Profile(s) : WinXPSP2x86, WinXPSP3x86 (Instantiated with WinXPSP2x86)
                         AS Layer1 : IA32PagedMemoryPae (Kernel AS)
                         AS Layer2 : FileAddressSpace (/mnt/e/CyberSec/HCMUS/forensic)
                          PAE type : PAE
                               DTB : 0xb40000L
                              KDBG : 0x80545ae0L
              Number of Processors : 1
         Image Type (Service Pack) : 3
                    KPCR for CPU 0 : 0xffdff000L
                 KUSER_SHARED_DATA : 0xffdf0000L
               Image date and time : 2013-08-29 19:30:18 UTC+0000
         Image local date and time : 2013-08-29 21:30:18 +0200
         
 #### Windows Password
         
 Đầu tiên để tìm _Windows password_ mình sử dụng lệnh hashdump
      
      ┌──(t00m㉿toomhufm)-[/mnt/e/CyberSec/HCMUS]
      └─$ vol.py -f forensic --profile=WinXPSP2x86 hashdump
      Volatility Foundation Volatility Framework 2.6.1
      Administrateur:500:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
      Invit:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
      HelpAssistant:1000:860664416da453ed895ae1b92d903b5e:abfa8a32902993f56cbf8188f708ee6f:::
      SUPPORT_388945a0:1002:aad3b435b51404eeaad3b435b51404ee:90caa058fbdd5bc1c14c893d571e05dd:::
      w3user:1004:5ddaeb42046e0f3f6fe90785485036fd:adfd5cfad559c822372ceb76013cc8e9:::
      
 Chúng ta đã có được password hash của user là **w3user** (Vì trước đó mình có thấy một có path trong ổ C với username này)
 
      C:\Documents and Settings\w3user\Local Settings\Temporary Internet Files\Content.IE5\0XAVKLQN\j-W1EhKjwpO[2].js
      HOMEPATH=\Documents and Settings\w3user
      TEMP=C:\DOCUME~1\w3user\LOCALS~1\Temp
      TMP=C:\DOCUME~1\w3user\LOCALS~1\Temp
      USERNAME=w3user
      USERPROFILE=C:\Documents and Settings\w3user
      APPDATA=C:\Documents and Settings\w3user\Application Data
      HOMEPATH=\Documents and Settings\w3user
      TEMP=C:\DOCUME~1\w3user\LOCALS~1\Temp
      TMP=C:\DOCUME~1\w3user\LOCALS~1\Temp
      USERNAME=w3user
      USERPROFILE=C:\Documents and Settings\w3user
      
 Mình crack hash này bằng [Hashes.com](https://hashes.com) và kết quả là : **5ddaeb42046e0f3f6fe90785485036fd:IL0VEFORENSIC**
 
 #### Email Password 
 
 Thường mọi người thường sử dụng copy & paste để điền mật khẩu đúng không? Vậy thì nó được lưu ở đâu? 

**CLIPBOARD** 

Ok, cùng kiểm tra clipboard nào 

    ┌──(t00m㉿toomhufm)-[/mnt/e/CyberSec/HCMUS]
    └─$ vol.py -f forensic --profile=WinXPSP2x86 clipboard -v
    Volatility Foundation Volatility Framework 2.6.1
    Session    WindowStation Format                 Handle Object     Data
    ---------- ------------- ------------------ ---------- ---------- --------------------------------------------------
             0 WinSta0       0xc009L               0x901b1 0xe2482a30
    0xe2482a3c  c6 00 05 00                                       ....
             0 WinSta0       CF_UNICODETEXT       0x3803b5 0xe25bfa60 SnapshotIsReallyNiceForHacker
    0xe25bfa6c  53 00 6e 00 61 00 70 00 73 00 68 00 6f 00 74 00   S.n.a.p.s.h.o.t.
    0xe25bfa7c  49 00 73 00 52 00 65 00 61 00 6c 00 6c 00 79 00   I.s.R.e.a.l.l.y.
    0xe25bfa8c  4e 00 69 00 63 00 65 00 46 00 6f 00 72 00 48 00   N.i.c.e.F.o.r.H.
    0xe25bfa9c  61 00 63 00 6b 00 65 00 72 00 00 00               a.c.k.e.r...
             0 WinSta0       0xc013L               0x901e3 0xe1cfcd78
    0xe1cfcd84  00 00 00 00 78 00 00 00 01 00 00 00 01 00 00 00   ....x...........
    0xe1cfcd94  00 00 00 00 00 00 00 00 0d 00 ff ff 00 00 00 00   ................
    0xe1cfcda4  01 00 00 00 ff ff ff ff 01 00 00 00 01 00 00 00   ................
    0xe1cfcdb4  00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00   ................
    0xe1cfcdc4  00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00   ................
    0xe1cfcdd4  00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00   ................
    0xe1cfcde4  00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00   ................
    0xe1cfcdf4  00 00 00 00 00 00 00 00                           ........
             0 WinSta0       CF_LOCALE            0x1101ad 0xe2555818
    0xe2555824  0c 04 00 00                                       ....
             0 WinSta0       CF_TEXT                   0x1 ----------
             0 WinSta0       CF_OEMTEXT                0x1 ----------

Tada, **SnapshotIsReallyNiceForHacker** vậy là xong email password 

#### Twitter Password 

Cái này mình chỉ đơn giản là strings file ra và grep twitter để tìm thử thì thấy luôn 

    ┌──(t00m㉿toomhufm)-[/mnt/e/CyberSec/HCMUS]
    └─$ strings forensic | grep twitter
    mage:url(//s.ytimg.com/yts/imgbin/www-sharebar-vfl_xbRuQ.png);background-repeat:no-repeat;width:24px;height:24px;background-size:auto;vertical-align:middle}.share-service-icon-ameba-sharebar{background-position:0 -868px}.share-service-icon-bebo-sharebar{background-position:0 -280px}.share-service-icon-blogger-sharebar{background-position:0 -532px}.share-service-icon-cyworld-sharebar{background-position:0 -588px}.share-service-icon-delicious-sharebar{background-position:0 -784px}.share-service-icon-digg-sharebar{background-position:0 -924px}.share-service-icon-facebook-sharebar{background-position:0 -140px}.share-service-icon-fotka-sharebar{background-position:0 -476px}.share-service-icon-goo-sharebar{background-position:0 -252px}.share-service-icon-googleplus-sharebar{background-position:0 -364px}.share-service-icon-grono-sharebar{background-position:0 -28px}.share-service-icon-hi5-sharebar{background-position:0 0}.share-service-icon-hyves-sharebar{background-position:0 -308px}.share-service-icon-linkedin-sharebar{background-position:0 -84px}.share-service-icon-livejournal-sharebar{background-position:0 -168px}.share-service-icon-meneame-sharebar{background-position:0 -672px}.share-service-icon-mixi-sharebar{background-position:0 -392px}.share-service-icon-mixx-sharebar{background-position:0 -560px}.share-service-icon-myspace-sharebar{background-position:0 -980px}.share-service-icon-nujij-sharebar{background-position:0 -336px}.share-service-icon-odnoklassniki-sharebar{background-position:0 -448px}.share-service-icon-pinterest-sharebar{background-position:0 -644px}.share-service-icon-rakuten-sharebar{background-position:0 -616px}.share-service-icon-reddit-sharebar{background-position:0 -56px}.share-service-icon-skyblog-sharebar{background-position:0 -700px}.share-service-icon-sledzik-sharebar{background-position:0 -840px}.share-service-icon-stumbleupon-sharebar{background-position:0 -504px}.share-service-icon-tuenti-sharebar{background-position:0 -196px}.share-service-icon-tumblr-sharebar{background-position:0 -420px}.share-service-icon-twitter-sharebar{background-position:0 -812px}.share-service-icon-vkontakte-sharebar{background-position:0 -756px}.share-service-icon-webryblog-sharebar{background-position:0 -224px}.share-service-icon-weibo-sharebar{background-position:0 -728px}.share-service-icon-wykop-sharebar{background-position:0 -112px}.share-service-icon-yahoo-sharebar{background-position:0 -952px}.share-service-icon-yigg-sharebar{background-position:0 -896px}.ytp-tooltip{position:absolute;left:0;top:0;display:none;overflow:visible;z-index:970}.ytp-tooltip-body{position:absolute;left:0;bottom:5px}.ytp-tooltip-below .ytp-tooltip-body{top:5px}.ytp-tooltip-content{position:relative;padding:2px 5px;font-size:11px;line-height:20px;color:#e3e3e3;background:rgb(22,22,22)}.ytp-tooltip-content-text{white-space:nowrap}.ytp-tooltip-arrow{position:absolute;left:-5px;bottom:0;width:0;height:0;border:1px solid transparent;border-width:5px 5px 0 5px;border-top-color:rgb(22,22,22)}.ytp-tooltip-below .ytp-tooltip-arrow{top:0;bottom:auto;border-width:0 5px 5px 5px;border-top-color:transparent;border-bottom-color:rgb(22,22,22)}.html5-video-info-panel{position:absolute;display:none;top:10px;left:10px;background:#1b1b1b;color:#fff;z-index:960}.html5-video-info-panel-close{position:absolute;top:5px;right:5px;cursor:pointer}.html5-video-info-panel-content{padding:5px}.html5-video-info-table th,.html5-video-info-table td{padding:3px;text-align:left}.html5-video-element-info-table table{border-collapse:collapse}.html5-video-element-info-table th,.html5-video-element-info-table td{border:1px solid #999;text-align:center}.html5-watermark{opacity:0.5;position:absolute;right:5px;bottom:3px;z-index:840;transform-origin:100% 100%;-webkit-transform-origin:100% 100%;-moz-transform-origin:100% 100%;-o-transform-origin:100% 100%;-ms-transform-origin:100% 100%;pointer-events:auto;-moz-transition:opacity 0.1s ease-out,bottom 0.1s ease-out;-ms-transition:opacity 0.1s ease-out,bottom 0.1s ease-out;-o-transition:opacity 0.1s ease-out,bottom 0.1s ease-out;
    \uc1\pard\f0\fs20 twitter \tab\tab w3twit:OverIsMyHero}
    
 Ở cuối các bạn có thể thấy dòng _\tab\tab w3twit:OverIsMyHero_ mình đoán đó là password 

## Final Step 
Giờ chúng ta có : 

Windows Password : IL0veForensic 

Email Password : SnapshotIsReallyNiceForHacker 

Twitter Password : OverIsMyHero

    flag: HCMUS-CTF{IL0veForensic_SnapshotIsReallyNiceForHacker_OverIsMyHero}
  
 
