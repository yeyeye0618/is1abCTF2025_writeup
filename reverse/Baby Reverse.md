## Baby Reverse
hackmd 連結 : [Baby Reverse](https://hackmd.io/@yeyeye618/HJx0CEpill)
![image](https://hackmd.io/_uploads/HklxyHpixe.png)
給了一個 annoy.exe
![image](https://hackmd.io/_uploads/SkQX1Spixe.png)
有一個輸入
應該是檢查輸入
正確就給 flag

用 IDA 開啟來看
![image](https://hackmd.io/_uploads/HyBA046olx.png)
可以看到剛剛的流程經過了四個 function 最後做檢查 check

1. 輸入
2. crypt_func1
3. crypt_func2
4. crypt_func3
5. crypt_func4
6. check

接下來就是分析 crypt_func 1 ~ 4 在做甚麼

crypt_func1:
![image](https://hackmd.io/_uploads/BkE0ZBaslg.png)
主要功能是把輸入每一位 XOR 0x11

crypt_func2:
![image](https://hackmd.io/_uploads/Hy5U-HTjgx.png)
如果是奇數位就 +38 偶數位 -17

crypto_func3:
![image](https://hackmd.io/_uploads/BJehvfrpile.png)
看位數 位數 mod 3 
如果 = 0 則 + 5
如果 = 1 則 - 9
如果 = 2 則 XOR 77

crypto_func4:
![image](https://hackmd.io/_uploads/HJXU7r6ill.png)
其中有用到 ror1 這個 fucntion
![image](https://hackmd.io/_uploads/Hy-c7Hpoee.png)
就是位元旋轉
![image](https://hackmd.io/_uploads/SkwyESTslx.png)

check:
![image](https://hackmd.io/_uploads/H19CYSTjex.png)
有 6 段 數值 v3 ~ v8
和在一起比較輸入
![image](https://hackmd.io/_uploads/rJX6KHpsll.png)
```
全部的數值:
36bf21cdac9a9c3a0aa685e22ec20dc9279cab9f8da6a4e234b58ed104ea0bb5ae511aea8ccb2ebca4e9354b2c4d29
```
照著上面的反過來做
```python
def rol1(b):
    return ((b << 1) & 0xff) | ((b >> 7) & 0x1)

def inverse_func4(data: bytes) -> bytes:
    return bytes(rol1(b) for b in data)

def inverse_func3(data: bytes) -> bytes:
    res = bytearray(len(data))
    for i, val in enumerate(data):
        r = i % 3
        if r == 0:
            res[i] = (val - 5) & 0xff
        elif r == 1:
            res[i] = (val + 9) & 0xff
        else:
            res[i] = val ^ 0x4D
    return bytes(res)

def inverse_func2(data: bytes) -> bytes:
    res = bytearray(len(data))
    for i, val in enumerate(data):
        if i % 2 == 0:
            res[i] = (val + 0x11) & 0xff
        else:
            res[i] = (val - 0x26) & 0xff
    return bytes(res)

def inverse_func1(data: bytes) -> bytes:
    return bytes(b ^ 0x11 for b in data)
values = [
    0x3a9c9aaccd21bf36,
    0xc90dc22ee285a60a,
    0xe2a4a68d9fab9c27,
    0xb50bea04d18eb534,
    0xbc2ecb8cea1a51ae,
    0x294d2c4b35e9a4,
]
target = b"".join(v.to_bytes(8, "little") for v in values).rstrip(b"\x00")

step4 = inverse_func4(target)
step3 = inverse_func3(step4)
step2 = inverse_func2(step3)
flag   = inverse_func1(step2)

print(target.hex())
print(flag)
```
![image](https://hackmd.io/_uploads/r10YoHTsgx.png)
flag : `is1abCTF{34syyyyy_r3v3rse_pi3c3_0f_c4k3_righ7~}`