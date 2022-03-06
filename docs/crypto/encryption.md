# Encryption

Encryption is a way of scrambling data so that only authorized parties can understand the information. In technical terms, it is the process of converting human-readable plaintext to incomprehensible text, also known as ciphertext. In simpler terms, encryption takes readable data and alters it so that it appears random. Encryption requires the use of a cryptographic key: a set of mathematical values that both the sender and the recipient of an encrypted message agree on.

[Definition](https://www.cloudflare.com/learning/ssl/what-is-encryption/)

---
## Symmetric Encryption
In symmetric-key encryption, the data is encoded and decoded with the same key. This is the easiest way of encryption, but also less secure. The receiver needs the key for decryption, so a safe way need for transferring keys. Anyone with the key can read the data in the middle.
### Fernet

Steps

1. Import Fernet
2. Then generate an encryption key, that can be used for encryption and decryption.
3. Convert the string to byte string, so that it can be encrypted.
4. Instance the Fernet class with the encryption key.
5. Then encrypt the string with Fernet instance.

6. Then it can be decrypted with Fernet class instance and it should be instanced with the same key used for encryption.

```python
from cryptography.fernet import Fernet

message = "Encrypt this message"

key = Fernet.generate_key()
fernet = Fernet(key)
# string must must be encoded to byte string before encryption
encMessage = fernet.encrypt(message.encode())

# Decrypting
decMessage = fernet.decrypt(encMessage).decode()
```
[More Info](https://www.geeksforgeeks.org/how-to-encrypt-and-decrypt-strings-in-python/)

---
## Asymmetric Encryption
In Asymmetric-key Encryption, we use two keys a public key and private key. The public key is used to encrypt the data and the private key is used to decrypt the data. By the name, the public key can be public (can be sent to anyone who needs to send data). No one has your private key, so no one the middle can read your data.

### RSA 

Steps:

1. Import rsa library
2. Generate public and private keys with rsa.newkeys() method.
3. Encode the string to byte string.
4. Then encrypt the byte string with the public key.
5. Then the encrypted string can be decrypted with the private key.
6. The public key can only be used for encryption and the private can only be used for decryption.

```python
import rsa

publicKey, privateKey = rsa.newkeys(512)
message = "Encrypt this message"
# string must must be encoded to byte string before encryption
encMessage = rsa.encrypt(message.encode(), publicKey)
# Decrypting
decMessage = rsa.decrypt(encMessage, privateKey).decode()
```
[More Info](https://www.geeksforgeeks.org/how-to-encrypt-and-decrypt-strings-in-python/)

---
## Homomorphic Encryption

### Python-Paillier 

[Documentation](https://github.com/data61/python-paillier)

A Python 3 library implementing the Paillier Partially Homomorphic Encryption.

The homomorphic properties of the paillier crypto system are:

- Encrypted numbers can be multiplied by a non encrypted scalar.
- Encrypted numbers can be added together.
- Encrypted numbers can be added to non encrypted scalars.


---
## Elliptic Curve Cryptography

###  

[Documentation](https://blog.cloudflare.com/a-relatively-easy-to-understand-primer-on-elliptic-curve-cryptography/)

