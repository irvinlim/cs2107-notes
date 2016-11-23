# CS2107 Lecture 4

## Cipher schemes
- **Stream ciphers** convert one symbol of plaintext directly into a symbol of ciphertext.
  - e.g. Monoalphabetic substitution ciphers
- **Block ciphers** encrypt a group of plaintext symbols as one block.
  - e.g. DES, AES

### Block cipher modes of operation
![](https://upload.wikimedia.org/wikipedia/commons/thumb/d/d6/ECB_encryption.svg/601px-ECB_encryption.svg.png)
- **Electronic Codebook (ECB)**: Each block is encrypted separately. Simplest mode of encryption, as it retains some easily retrievable information about the overall message.

![](https://upload.wikimedia.org/wikipedia/commons/thumb/8/80/CBC_encryption.svg/601px-CBC_encryption.svg.png)
- **Cipher Block Chaining (CBC)**: Each block of plaintext is XORed with the previous ciphertext block before being encrypted.

![](https://upload.wikimedia.org/wikipedia/commons/thumb/9/9d/CFB_encryption.svg/601px-CFB_encryption.svg.png)
- **Cipher Feedback (CFB)**: Similar to CBC, but the previous ciphertext is encrypted and XORed with the plaintext.

## Avalanche effect
The avalanche effect is the desirable property of cryptographic algorithms, typically block ciphers and cryptographic hash functions wherein if when an **input is changed slightly** (for example, flipping a single bit) the **output changes significantly** (e.g., half the output bits flip). 

The **strict avalanche criterion (SAC)** is a formalization of the avalanche effect. It is satisfied if, whenever a single input bit is complemented, each of the output bits changes with a 50% probability.

The **bit independence criterion (BIC)** states that output bits j and k should change independently when any single input bit i is inverted, for all i, j and k.

*See more: [https://en.wikipedia.org/wiki/Avalanche_effect](Wikipedia)*

## Data Encryption Standard (DES)
- **Block cipher**: Operates on 64-bit blocks
- Uses 64-bit symmetric keys
  - 56 bits is for the actual key, 8 bits are for error detection (found at every 8th bit)

### Encryption scheme
<img src="http://www.srcf.ucam.org/~bgr25/cipher/images/des.png" width="450">

1. Given a 64-bit block from the plaintext `IP` (initial permutation), split into 2x 32-bit blocks L<sub>0</sub> and R<sub>0</sub> respectively.
2. Given the 64-bit key, 56 bits are chosen, and split into 2x 28-bit halves, termed KL<sub>0</sub> and KR<sub>0</sub> respectively.
3. Repeat for 16 rounds, given L<sub>i</sub>, R<sub>i</sub> where `i` is the round number:
  1. Taking the key halves, rotate each KL<sub>i</sub> and KR<sub>i</sub> by a fixed number of bits.
  2. From the 56 bits combined from KL<sub>i</sub> and KR<sub>i</sub>, 48 bits are chosen, which is set to K<sub>i+1</sub>.
  3. Let:
    - L<sub>i+1</sub> = R<sub>i</sub>
    - R<sub>i+1</sub> = L<sub>i</sub> ⊕ F(R<sub>i</sub>, K<sub>i+1</sub>)  
      (where F is the [Feistel function](https://en.wikipedia.org/wiki/Data_Encryption_Standard#The_Feistel_.28F.29_function))
  4. Repeat.
4. At the end, we end up with L<sub>16</sub> and R<sub>16</sub>, which are combined to form the encrypted 64-bit block `FP` (final permutation).

After each round, the number of differing bits tends to approximately 32 bits out of 64 (following the strict avalanche criterion of 50%).

### 2DES
Suppose we use 2x 56-bit DES keys to encrypt a plaintext. However, the keyspace is not 2<sup>112</sup>, due to the meet-in-the-middle attack.

#### Meet-in-the-middle
1. Suppose the attacker has a plaintext and ciphertext pair `(p, c)`.
2. Compute a table of `E(k, p)` for each of the possible 2<sup>56</sup> keys.
3. Compute `D(k, c)` for each of the 2<sup>56</sup> keys.
4. For each match in the two tables, we have found a possible key.

This takes 2<sup>56</sup> × 2 = 2<sup>57</sup> operations, as compared to 2<sup>112</sup> as originally imagined.

### 3DES
Variant of the above, but resistant to meet-in-the-middle attack. Uses a method called 3DES-EDE (Encrypt-Decrypt-Encrypt). Has a proper keyspace of 168 bits of each key used is different.

## Advanced Encryption Standard (AES)
- Also known as Rijndael
- **Block cipher**: Operates of 128-bit blocks
- Has three variants: AES-128, AES-192 and AES-256, which use 128-, 192- and 256-bit keys.
  - AES has 10 rounds for 128-bit keys, 12 rounds for 192-bit keys, and 14 rounds for 256-bit keys.