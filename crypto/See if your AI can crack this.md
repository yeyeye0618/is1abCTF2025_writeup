## See if your AI can crack this
hackmd 連結 : [See if your AI can crack this](https://hackmd.io/@yeyeye618/ByOnk8ajgl)
![image](https://hackmd.io/_uploads/rJ5RkIpsge.png)
題目給了 4 個檔案
一個 secp256k1 的公鑰
signatures.csv（ECDSA 簽名)

```
┌──(yeyeye618㉿yeyeye-618)-[~/Beep_beep_beep_I_can't_read_this_file]
└─$ tree .
├── pubkey.txt
├── signatures.csv
├── sign.py
└── test_vectors.py
```
題目中有提到有些 signature 有些是沒有驗證過的需要用 SEC 1/2 驗證
要求是還原出私鑰

先把好的 signature 篩出來

```python
import csv
from sign import verify, pubkey_from_hex

pubkey = pubkey_from_hex(
    "d1acf58809885440aa21f2889bf2f55c8a3cda842f0369dcc51fc5f32245c34b",
    "473951cf71f9bebe04b8e72fd09efbe76706b439584bc5212220c96bb3f23216"
)


with open("signatures.csv", newline='') as input, \
    open("verified_signatures.csv", "w", newline='') as output:
    reader = csv.DictReader(filter(lambda row: not row.startswith("#"), input))
    names = reader.fieldnames
    writer = csv.DictWriter(output, fieldnames=names)
    writer.writeheader()

    for row in reader:
        msg_hash_int = int(row["msg_hash"], 16)
        r = int(row["r"], 16)
        s = int(row["s"], 16)
        is_valid = verify(msg_hash_int, r, s, pubkey)
        if is_valid:
            writer.writerow(row)
```

![image](https://hackmd.io/_uploads/rkL4QP6oeg.png)

接著做解密私鑰
```python
import csv
from sign import n

def modinv(x, m):
    return pow(x, -1, m)

def check(r_dict):
    for r, sigs in r_dict.items():
        if len(sigs) <= 1:
            continue
        (s1, h1), (s2, h2) = sigs[0], sigs[1]
        if s1 == s2:
            continue
        k = ((h1 - h2) * modinv(s1 - s2, n)) % n
        d = ((s1 * k - h1) * modinv(r, n)) % n
        print(f"flag: is1abCTF{{{d:x}}}")
        break

r_dict = {}

with open("verified_signatures.csv") as f:
    reader = csv.DictReader(f)
    for row in reader:
        r = int(row["r"], 16)
        s = int(row["s"], 16)
        h = int(row["msg_hash"], 16)
        if r not in r_dict:
            r_dict[r] = []
        r_dict[r].append((s, h))
    check(r_dict)
```
![image](https://hackmd.io/_uploads/SyY_Hw6ilx.png)
flag: `is1abCTF{6bf908ba4975f133b8ccfceed54faa92958729979216e38cd7e5d7ece7e477a8}`