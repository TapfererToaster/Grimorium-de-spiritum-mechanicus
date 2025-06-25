Cryptography is used to protect confidentiality, integrity and authenticity.
You use cryptography daily, some scenarios are:
- When you log into a website, your credentials are encrypted and sent to the server so no one can retrieve them by snooping on your session.
- When you connect over SSH, your SSH client and the server establish an encrypted tunnel so no one can eavesdrop on your session.
- When conducting online banking, your browser checks the remote server's  certificate to confirm that you are communicating with your bank's server and not an attacker's.
- When you download a file, you can use hash functions to check if the file has been altered during transit.

# Basic Terms
![[Cryptography basic.png]]
- **Plaintext**: 
  the original, readable data (document, image, media file, binary data, ...) before encryption
- **Ciphertext**:
  data after encryption; unreadable and you should not get any information about the plaintext from the ciphertext
- **Cipher**:
  an algorithm or method to convert plaintext into ciphertext and back again; the choice of cipher is diclosed 
- **Key**:
  a string of bits the cipher uses to encrypt or decrypt data
- **Encryption**:
  process of converting plaintext into ciphertext usind a cipher and a key
- **Decryption**:
  process of converting a ciphertext into plaintext; recovering the plaintext without the key should be impossible.
# Symmetric Encryption
![[Symmetric Cryptography.png]]

*Symmetric encryption*, also called *symmetric cryptography* or *private key cryptography*, uses the same key to encrypt and decrypt data. Keeping the key secret is a must, furthermore, communicating the key to the intended parties can be challenging as it requires a secure communication channel. 
Examples:
- **Data Encryption Standard (DES)**:
  adopted as a standard in 1977; uses a 56-bit key; replaces with 3DES
- **3DES**:
  DES is applied three times; key size is 168 bits, though effecitve security is 112 bits; deprecated in 2019
- **Advanced Encryption Standard (AES)**:
  was adopted as a standard in 2001 and its key size can be 128, 192 or 256 bits long.

# Asymmetric Encryption
![[Asymmetric Cryptography.png]]
*Asymmetric encryption* uses a pair of keys, one to encrypt and the other to decrypt the data. The two keys are referred to as *public* and *private* key. 

> [!important]
> The private key must be kept private, meaning it should never be shared.

Examples are *RSA*, *Diffie-Hellman* and *Elliptic Curve cryptography (ECC)*. Asymmetric encryption tends to be slower and use larger keys than symmetric encryption. 
For example, RSA uses 2048, 3072 and 4096 bit keys, while 2048 is the recommended minimum key size. Diffie-Hellman also has a recommended minimum key size of 2048 bits but uses 3072 and 4096 bit keys for enhanced security. ECC can achieve equivalent security with shorter keys, for example, with a 256-bit key it provides a level of security comparable to a 3072-bit RSA key.

The basis for asymmetric encryption are a group of mathematical problems that are easy to compute in one direction but extremely difficult to reverse.

## Analogy
When using asymmetric encryption to two parties must negotiate and agree on symmetric encryption ciphers and keys. How do they agree on a key without transmitting the key for people to see?

Imagine you want to share a secret letter with a friend who is far away. How can you send it to him, while also making sure that no one could read or tamper with it during transit?  
The answer is to ask your friend to send a lock. Now you can place the letter inside a box and lock it with the lock your friend send you and send the box to him. No one except him can open the box and open the letter.

| Analogy       | Cryptographic System                |
| ------------- | ----------------------------------- |
| Secret letter | Symmetric Encryption Cipher and Key |
| Lock          | Public Key                          |
| Key           | Private Key                         |
## RSA
RSA is based on the mathematically difficult problem of factoring a large number. Multiplying two large prime numbers is a straightforward operation; however, finding the factors of a huge number takes much more computing power.
Lets look at the RSA algorithm:
1. Bob chooses two prime numbers: *p* = 157 and *q* = 199. He calculates *n* = *p* * *q* = 31243.
2. With _ϕ_(_n_) = _n_ − _p_ − _q_ + 1 = 31243 − 157 − 199 + 1 = 30888, 
   - Bob selects *e* = 163, such that e is relatively prime to _ϕ_(_n_)
   - He selects *d* = 379, where *e* * *d* = 1 mod _ϕ_(_n_)
     i.e. *e* * *d* = 163 * 379 = 61777; 61777 mod 30888 = 1
   - The public key is (*n*,*e*), i.e. (31243,163) 
     and the private key is $(*n*,*d*) , i.e. (31243,379)
1. The value they want to encrypt is *x* = 13
   - Alice would calculate and send *y* =  $x^{e}$ mod *n* = $13^{163}$  =
   - Alice would calculate and send 
     $$y=x^{n}\text{ mod } n$$
     $$= 13^{163} \text{ mod } 31243 = 16341$$
1. Bob will decrypt the received value by calculating
   $$x = y^d \text{ mod } n$$
   $$= 16341^379 \text{ mod } 31243 = 13$$

> [!summary] Main Variables
> The variables for RSA are:
> - *p* and *q* are large prime numbers
> - *n* is the product of *p* and *q*
> - The public key is *n* and *e*
> - The private key is *n* and *d*
> - *m* is used to represent the original message, i.e., plaintext
> - *c* represents the encrypted text, i.e., ciphertext

### In CTFs
RSA and the math behind it often come up in CTFs. Tools or defeating RSA are for example:
- [RsaCtfTool](https://github.com/Ganapati/RsaCtfTool)
- [rsatool](https://github.com/ius/rsatool)
## Diffie-Hellman Key Exchange
![[Diffie-Hellman Key Exchange.png]]
A key exchange aims to establish a shared key between two parties over a insecure communication channel without requiring a pre-existing shared key and without an observer being able to get the key.
The process for the key exchange is:
1. Alice and Bob agree on public variables:
   - a large prime *p* 
   - a generator *g*, where 0 < *g* < *p*
1. Each party chooses a private integer, representing a *private key*:
   - Alice chooses *a*
   - Bob chooses *b*
1. Each party calculates their *public key*, using their *private key* and the *public variables*:
   - Alice calculates: $A = g^{a} \text{ mod } p$
   - Bob calculates: $B= g^{b} \text{ mod } p$
1. Alice and Bob send the keys to each other, called *key exchange*
2. Alice and Bob calculate the *shared secret* using the *public key* and their own *private key*:
   - Alice calculates: $B^{a} \text{ mod } p$
   - Bob calculates: $A^{b}\text{ mod }p$
     
   Both calculations yield the same result: $g^{ab} \text{ mod } p = \text{shared secret key}$ 

## Digital Signatures and Certificates
![[Digital Signature.png]]