# Scripts

# Functions

## generate strong prime (p,q)

```python
# return a random prime number between low and high
# not optimized
def prime_finder(low,high):
    test_number = random.randrange(low, high)
    for i in range(2, test_number):
        if test_number % i == 0:
            return prime_finder(low,high)
    return test_number
```

## generate possible encryption key (e)

```python
# returns all the possible encryption keys, given n and phi
def generate_enckey(n,phi):
    encKeys = []
    for i in range(2, phi):
        if gcd(i, phi) == 1 and gcd(i, n) == 1:
            encKeys.append(i)
        if len(encKeys) >= 100:
            break
    return pub_keys

e = random.choice(generate_enckey(n,phi))
```

## generate possible decryption key (d)

```python
# returns all the possible decryption keys, given e and phi
def generate_deckey(e,phi):
    decKeys = []
    i = 2
    while len(decKeys) < 5:
        if i * e % phi == 1:
            decKeys.append(i)
        i += 1
    return decKeys

d = random.choice(generate_deckey(e,phi))
```

## check if 2 numbers are co-prime

```python
# return true if both numbers are co-prime
def coprime(a, b):
    if ( gcd(a, b) == 1):
        return True
    else:
        return False
```

## calculate greatest common divider for 2 numbers

```python
def gcd(a, b):
    if b == 0:
        return a
    return gcd(b, a%b)
```