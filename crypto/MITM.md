## MITM
hackmd 連結 : [MITM](https://hackmd.io/@yeyeye618/rkW0rPajxe)
![image](https://hackmd.io/_uploads/BJty8Dpogg.png)
其中給了一個 `server.py`
```python
#!/usr/bin/env python3

import socket
import os
import time
import threading
import hashlib
import secrets
from Crypto.Cipher import AES
from Crypto.Util.Padding import pad
from Crypto.Random import get_random_bytes

# --- Server Settings ---
HOST = '0.0.0.0'
PORT = 1337
FLAG_FILE = 'flag.txt'

def get_flag():
    if not os.path.exists(FLAG_FILE):
        return "flag{file_not_found_on_server}"
    with open(FLAG_FILE, 'r') as f:
        return f.read().strip()

def handle_connection(conn, addr):
    print(f"[+] New connection from {addr[0]}:{addr[1]}, for which a separate thread has been created.")
    try:
        # --- Beginning of eavesdropping scenario ---
        conn.sendall(b"----- MITM Attack Initialized. Intercepting encrypted channel -----\n\n")
        time.sleep(1)

        # --- 1. Diffie-Hellman parameters ---
        p = 155214169224186174245759019817233758959712483609876556421679567759735878173206273314271380424223420051598278563855517852997101246947883176353747918435174813511975576536353646684036755728974423538143090186411163821396091652013002565116673426504500657692938270440503451481091007910872038288051399770068237950977
        g = 5

        # --- 2. Initial Handshake ---
        conn.sendall(b"Alice: Hello Bob, initiating secure channel.\n")
        time.sleep(1)
        conn.sendall(b"Bob: Hello Alice, I am ready.\n\n")
        time.sleep(1)

        # --- 3. Key Exchange ---
        # Alice generates private key a and calculates public key A
        alice_private_key = secrets.randbelow(p - 101) + 100 # Ensure a >= 100
        alice_public_key = pow(g, alice_private_key, p)
        conn.sendall(f"Alice -> Bob (Public Key): {alice_public_key}\n".encode('utf-8'))
        time.sleep(1)

        # Bob generates private key b and calculates public key B
        bob_private_key = secrets.randbelow(p - 101) + 100 # Ensure b >= 100
        bob_public_key = pow(g, bob_private_key, p)
        conn.sendall(f"Bob -> Alice (Public Key): {bob_public_key}\n\n".encode('utf-8'))
        time.sleep(1)
        
        # --- 4. Calculate shared secret & generate symmetric key ---
        # The server simulates Alice and calculates the shared secret (g^b)^a mod p
        shared_secret_alice = pow(bob_public_key, alice_private_key, p)

        # The server also simulates Bob to calculate the shared secret (g^a)^b mod p
        shared_secret_bob = pow(alice_public_key, bob_private_key, p)

        # Server-side verification to ensure the algorithm is implemented correctly
        if shared_secret_alice == shared_secret_bob:
            print("[V] Verification successful! Computed the same shared secret.")

        # Convert the shared secret (a large number) to a 32-byte key using SHA-256
        secret_bytes = shared_secret_alice.to_bytes((shared_secret_alice.bit_length() + 7) // 8, byteorder='big')
        symmetric_key = hashlib.sha256(secret_bytes).digest()

        # --- 5. Use AES-256-CBC to encrypt and send the message ---
        conn.sendall(b"Alice: Message is encrypted. Here it is.\n")
        
        # Encrypt Alice's message
        flag = get_flag()
        alice_part = flag[:10]
        plaintext_alice = f"Great. The first part is: {alice_part}".encode('utf-8')
        
        iv_alice = get_random_bytes(AES.block_size) # Generate a random 16-byte IV
        cipher_alice = AES.new(symmetric_key, AES.MODE_CBC, iv_alice)
        ciphertext_alice = cipher_alice.encrypt(pad(plaintext_alice, AES.block_size))
        
        encrypted_message_alice = iv_alice.hex() + ciphertext_alice.hex()
        conn.sendall(f"Alice -> Bob (Encrypted): {encrypted_message_alice}\n".encode('utf-8'))
        time.sleep(1)

        # Encrypt Bob's message
        bob_part = flag[10:]
        plaintext_bob = f"Perfect! I have the rest. It's: {bob_part}".encode('utf-8')
        
        iv_bob = get_random_bytes(AES.block_size) # A new IV must be used for each encryption
        cipher_bob = AES.new(symmetric_key, AES.MODE_CBC, iv_bob)
        ciphertext_bob = cipher_bob.encrypt(pad(plaintext_bob, AES.block_size))
        
        encrypted_message_bob = iv_bob.hex() + ciphertext_bob.hex()
        conn.sendall(f"Bob -> Alice (Encrypted): {encrypted_message_bob}\n\n".encode('utf-8'))
        
        # --- End of eavesdropping scenario ---
        conn.sendall(b"----- Target communication terminated. Eavesdropping channel closed -----\n")

    except Exception as e:
        print(f"[!] Error handling connection from {addr[0]}:{addr[1]}: {e}")
    finally:
        conn.close()
        print(f"[-] Connection with {addr[0]}:{addr[1]} closed, thread terminated.")

def main():
    with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
        # Allow address reuse to avoid "Address already in use" errors
        s.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
        
        s.bind((HOST, PORT))
        s.listen()
        
        print(f"[*] Multi-threaded server listening on {HOST}:{PORT}")
        
        while True:
            # Wait for and accept new connections
            conn, addr = s.accept()
            
            # Create a new thread for this connection
            client_thread = threading.Thread(target=handle_connection, args=(conn, addr))
            
            # Start the thread
            client_thread.start()

if __name__ == '__main__':
    main()
```

![image](https://hackmd.io/_uploads/r16L_vTilx.png)

這個 `server.py` 模擬了 Diffie–Hellman key exchange 
給出質數p和一個base g，並把 Alice 和 Bob 的公開金鑰和 flag 用 AES-256-CBC 加密

[Diffie-Hellman key exchange](https://zh.wikipedia.org/wiki/%E8%BF%AA%E8%8F%B2-%E8%B5%AB%E7%88%BE%E6%9B%BC%E5%AF%86%E9%91%B0%E4%BA%A4%E6%8F%9B)

這個方法理論上就算知道 p 和 g 也是很難破解的
但因為這題的 p 找的不好 ( p-1 可以被拆成小的質因數可以用 [Pohlig–Hellman](https://en.wikipedia.org/wiki/Pohlig%E2%80%93Hellman_algorithm) 解)，所以可以很容易的破解出來

```
import gmpy2
from gmpy2 import mpz, powmod, invert
import hashlib
from Crypto.Cipher import AES
from Crypto.Util.Padding import unpad
from multiprocessing import Pool, cpu_count

p = mpz("155214169224186174245759019817233758959712483609876556421679567759735878173206273314271380424223420051598278563855517852997101246947883176353747918435174813511975576536353646684036755728974423538143090186411163821396091652013002565116673426504500657692938270440503451481091007910872038288051399770068237950977")
g = mpz(5)

def decrypt_hexblob(hexblob, key):
    iv = bytes.fromhex(hexblob[:32])
    ct = bytes.fromhex(hexblob[32:])
    cipher = AES.new(key, AES.MODE_CBC, iv)
    pt = unpad(cipher.decrypt(ct), AES.block_size)
    return pt.decode('utf-8', errors='replace')

def bsgs(g, h, p):
    g, h, p = mpz(g), mpz(h), mpz(p)
    m = int(gmpy2.isqrt(p)) + 1
    table = {}
    x = mpz(1)
    for j in range(m):
        if x not in table:
            table[x] = j
        x = (x * g) % p
    factor = powmod(g, m*(p-2), p)
    gamma = h
    for i in range(m):
        if gamma in table:
            return i*m + table[gamma]
        gamma = (gamma * factor) % p
    return None

def discrete_log_prime_power(g, h, p_mod, q, e):
    x = mpz(0)
    for k in range(e):
        exp = (p_mod-1)//(q**(k+1))
        hk = powmod(h * invert(powmod(g, x, p_mod), p_mod), exp, p_mod)
        gk = powmod(g, exp, p_mod)
        d = bsgs(gk, hk, p_mod)
        if d is None:
            raise ValueError("BSGS failed")
        x += d * (q**k)
    return x

def crt(mods, rems):
    M = mpz(1)
    for m in mods: M *= m
    x = mpz(0)
    for m, r in zip(mods, rems):
        Mi = M // m
        inv = invert(Mi, m)
        x += r * Mi * inv
    return x % M

def worker(params):
    g, h, p_mod, q, e = params
    xi = discrete_log_prime_power(g, h, p_mod, q, e)
    return (q**e, xi % (q**e))

if __name__ == "__main__":
    factors = {
        2:1, 3:2, 5:1, 7:1, 13:1
    }
    alice_pub = mpz(str(input("Alice public key:")))
    bob_pub   = mpz(str(input("Bob public key:")))

    alice_enc_hex = str(input("alice msg:"))
    bob_enc_hex   = str(input("bob msg:"))
    
    with Pool(cpu_count()) as pool:
        results = pool.map(worker, [(g, alice_pub, p, q, e) for q,e in factors.items()])

    mods, rems = zip(*results)
    alice_priv = crt(mods, rems) % (p-1)

    shared = powmod(bob_pub, alice_priv, p)
    key = hashlib.sha256(int(shared).to_bytes((shared.bit_length()+7)//8, byteorder='big')).digest()

    pt_a = decrypt_hexblob(alice_enc_hex, key)
    pt_b = decrypt_hexblob(bob_enc_hex, key)
    print("[*] Full flag:", (pt_a + pt_b).split(":")[-1].strip())
```
![image](https://hackmd.io/_uploads/rkP42dpogl.png)
flag : `is1abCTF{D1ff1e_h3llm4n_W1th_Sm0o7h_4t7Ack}`