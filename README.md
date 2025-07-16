# AES Encryption and Brute Force Attack Project

This project demonstrates AES-128 encryption/decryption implementation and brute force attack techniques for educational purposes. The code is designed for the CSC489 course homework assignment.

## Overview

The project consists of two main components:
1. **AES Encryption/Decryption Implementation** - Basic AES-128 encryption and decryption functions
2. **Brute Force Attack Demonstration** - Shows the feasibility and limitations of brute force attacks on AES keys

## Files

- `Encrypt and decrypt.ipynb` - Contains the basic AES encryption and decryption implementation
- `BruteForce Decryption.ipynb` - Demonstrates brute force attacks on AES keys with varying numbers of missing bytes

## Requirements

```python
pip install pycryptodome
```

Required libraries:
- `pycryptodome` - For AES encryption/decryption
- `base64` - For encoding/decoding ciphertext
- `itertools` - For generating key combinations
- `time` - For measuring attack duration

## Features

### AES Encryption/Decryption
- **Algorithm**: AES-128 in ECB mode
- **Key Size**: 16 bytes (128 bits)
- **Padding**: PKCS7 padding for block alignment
- **Encoding**: Base64 encoding for ciphertext representation

### Brute Force Attack Scenarios
The project demonstrates three different brute force scenarios:

1. **1 Missing Byte** (8 bits) - 256 possible combinations
2. **4 Missing Bytes** (32 bits) - 4,294,967,296 combinations
3. **8 Missing Bytes** (64 bits) - 18,446,744,073,709,551,616 combinations

## Usage

### Basic Encryption/Decryption

```python
from Crypto.Cipher import AES
from Crypto.Random import get_random_bytes
import base64

# Generate a random key
key = get_random_bytes(16)

# Encrypt plaintext
plaintext = "Your message here"
encrypted_text = aes_encrypt(plaintext, key)

# Decrypt ciphertext
decrypted_text = aes_decrypt(encrypted_text, key)
```

### Brute Force Attack

```python
# For 1 missing byte
known_key_part = key[:-1]  # First 15 bytes
recovered_key = brute_force_missing_last_byte(ciphertext, known_key_part, plaintext)

# For 4 missing bytes
known_key_part = key[:-4]  # First 12 bytes
recovered_key = brute_force_missing_last_4_bytes(ciphertext, known_key_part, plaintext)
```

## Results and Analysis

### Successful Attacks
- **1 Missing Byte**: Successfully recovered in ~0.008 seconds
- **4 Missing Bytes**: Attack terminated after 1 hour (infeasible)
- **8 Missing Bytes**: Attack terminated after 2 hours (infeasible)

### Time Complexity Analysis
- **1 Byte**: 2^8 = 256 combinations (feasible)
- **4 Bytes**: 2^32 ≈ 4.3 billion combinations (computationally expensive)
- **8 Bytes**: 2^64 ≈ 1.8 × 10^19 combinations (practically impossible)

## Security Implications

This project demonstrates why:
1. **Full key security is crucial** - Even missing a few bytes makes brute force attacks exponentially harder
2. **AES-128 is secure** - With a full 128-bit key space, brute force attacks are computationally infeasible
3. **Key management is critical** - Partial key exposure significantly reduces security

## Educational Notes

- The code uses ECB mode for simplicity, but production systems should use more secure modes like CBC or GCM
- The brute force attacks shown are for educational purposes only
- Real-world AES implementations include additional security measures

## Code Structure

### Core Functions
- `pad()` - Implements PKCS7 padding
- `unpad()` - Removes PKCS7 padding
- `aes_encrypt()` - Encrypts plaintext using AES-128
- `aes_decrypt()` - Decrypts ciphertext using AES-128
- `brute_force_missing_last_byte()` - Brute force attack for 1 missing byte
- `brute_force_missing_last_4_bytes()` - Brute force attack for 4 missing bytes
- `brute_force_missing_last_8_bytes()` - Brute force attack for 8 missing bytes

## Sample Output

```
Generated Key (Hex): c01b36bf1c7401952a88e61fed97ff18
Original Text: This is a text to test the program...
Encrypted Text: NzH2bKnEl1IPISQuNqX5A9B2KN6bKiR9VKO+ywKH1A...
Known Key Part (Hex): c01b36bf1c7401952a88e61fed97ff
Brute-force successful! Missing byte: 0x18
Recovered Key: c01b36bf1c7401952a88e61fed97ff18
Time taken: 0.0080 seconds
```

## Limitations and Known Issues

1. **Error Handling**: Some UTF-8 decoding errors occur during brute force attempts
2. **Performance**: Attacks on 4+ missing bytes are not practically feasible
3. **Memory Usage**: Large key spaces may cause memory issues

## Disclaimer

This code is for educational purposes only. Do not use these techniques for unauthorized access or illegal activities. Always follow ethical guidelines and legal requirements when working with cryptographic systems.
