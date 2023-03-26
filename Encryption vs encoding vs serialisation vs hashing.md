## Encryption vs encoding vs serialisation vs hashing
Encryption, encoding, serialisation, and hashing are all techniques used to transform data, but they serve different purposes and have different properties. Here are the differences:

## Encryption
Encryption is the process of converting plain text into cipher-text using a secret key or algorithm. Encryption is used to protect sensitive information, such as passwords or credit card numbers, by making it unreadable to unauthorised parties. The cipher-text can only be decrypted using the corresponding key or algorithm, which is kept secret. Encryption provides confidentiality, but not integrity or authenticity.

![image](https://user-images.githubusercontent.com/7569031/227779032-cc53a85b-fc46-45a8-99ea-7a8c6cd0642a.png)
> Photo by Towfiqu barbhuiya on Unsplash

Suppose you want to send a confidential email to a colleague. You could encrypt the email using a symmetric encryption algorithm like AES and a secret key, so that only your colleague with the corresponding key can read the email. If someone intercepts the email, they won’t be able to read its contents without the key.

```python
import cryptography.fernet

# Generate a secret key
key = cryptography.fernet.Fernet.generate_key()

# Initialize a Fernet object with the key
f = cryptography.fernet.Fernet(key)

# Encrypt a message
plaintext = b"Hello, world!"
ciphertext = f.encrypt(plaintext)

# Decrypt the message
decrypted_plaintext = f.decrypt(ciphertext)

print("Plaintext:", plaintext)
print("Ciphertext:", ciphertext)
print("Decrypted plaintext:", decrypted_plaintext)
```

This script uses the cryptography library to generate a secret key, initialise a Fernet encryption object with the key, encrypt a message, and then decrypt the cipher-text back into the original plaintext.

## Encoding
Encoding is the process of converting data from one format to another, usually to make it easier to transmit or store. Encoding is typically reversible, meaning that the original data can be reconstructed from the encoded data using the same algorithm. Examples of encoding include ASCII, UTF-8, and base64. Encoding does not provide security or confidentiality.

Suppose you have a binary image file that you want to transmit over a text-based protocol, like email or HTTP. You could encode the binary data into a text-based format, like base64, so that it can be transmitted without being corrupted. The recipient can then decode the base64 data back into the original binary image file.

```python
import base64

# Encode binary data
binary_data = b"\x00\x01\x02\x03"
text_data = base64.b64encode(binary_data)

# Decode text data
decoded_binary_data = base64.b64decode(text_data)

print("Binary data:", binary_data)
print("Text data:", text_data)
print("Decoded binary data:", decoded_binary_data)
```

This script uses the built-in base64 module to encode binary data into a text-based format, and then decode the text data back into the original binary data.

## Serialisation
Serialisation is the process of converting complex data structures, such as objects or arrays, into a format that can be stored or transmitted as a stream of bytes. Serialisation allows data to be easily transferred between different programming languages or platforms. Examples of serialisation formats include JSON, XML, and Protocol Buffers.

Suppose you have a complex data structure, like a list of dictionaries in Python, that you want to save to disk or transmit over a network. You could serialise the data using a format like JSON, which converts the data into a string of text that can be easily saved or transmitted. The recipient can then deserialize the JSON back into the original data structure.

```python
import json

# Serialize complex data
data = [{"name": "Alice", "age": 30}, {"name": "Bob", "age": 40}]
serialized_data = json.dumps(data)

# Deserialize serialized data
deserialized_data = json.loads(serialized_data)

print("Original data:", data)
print("Serialized data:", serialized_data)
print("Deserialized data:", deserialized_data)
```

This script uses the built-in json module to serialise a list of dictionaries into a JSON string, and then deserialise the JSON string back into the original data.

## Hashing

![image](https://user-images.githubusercontent.com/7569031/227778986-db8cb953-a3c2-46af-a4e4-593b3d22401e.png)
> Photo by Markus Spiske on Unsplash

Hashing is the process of converting data of arbitrary size into a fixed-size hash value, typically using a one-way cryptographic hash function. Hashing is used to verify the integrity of data by ensuring that any changes to the data result in a different hash value. Hashing is also used for password storage, as the hash value of a password can be stored instead of the password itself. However, hashing is not reversible, meaning that the original data cannot be reconstructed from the hash value.

Suppose you want to store user passwords securely in a database. Instead of storing the passwords directly, you could hash them using a one-way cryptographic hash function like SHA-256. When a user logs in, you can hash their entered password and compare it to the stored hash to verify if it’s correct. This way, even if an attacker gains access to the database, they won’t be able to recover the original passwords.

```python
import hashlib

# Hash a password
password = "mysecretpassword"
salt = "somesalt"
hashed_password = hashlib.sha256((password + salt).encode()).hexdigest()

# Verify a password
entered_password = "mysecretpassword"
entered_hashed_password = hashlib.sha256((entered_password + salt).encode()).hexdigest()

if entered_hashed_password == hashed_password:
    print("Password is correct")
else:
    print("Password is incorrect")
```

This script uses the built-in hashlib module to hash a password using SHA-256, with the addition of a salt to make the hash more secure. It also shows how to verify if a entered password matches the stored hashed password by hashing the entered password with the same salt and comparing the resulting hash values.

In summary, encryption provides confidentiality, encoding provides compatibility, serialisation provides portability, and hashing provides integrity.

Imported from my article [Medium: Encryption vs encoding vs serialisation vs hashing ](https://medium.com/design-bootcamp/encryption-vs-encoding-vs-serialisation-vs-hashing-38827ecec89e)

