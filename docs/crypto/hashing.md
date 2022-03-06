# Hashing
Cryptographic hashes are used in day-day life like in digital signatures, message authentication codes, manipulation detection, fingerprints, checksums (message integrity check), hash tables, password storage and much more. They are also used in sending messages over network for security or storing messages in databases.

[Definition](https://www.investopedia.com/terms/b/business-intelligence-bi.asp)

Many encryption and hashing algorithms use the follow functions:

- `encode()` : Converts the string into bytes to be acceptable by hash function.
- `digest()` : Returns the encoded data in byte format.
- `hexdigest()` : Returns the encoded data in hexadecimal format.


---
## Hashing Algorithms

### Python hash()

```python
int_val = 4
str_val = 'GeeksforGeeks'
flt_val = 24.56
 
# Printing the hash values.
# Notice Integer value doesn't change
# You'l have answer later in article.
print("The integer hash value is : " + str(hash(int_val)))
print("The string hash value is : " + str(hash(str_val)))
print("The float hash value is : " + str(hash(flt_val)))
```
[More Info](https://www.geeksforgeeks.org/python-hash-method/)

### MD5 Hash 


```python
import hashlib
# note the string is in byte format
result = hashlib.md5(b'information to hash')

# alternatively...
info_to_hash = "information to hash"
result = hashlib.md5(info_to_hash.encode())

# printing the equivalent byte value.
print(result.digest())
# b'\xf1\xe0ix~\xcetS\x1d\x11%Y\x94\\hq'

# printing the equivalent hexadecimal value.
print(result.hexdigest())
# f1e069787ece74531d112559945c6871
```

### SHA1 Hash 



### SHA256 Hash 

```python
import hashlib
  
# initializing string
str = "GeeksforGeeks"

# then sending to SHA1()
result = hashlib.sha1(str.encode())
# printing the equivalent hexadecimal value.
print(result.hexdigest())

# then sending to SHA256()
result = hashlib.sha256(str.encode())
print(result.hexdigest())

# then sending to SHA512()
result = hashlib.sha512(str.encode())
print(result.hexdigest())

# Of the same inputs, each algorithm produces these outputs
# SHA1: 4175a37afd561152fb60c305d4fa6026b7e79856
# SHA256: f6071725e7ddeb434fb6b32b8ec4a2b14dd7db0d785347b2fb48f9975126178f
# SHA512: 0d8fb9370a5bf7b892be4865cdf8b658a82209624e33ed71cae353b0df254a75db63d1baa35ad99f26f1b399c31f3c666a7fc67ecef3bdcdb7d60e8ada90b722
```

[More Info](https://www.geeksforgeeks.org/sha-in-python/)

