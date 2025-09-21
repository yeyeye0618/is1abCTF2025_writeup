## File Stealer
hackmd 連結 : [File Stealer](https://hackmd.io/@yeyeye618/SJiHEEaill)
![image](https://hackmd.io/_uploads/Bk0t4V6jle.png)
有給一個壓縮檔，裡面是一個 img 而且開不起來
![image](https://hackmd.io/_uploads/rkhiNEpsxe.png)

直接用 binwalk 撈裡面的檔案試試
```
binwalk -e file_stealer.img
```
![image](https://hackmd.io/_uploads/BJKSrVaiex.png)
有一個壓縮檔
解壓縮後 有一個 doc.txt
![image](https://hackmd.io/_uploads/H1s5rN6jxg.png)

就找到 flag 了
flag : `is1abCTF{h1dd3n_p4rt1t10n_1s_4w3s0m3}`