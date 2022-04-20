# Cheatsheet

## file to hex

`cat file | xxd -p | tr -d '\\n'`

## show public key details

```
openssl rsa -noout -text -inform PEM -in file.pub -pubin
RsaCtfTool --dumpkey --key file.key

```

## Encrypt/Decrypt file using RSA private key

[https://kulkarniamit.github.io/whatwhyhow/howto/encrypt-decrypt-file-using-rsa-public-private-keys.html](https://kulkarniamit.github.io/whatwhyhow/howto/encrypt-decrypt-file-using-rsa-public-private-keys.html)

## Encrypt/Decrypt file with AES-CBC cipher

```python
from Crypto.Cipher import AES
from Crypto import Random
from dns.resolver import Resolver, NXDOMAIN
from binascii import hexlify, unhexlify

key = unhexlify('2c218bf9582baacb0a36b624e5b05cf7')
iv= unhexlify('e034dd30293bf09eceb8eebcd03466d6')
filename = '/home/h3x/work/ctf/24hctf/ransomware2/test.txt'

def pkcs7(data):
    pad = 16 - len(data)
    return data + (bytes([pad]) * pad)

def encrypt_files():
    aes = AES.new(key, AES.MODE_CBC, iv)
    enc_filename = '/home/h3x/work/ctf/24hctf/ransomware2/test.enc'
    with open(filename, "rb") as infile, open(enc_filename, "wb") as outfile:
        data = infile.read(16)
        while len(data) == 16:
            outfile.write(aes.encrypt(data))
            data = infile.read(16)
        outfile.write(aes.encrypt(pkcs7(data)))

def decrypt():
    aes = AES.new(key, AES.MODE_CBC, iv)
    enc_filename = '/home/h3x/work/ctf/24hctf/ransomware2/lsass.enc'
    dec_filename = '/home/h3x/work/ctf/24hctf/ransomware2/lsass.dmp'
    with open(enc_filename, "rb") as infile, open(dec_filename, "wb") as outfile:
        data = infile.read(16)
        while len(data) == 16:
            outfile.write(aes.decrypt(data))
            data = infile.read(16)
        #outfile.write(aes.decrypt(pkcs7(data)))

#encrypt_files()
decrypt()
```

## hex byte array > xor

```python
byte_array  = [0xd6, 0xab, 0xe7, 0xf0, 0xf3, 0xa3, 0xb3, 0xa4, 0xc8, 0xfd, 0xbb, 0xf7, 0xe7, 0xd6, 0xb3, 0xfd, 0xd6, 0xb3, 0xa0, 0xfd, 0xa0, 0xf4, 0xd6, 0xb2, 0xe3, 0xb5, 0xf0, 0xbc, 0xf4, 0xa9, 0xa5]
def decode(byte_array):
        out = ""
        for b in byte_array:
            xor = b ^ 150
            asc = chr(xor)
            out += asc
        return out

print(decode(byte_array))
```