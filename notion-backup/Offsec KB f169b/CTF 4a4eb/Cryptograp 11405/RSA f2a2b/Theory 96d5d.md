# Theory

# Public key

public key is composed of a public modulo (n) and a public exponent (e)

## public modulus (n)

n is composed of 2 prime numbers (p,q)

```
p = 11 # prime number
q = 13 # prime number
n = p*q = 11 * 13 = 143
```

## phi

phi represents the greatest common divider of all number between 1 and n (n included)

Short way of finding phi :

```
phi = (p-1) * (q-1)
phi = (11-1) * (13-1) = 10 * 12 = 120
```

## encryption key (e)

e must be a number that is less than phi and co-prime to n and phi

```
2<e<phi
gcd(n,e) == 1 == gcd(phi,e)

# for the example
e = 7
```

## the key

```
p = 11
q = 13
n = 143
phi = 120
e = 7

publickey = (e,n) = (7, 143)
```

# Private Key

## decryption key (d)

maths: d * e % phi = 1

```
# Given:
phi = 120
e = 7
# There's a function to calculate possible d
# Possible d:
# 223, 343, 463, 583, 703, 823, 943, 1063

# for the example
d = 223
```

## recap

```
private_key = [d, n] = (223, 143)
```

# Encryption

ciphertext (ct) = plaintext (pt) ^ e % n

ct = pt ^ e % n

```
pt = 15
ct = 15 ^ 7 % 143 = 115
```

# Decryption

decrypted text (dec) = ciphertext (ct) ^ d % n

dec = pt ^ d % n

```
ct = 115
dec = 115 ^ 223 % 143 = 15
```