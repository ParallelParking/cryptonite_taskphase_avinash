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
- Find the product of our numbers, so here that's 14 (you'll notice, this becomes part of the ordered pair in both our public and private keys)
- We have to calculate the 'Phi function' of this number 14. The output of this function is the count of numbers that are coprime to the input and less than it. In the case of 14, these numbers are 1,3,5,9,11,13. Thus, there are 6 of them and 6 is the output of the function.
- Practically, the product of our primes will be several magnitudes larger, so there is a much easier way to calculate count of coprimes. Say our original prime numbers, 2 & 7 are p & q. The count of coprimes of the product pq for any p & q is given by (p-1)(q-1). eg. 1\*6=6.

&#934

