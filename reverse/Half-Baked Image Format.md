## Half-Baked Image Format
hackmd 連結 : [Half-Baked Image Format](https://hackmd.io/@yeyeye618/rJ6M2r6oxe)
![image](https://hackmd.io/_uploads/BJJQ2Hpjeg.png)
有給 `muzi.py` 跟 `flag.muzi`
`muzi.py`:
```python
from PIL import Image
import numpy as np
import crc8

def main():
    inp = input("""
                Welcome to the is1ab Image Program
                It can convert images to MUZIs
                It will also display MUZIs

                [1] Convert Image to MUZI
                [2] Display MUZI

                """)
    match inp:
        case "1":
            start_conv()
        case "2":
            pass  ##### remove display #####
        case _:
            return 0
    return 0

def start_conv():
    file = input("Enter the path to your image you want converted to a MUZI file:\n")
    out = input("Enter the path you’d like to write the MUZI to:\n")
    img = Image.open(file)
    w, h = img.size

    write = [
        b'\x4D', b'\x55', b'\x5A', b'\x49', b'\x00', b'\x01', b'\x00', b'\x02'
    ]

    for x in w.to_bytes(4):
        write.append(x.to_bytes(1))
    for y in h.to_bytes(4):
        write.append(y.to_bytes(1))

    write.append(b'\x57')
    write.append(b'\x49')
    write.append(b'\x46')
    write.append(b'\x49')
    
    for i in range(h):
        dat = [b'\x44',b'\x41',b'\x54',b'\x52']
        for j in range(w):
            dat.append(img.getpixel((j,i))[0].to_bytes(1)) 
        dat.append(getCheck(dat[4:]))
        for wa in dat:
            write.append(wa)

    for i in range(h): 
        dat = [b'\x44',b'\x41',b'\x54',b'\x47']
        for j in range(w):
            dat.append(img.getpixel((j,i))[1].to_bytes(1)) 
        dat.append(getCheck(dat[4:]))
        for wa in dat:
            write.append(wa)

    for i in range(h): 
        dat = [b'\x44',b'\x41',b'\x54',b'\x42']
        for j in range(w):
            dat.append(img.getpixel((j,i))[2].to_bytes(1))
        dat.append(getCheck(dat[4:]))
        for wa in dat:
            write.append(wa)

    write.append(b'\x44')
    write.append(b'\x41')
    write.append(b'\x54')
    write.append(b'\x45')

    with open(out, "ab") as f:
        for b in write:
            f.write(b)

    return 0

def getCheck(datr):
    dat = ''
    for w in datr:
        dat+=chr(int.from_bytes(w))
    return int.to_bytes(int(crc8.crc8(dat.encode()).hexdigest(),base=16),1)

if __name__ == '__main__':
    main()
```
應該是要把 `muzi.py` 做的反過來做以解密 `flag.muzi`

1. 先讀 header，得到圖片的大小 
2. 然後讀 RGB 的 block，每個 px 1 byte 
3. 重組 RGB 成圖片

`exploit.py`:
```
from PIL import Image

def read_channel(data, offset, tag, w, h):
    channel = []
    for i in range(h):
        assert data[offset:offset+4] == tag
        offset += 4
        row = []
        for j in range(w):
            row.append(data[offset])
            offset += 1
        offset += 1
        channel.append(row)
    return channel, offset


def inverse_muzi(data):
    w = int.from_bytes(data[8:12], 'big')
    h = int.from_bytes(data[12:16], 'big')

    offset = 16 + 4

    R, offset = read_channel(data, offset, b'DATR', w, h)
    G, offset = read_channel(data, offset, b'DATG', w, h)
    B, offset = read_channel(data, offset, b'DATB', w, h)

    img = Image.new('RGB', (w, h))
    for i in range(h):
        for j in range(w):
            img.putpixel((j, i), (R[i][j], G[i][j], B[i][j]))
    
    return img

with open("flag.muzi", "rb") as f:
    data = f.read()
output_img = inverse_muzi(data)
output_img.save("flag.png")
```
![image](https://hackmd.io/_uploads/S1LB1Upigg.png)

貓

flag : `is1abCTF{f1le_sturctur3_r3v3rs1ng_f0r_fun}`