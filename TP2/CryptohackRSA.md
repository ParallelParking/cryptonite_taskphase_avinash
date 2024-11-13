# RSA

It is a 'public key cryptosystem', wherein the encryption key is public and distinct from the decryption key, which is kept secret. An RSA user can create and publish a public key based on two large prime numbers along with an auxiliary value. The prime numbers are kepy secret. These messages may be encrypted by anyone, via public key, but may only be decrypted by someone knowing the private key.

The security of RSA relies on the difficulty of factoring the product of 2 large prime numbers. It is an open question as to whether factoring is actually difficult, but there is no known system to defeat RSA.

It is a relatively slow algorithm, because of this it is not commonly used to directly encrypt user data. More often, it is used to transmit shared keys for symmetric-key cryptography, which is then used for bulk encryption-decryption.

By this essentially the message itself is encrypted via a fast algorithm like AES, then the symmetric key to AES is encrypted via RSA, so interception of the key doesn't result in interception of the message.

Further, a message encrypted by the private key can be decrypted via the public key, so this can be used to verify a sender (a public key that successfully decrypts the message means the message MUST have come from the sender), so for the best of both worlds, a message encrypted via RSA is encrypted with both the recipient's public key and the sender's private key, and decrypted vice versa.

FUN NOTE: you can see the public key of websites you're visiting by hitting the lock icon, they are LONG long, like hundreds of digits long.

https://en.wikipedia.org/wiki/RSA_(cryptosystem)

https://www.youtube.com/watch?v=ZPXVSJnDA_A

https://www.youtube.com/watch?v=4zahvcJ9glg

## STEPS TO DOING RSA:

- My public key: an ordered pair of numbers, say (5,14)
- I want to encrypt the letter 'B'
- We need to encode our text data as a number. Let's say 'B' -> 2
- So we take the encoded value, 2, raise it to the power of 5, and take it modulo the second number of set, 14, i.e. 2^5 (mod 14)
- This encrypts to 4 <- this is our 'Cyphertext'. This in turn encodes to 'D'
- My private key: an ordered pair of numbers, the second is the same as public key but the first is different! Let's say (11, 14). The first number is the actual key.
- Do the same thing again, but with encoded message, so 'D' -> 4, so 4^11 (mod 14) = 2 -> 'B', our original message!
- How did we get 11 for the private key!? Why does this work!?

## GENERATING OUR KEYS

- Pick 2 prime numbers, say 2 & 7
- Find the product of our numbers, so here that's 14 (you'll notice, this becomes part of the ordered pair in both our public and private keys). This number is referred to as 'N'
- We have to calculate the 'Phi function' of this number 14. The output of this function is the count of numbers that are coprime to the input and less than it. In the case of 14, these numbers are 1,3,5,9,11,13. Thus, there are 6 of them and 6 is the output of the function. I will refer to the output of the Phi function with an F (Can't get Greek letters to work lol)
- Practically, the product of our primes will be several magnitudes larger, so there is a much easier way to calculate count of coprimes. Say our original prime numbers, 2 & 7 are p & q. The count of coprimes of the product pq for any p & q is given by (p-1)(q-1). eg. 1\*6=6. 
- Now, to choose the first number of our public number pair (we're calling this 'e'), it must satisfy the following conditions: 
	- 1 < e < F
	- e is coprime with both N and F
- So here, e can be by first criteria, 2,3,4,5. However, 2,3,4 are all factors of either 6 or 14. Therefore, e = 5
- Finally, to choose the first number of our private key (this is 'd'), it must satisfy the condition de (mod F) = 1. For example, d = 4, or d = 11.
- We don't want to pick d = 4 as it makes hacking the encryption too easy.

NOTE: a common e is 65537

NOTE: the Phi function is termed the 'Euler totient'

NOTE: calculating d is actually the 'modular multiplicative inverse, i.e. d is e^-1 (mod F)
## Challenges:

1. 101^17 (mod 22663) = 19906
2. 12^65537 (mod 17\*23) = 301
3. Code:
```py
>>> p = 857504083339712752489993810777
>>> q = 1029224947942998075080348647219
>>> (p - 1) * (q - 1)
882564595536224140639625987657529300394956519977044270821168
```
4. pow(65537, -1, (p - 1) * (q - 1))
5. pow(c,d,p\*q)
6. Code:
```py
s = pow(bytes_to_long(sha256("crypto{Immut4ble_m3ssag1ng}".encode('utf-8')).digest()),d,n)
print(s)
```

Crossed wires:

```py
from Cryptodome.Util import number

N = ...

friend_keys = [...]
c = ...

p = ...
q = ...

phi = (q-1)*(p-1)

for key in friend_keys[::-1]:
    d = number.inverse(key[1], phi)
    c = pow(c, d, N)
print(number.long_to_bytes(c))
```

Square eyes:
```py
N = ...
p = int(math.isqrt(N))
e = ...
c = ...
phi = (p - 1) * p
d = inverse(e, phi)
m = pow(c, d, N)
m = long_to_bytes(m)
print("Decrypted message:", m)
```


