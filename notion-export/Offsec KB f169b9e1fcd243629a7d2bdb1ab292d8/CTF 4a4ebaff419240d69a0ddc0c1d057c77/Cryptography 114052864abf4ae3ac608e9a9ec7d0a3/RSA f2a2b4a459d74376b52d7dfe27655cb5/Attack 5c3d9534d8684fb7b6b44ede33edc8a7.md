# Attack

## calculate decryption key (d) given e,p,q

```python
from gmpy2 import *
d = invert(e, (p-1) * (q-1))
```

## factorization with primefac module

```python
import primefac
n = 0x8d9424ddbf9fff7636d98abc25af7fde87e719dc3ceee86ca441b079e167cc22ff283f1a8671263c2e5ebd383ca3255e903b37ebca9961fd8a657cb987ef1e709866acc457995bfc7a6d4be7e88b9ee03a9872329e05cb7eb849d61e4bb44a25be8bd42f19f13a9417bfab73ba616b7c05865640682dc685890bbce8c20c65175f322b5b27788fede4f6704c6cb7b2d2d9439fad50f8b79ffab0b790591ae7f43bd0316565b097b9361d3beb88b6ef569d05af75d655b5133dc59a24c86d147a5eb5311344a66791f03a3da797effd600aa61564ce4ffd81f70bfedf12ca7857b9ac781a4823f6c1a08f1e86f8fe0e1eb3eb6ac71b63e4b03ba841c8588f6df1

q = primefac.pollard_pm1(n)
p = n//q

assert n == p*q
```

## factorization with custom Pollard’s p-1 function

```python
from gmpy2 import *
def Pollards_p_1(N):
    a = 2
    n = 2
    while True:
        a = pow(a,n,N)
        res = gcd(a-1,N)
        if res != 1 and res != N:
            print('n = {a}'.format(a=n))
            print('p = {a}'.format(a=res))
            return res
        n += 1

q = Pollards_p_1(n)
p = n//q
assert n == p*q
```

## RSA Fermat Attack

```python
from sympy.ntheory.primetest import is_square
from Crypto.Util.number import long_to_bytes,inverse
import sympy
def fermat(n):
    a = int(sympy.sqrt(n)) # Fermat's Algorithm
    b = (a*a) - n
    while not is_square(b):
        a += 1
        b = (a*a) - n
    else:
        p = int(a - (sympy.sqrt(b)))
        q = n//p
        if p * q == n:
            return p,q
        else:
            return "No Luck"

n =
e =65537
c =

phi = (p-1)*(q-1)
d = inverse(e,phi)
m = pow(c,d,n)
print(long_to_bytes(m).decode('ascii'))
```

## RSA Common Modulus Attack

Used on "Common Mistake" during HTB Cyber Santa

### Conditions

- 2 messages (n,e,ct) with common n (n,e1,e2,ct1,ct2)
- gcd(e1,e2) == 1
- gcd(ct2,n) == 1
- gcd(ct1,n) == 1

### Attack Script

```python
import codecs
from Crypto.Util.number import inverse
from math import gcd
# n common to both messages
n = 0xa96e6f96f6aedd5f9f6a169229f11b6fab589bf6361c5268f8217b7fad96708cfbee7857573ac606d7569b44b02afcfcfdd93c21838af933366de22a6116a2a3dee1c0015457c4935991d97014804d3d3e0d2be03ad42f675f20f41ea2afbb70c0e2a79b49789131c2f28fe8214b4506db353a9a8093dc7779ec847c2bea690e653d388e2faff459e24738cd3659d9ede795e0d1f8821fd5b49224cb47ae66f9ae3c58fa66db5ea9f73d7b741939048a242e91224f98daf0641e8a8ff19b58fb8c49b1a5abb059f44249dfd611515115a144cc7c2ca29357af46a9dc1800ae9330778ff1b7a8e45321147453cf17ef3a2111ad33bfeba2b62a047fa6a7af0eef

# message 1
e1 = 0x10001
ct1 = 0x55cfe232610aa54dffcfb346117f0a38c77a33a2c67addf7a0368c93ec5c3e1baec9d3fe35a123960edc2cbdc238f332507b044d5dee1110f49311efc55a2efd3cf041bfb27130c2266e8dc61e5b99f275665823f584bc6139be4c153cdcf153bf4247fb3f57283a53e8733f982d790a74e99a5b10429012bc865296f0d4f408f65ee02cf41879543460ffc79e84615cc2515ce9ba20fe5992b427e0bbec6681911a9e6c6bbc3ca36c9eb8923ef333fb7e02e82c7bfb65b80710d78372a55432a1442d75cad5b562209bed4f85245f0157a09ce10718bbcef2b294dffb3f00a5a804ed7ba4fb680eea86e366e4f0b0a6d804e61a3b9d57afb92ecb147a769874
c1 = pow(ct1,e1,n)

# message 2
e2 = 0x23
ct2 = 0x79834ce329453d3c4af06789e9dd654e43c16a85d8ba0dfa443aefe1ab4912a12a43b44f58f0b617662a459915e0c92a2429868a6b1d7aaaba500254c7eceba0a2df7144863f1889fab44122c9f355b74e3f357d17f0e693f261c0b9cefd07ca3d1b36563a8a8c985e211f9954ce07d4f75db40ce96feb6c91211a9ff9c0a21cad6c5090acf48bfd88042ad3c243850ad3afd6c33dd343c793c0fa2f98b4eabea399409c1966013a884368fc92310ebcb3be81d3702b936e7e883eeb94c2ebb0f9e5e6d3978c1f1f9c5a10e23a9d3252daac87f9bb748c961d3d361cc7dacb9da38ab8f2a1595d7a2eba5dce5abee659ad91a15b553d6e32d8118d1123859208
c2 = pow(ct2,e2,n)

## Common modulus attack
## Conditions
assert gcd(e1,e2) == 1
assert gcd(ct2,n) == 1
s1 = inverse(e1, e2)
s2 = (gcd(e1,e2) - e1*s1)//e2
temp = inverse(ct2,n)
m1 = pow(ct1,s1,n)
m2 = pow(temp, -s2, n)
print(long_to_bytes((m1*m2) % n))
b'HTB{c0mm0n_m0d_4774ck_15_4n07h3r_cl4ss1c}'

```