# Try harder
hackmd 連結 : [Try harder](https://hackmd.io/@yeyeye618/ryj9e6Pixg)
![image](https://hackmd.io/_uploads/SJHZZ6Djlg.png)

Category: Misc

給了 CHW 學長的 [github](https://github.com/Chw41/OffSec-Certification)
而且提到了 RDP 跟 stephanie 這個名字


在 [[OSCP, PEN-200] Instructional notes - Part 6.md](https://github.com/Chw41/OffSec-Certification/blob/main/%5BOSCP%2C%20PEN-200%5D%20Offensive%20Security%20Certified%20Professional%20/Instructional%20notes/%5BOSCP%2C%20PEN-200%5D%20Instructional%20notes%20-%20Part%206.md) 有看到相關的線索
![image](https://hackmd.io/_uploads/ByxKz6wogg.png)

把圖片下載下來用 strings 看

![image](https://hackmd.io/_uploads/S1D87pDoxx.png)

可以看到一個像是 JWT 的 東西


```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJhdXRob3IiOiJDSFciLCJmbGFnX3BhcnQxIjoiaXMxYWJDVEZ7N3J5X2g0cmQzciJ9.L-7NOoFHngPpQVNHXRBXLuWmskw6xA9h1Q6IiOFVfK8
```

用 base64 解碼可以看到
![image](https://hackmd.io/_uploads/BJNciCwsxx.png)
```
{"alg":"HS256","typ":"JWT"}{"author":"CHW","flag_part1":"is1abCTF{7ry_h4rd3r"}
```
看起來只有一段 flag

之後花了點時間從 github 的 commit 紀錄找到了這張圖片之前的圖片
可以看到之前也有一張圖片
![image](https://hackmd.io/_uploads/SyjXhCDjge.png)

把圖片抓下來
![image](https://hackmd.io/_uploads/HysI3RPjge.png)
一模一樣的圖片

用同樣的方法
![image](https://hackmd.io/_uploads/SkAChCvieg.png)
一樣也有一段 JWT Token
![image](https://hackmd.io/_uploads/rJwbTRDilx.png)

獲得 flag
`is1abCTF{7ry_h4rd3r_f0r_05cp_by_chw}`