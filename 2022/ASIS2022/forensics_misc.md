# Comopti 

Đầu tiên mình sử dụng [Raise Data Recovery for ResierFS](https://www.ufsexplorer.com/raise-data-recovery-reiser/) để recover 2 file pgm 

![image](https://user-images.githubusercontent.com/92283038/196042116-7e24b23f-0abb-44e7-be37-2c5d63244f55.png)

Để xem được ảnh thì ta cần sửa lại phần này. Đọc thêm về PGM tại [đây](https://subsurfwiki.org/wiki/Portable_gray_map)

![image](https://user-images.githubusercontent.com/92283038/196042263-e6e7378f-1c87-4aa7-82aa-7d42a2e0e8d8.png)

Sửa dimension  (xem số lượng hàng và cột) và ở các dòng phía trên ta cần xóa dấu # và thêm vào giữa các kí tự một khoảng trắng. Cuối cùng xem file ảnh bằng GIMP

![image](https://user-images.githubusercontent.com/92283038/196043223-747c24b4-b39a-4faf-a3a0-f51a9c4f5a34.png)

Làm theo hướng dẫn trong ảnh là ta có flag

![image](https://user-images.githubusercontent.com/92283038/196043253-35dddca4-15aa-4471-832c-9672b0dec639.png)

```
FLAG : ASIS{pgm_1M4gE_f0Rma7_ManUpL4T!On!}
```


# Wormgep

Bài này đề cho 1 file report nhưng đã bị encrypt, mình chỉ việc cho vào CyberChef là có flag =))) 
![image](https://user-images.githubusercontent.com/92283038/196043356-8cfc6012-3dd5-47d7-98fc-1eaecafaa282.png)

```
FLAG : ASIS{N07_@ll_v!ru535_@r3_AS_8@d_a5_cov!d}
```

# Stacked QRS

Bài này thì cho chúng ta các mã QR bị chèn lên nhau, nhưng chỉ bị chèn ở các góc chứa các mảnh Position Dectection Pattern nên việc sửa mã là rất đơn giản

![image](https://user-images.githubusercontent.com/92283038/196043394-a2a0b22a-2f17-419c-aca0-29aea034d957.png)

Mình cho vào photoshop rồi đánh dấu các mã rồi kiểm tra từng mã một 

![image](https://user-images.githubusercontent.com/92283038/196043506-df9a088f-eece-4f33-823b-2960bb7231a5.png)

Sau một hồi check thì có 3 mã QR cho ta 3 phần của flag

``` 
FLAG : ASIS{7iM3_70_fix_7Hi5_0ld_diR7y_PRin73R!!}
```




