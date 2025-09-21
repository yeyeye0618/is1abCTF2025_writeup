## Crypto Party
hackmd 連結 : [Crypto Party](https://hackmd.io/@yeyeye618/rJgoaRvogg)
![image](https://hackmd.io/_uploads/rJM06Avsee.png)
Category: Crypto

題目給了一段 密文

```
==gVftUYTV0TQFTeuR1c0J2XK9Fc1dGexh0Rt9WfNB3R291Z7Z2Y
```

說一共做了 6 次操作
仿射密碼
維吉尼亞密碼
凱撒密碼
柵欄密碼
Base64 編碼
字串逆轉

所以只要照著步驟倒回去就可以了

首先是 字串編碼 和 base64 編碼沒甚麼問題

接著是 柵欄密碼

因為上面剩下的加密方式都沒有會改變文字前後的
所以可以用試的試出來 key = 3

```
co1mfGPH{qOxggEu_pS_vJa_GbKtps_TMnVy}
```

GbKtps TMnVy 我猜是 CrYpto PArTy

而已知 prefix => co1mfGPH = is1abCTF

分別假設 維吉尼亞密碼 的 key 長度是 2 ~ 15, 1(會被 ROT 吸收)

```
comf G PHqOxggEupSvJa G bKtpsTMnVy
ABAB A BABABABABABABA B ABABABABAB 長度 2
ABCA B CABCABCABCABCA B CABCABCABC 長度 3
ABCD A BCDABCDABCDABC D ABCDABCDAB 長度 4
ABCD E ABCDEABCDEABCD E ABCDEABCDE 長度 5
ABCD E FABCDEFABCDEFA B CDEFABCDEF 長度 6
ABCD E FGABCDEFGABCDE F GABCDEFGAB 長度 7
ABCD E FGHABCDEFGHABC D EFGHABCDEF 長度 8
ABCD E FGHIABCDEFGHIA B CDEFGHIABC 長度 9
ABCD E FGHIJABCDEFGHI J ABCDEFGHIJ 長度 10
ABCD E FGHIJKABCDEFGH I JKABCDEFGH 長度 11
ABCD E FGHIJKLABCDEFG H IJKLABCDEF 長度 12
ABCD E FGHIJKLMABCDEF G HIJKLMABCD 長度 13
ABCD E FGHIJKLMNABCDE F GHIJKLMNAB 長度 14
ABCD E FGHIJKLMNOABCD E FGHIJKLMNO 長度 15
```
兩個 G 中間的長度 是 15 所以 有可能的是 除 1 之外的 15 的因數，剩下的必需有重複的字元

可以嘗試 3 5 15 再試剩下的長度為 2,4,6,7,8,9,10,11,12,13,14

剩下的 仿射密碼 只要 a 和 26 互質 
剩下的 b 可以從 1 ~ 26 嘗試
決定給 gpt 教授硬爆，爆出來
凱薩密碼需要 Rotate 12
維吉尼亞密碼 key = `key`
仿射密碼 a=7 b=0

![image](https://hackmd.io/_uploads/SJg8Z_9oxg.png)
flag : `is1abCTF{wElcoMe_tO_tHe_CrYpto_PArTy}`
