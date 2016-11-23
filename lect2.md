# CS2107 Lecture 2

## Terminology
- **Key size**: Number of bits needed to store the encoded text (e.g. 256-bit)
- **Keyspace**: Number of keys which encrypt differently
- **Useful keyspace**: Number of keys where `E(k, m) != m`

**Calculations:**
- KSize = ceil(log<sub>2</sub>(KSpace))
- KSpace ≤ 2<sup>KSize</sup>

## Monoalphabetic Substitution Ciphers
Each letter in the plaintext is mapped to another letter in the alphabet (one-one mapping).

### Properties
- **Keyspace**: 26! = 4.03291461e26
- **Useful keyspace**: 26! - 1

### Frequency Analysis
By studying the frequency of letters or groups of letters in a ciphertext, we can attempt to break the cipher.

| Letter | Frequency | Letter | Frequency | Letter | Frequency | Letter | Frequency |
|-------|----------|-------|----------|-------|----------|-------|----------|
| e     | 12.702%  | t     | 9.056%   | a     | 8.167%   | o     | 7.507%   |
| i     | 6.966%   | n     | 6.749%   | s     | 6.327%   | h     | 6.094%   |
| r     | 5.987%   | d     | 4.253%   | l     | 4.025%   | c     | 2.782%   |
| u     | 2.758%   | m     | 2.406%   | w     | 2.360%   | f     | 2.228%   |
| g     | 2.015%   | y     | 1.974%   | p     | 1.929%   | b     | 1.492%   |
| v     | 0.978%   | k     | 0.772%   | j     | 0.153%   | x     | 0.150%   |
| q     | 0.095%   | z     | 0.074%   |
 
## Rotation Ciphers
Each letter in the plaintext is replaced by a letter some fixed number of positions down the alphabet. It is a special case of a monoalphabetic substitution cipher.

- **Keyspace**: 26
- **Useful keyspace**: 25

```
S = Number of symbols (i.e. number of alphabets)
C = E(k, p) = (p + k) mod S
p = D(k, c) = (c - k) mod S
```

### Cæsar Cipher (Roman Alphabet)
- **Keyspace**: 23
- **Useful keyspace**: 22

*NB: The Roman alphabet has only 23 letters.*

### Union/Confederate Ciphers
- **Keyspace**: 26
- **Useful keyspace**: 25

### Playfair Cipher
Using a 5x5 matrix with unique alphabets (with I/J considered to be the same alphabet), choose a keyword with non-repeated letters and extract out the letters from the matrix, moving them to the start of the matrix.

| M | O | N | A   | R |
|---|---|---|-----|---|
| C | H | Y | B   | D |
| E | F | G | I/J | K |
| L | P | Q | S   | T |
| U | V | W | X   | Z |

**Encryption**:
- Let p<sub>i</sub> be the plaintext letter at position i, and c<sub>i</sub> be the ciphertext letter at position i.
- For each distinct pair of letters (p<sub>i</sub>, p<sub>i+1</sub>) for i ∈ [0, ceil(k/2)], *(add filler letter if necessary)*
  - If p<sub>i</sub> and p<sub>i+1</sub> are in the same column,
    - c<sub>i</sub> = the letter below p<sub>i</sub> (wrap around if needed)
    - c<sub>i+1</sub> = the letter below p<sub>i+1</sub> (wrap around if needed)
  - Else If p<sub>i</sub> and p<sub>i+1</sub> are in the same row,
    - c<sub>i</sub> = the letter to the right of p<sub>i</sub> (wrap around if needed)
    - c<sub>i+1</sub> = the letter to the right of p<sub>i+1</sub> (wrap around if needed)
  - Otherwise, p<sub>i</sub> and p<sub>i+1</sub> form a rectangle, so,
    - c<sub>i</sub> = the letter in the same row as p<sub>i</sub> and same column as p<sub>i+1</sub>
    - c<sub>i+1</sub> = the letter in the same row as p<sub>i+1</sub> and same column as p<sub>i</sub>

**Decryption**: Simply do the encryption procedure, but in opposite directions.

#### Properties
- **Keyspace**: 25!/5<sup>2</sup> = 24!
- **Useful keyspace**: 24! - 1

#### Frequency analysis
Can be broken with 26x26 = 676-symbol frequency analysis for each digram.


## Polyalphabetic Substitution Ciphers
Each letter in the plaintext is mapped to a combination of different letters (not one-one). We use a key to select which cipher is used for each letter of the plaintext.

### Vigenère Cipher (1553)
By using a tableau (below) and a repeated key (e.g. `BAD`):

|    | A  | B  | C  | D  | E  | F  | G  | H  | I  | J  | K  | L  | M  | N  | O  | P  | Q  | R  | S  | T  | U  | V  | W  | X  | Y  | Z  | 
|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----| 
| A  | A  | B  | C  | D  | E  | F  | G  | H  | I  | J  | K  | L  | M  | N  | O  | P  | Q  | R  | S  | T  | U  | V  | W  | X  | Y  | Z  | 
| B  | B  | C  | D  | E  | F  | G  | H  | I  | J  | K  | L  | M  | N  | O  | P  | Q  | R  | S  | T  | U  | V  | W  | X  | Y  | Z  | A  | 
| C  | C  | D  | E  | F  | G  | H  | I  | J  | K  | L  | M  | N  | O  | P  | Q  | R  | S  | T  | U  | V  | W  | X  | Y  | Z  | A  | B  | 
| D  | D  | E  | F  | G  | H  | I  | J  | K  | L  | M  | N  | O  | P  | Q  | R  | S  | T  | U  | V  | W  | X  | Y  | Z  | A  | B  | C  | 
| E  | E  | F  | G  | H  | I  | J  | K  | L  | M  | N  | O  | P  | Q  | R  | S  | T  | U  | V  | W  | X  | Y  | Z  | A  | B  | C  | D  | 
| F  | F  | G  | H  | I  | J  | K  | L  | M  | N  | O  | P  | Q  | R  | S  | T  | U  | V  | W  | X  | Y  | Z  | A  | B  | C  | D  | E  | 
| G  | G  | H  | I  | J  | K  | L  | M  | N  | O  | P  | Q  | R  | S  | T  | U  | V  | W  | X  | Y  | Z  | A  | B  | C  | D  | E  | F  | 
| H  | H  | I  | J  | K  | L  | M  | N  | O  | P  | Q  | R  | S  | T  | U  | V  | W  | X  | Y  | Z  | A  | B  | C  | D  | E  | F  | G  | 
| I  | I  | J  | K  | L  | M  | N  | O  | P  | Q  | R  | S  | T  | U  | V  | W  | X  | Y  | Z  | A  | B  | C  | D  | E  | F  | G  | H  | 
| J  | J  | K  | L  | M  | N  | O  | P  | Q  | R  | S  | T  | U  | V  | W  | X  | Y  | Z  | A  | B  | C  | D  | E  | F  | G  | H  | I  | 
| K  | K  | L  | M  | N  | O  | P  | Q  | R  | S  | T  | U  | V  | W  | X  | Y  | Z  | A  | B  | C  | D  | E  | F  | G  | H  | I  | J  | 
| L  | L  | M  | N  | O  | P  | Q  | R  | S  | T  | U  | V  | W  | X  | Y  | Z  | A  | B  | C  | D  | E  | F  | G  | H  | I  | J  | K  | 
| M  | M  | N  | O  | P  | Q  | R  | S  | T  | U  | V  | W  | X  | Y  | Z  | A  | B  | C  | D  | E  | F  | G  | H  | I  | J  | K  | L  | 
| N  | N  | O  | P  | Q  | R  | S  | T  | U  | V  | W  | X  | Y  | Z  | A  | B  | C  | D  | E  | F  | G  | H  | I  | J  | K  | L  | M  | 
| O  | O  | P  | Q  | R  | S  | T  | U  | V  | W  | X  | Y  | Z  | A  | B  | C  | D  | E  | F  | G  | H  | I  | J  | K  | L  | M  | N  | 
| P  | P  | Q  | R  | S  | T  | U  | V  | W  | X  | Y  | Z  | A  | B  | C  | D  | E  | F  | G  | H  | I  | J  | K  | L  | M  | N  | O  | 
| Q  | Q  | R  | S  | T  | U  | V  | W  | X  | Y  | Z  | A  | B  | C  | D  | E  | F  | G  | H  | I  | J  | K  | L  | M  | N  | O  | P  | 
| R  | R  | S  | T  | U  | V  | W  | X  | Y  | Z  | A  | B  | C  | D  | E  | F  | G  | H  | I  | J  | K  | L  | M  | N  | O  | P  | Q  | 
| S  | S  | T  | U  | V  | W  | X  | Y  | Z  | A  | B  | C  | D  | E  | F  | G  | H  | I  | J  | K  | L  | M  | N  | O  | P  | Q  | R  | 
| T  | T  | U  | V  | W  | X  | Y  | Z  | A  | B  | C  | D  | E  | F  | G  | H  | I  | J  | K  | L  | M  | N  | O  | P  | Q  | R  | S  | 
| U  | U  | V  | W  | X  | Y  | Z  | A  | B  | C  | D  | E  | F  | G  | H  | I  | J  | K  | L  | M  | N  | O  | P  | Q  | R  | S  | T  | 
| V  | V  | W  | X  | Y  | Z  | A  | B  | C  | D  | E  | F  | G  | H  | I  | J  | K  | L  | M  | N  | O  | P  | Q  | R  | S  | T  | U  | 
| W  | W  | X  | Y  | Z  | A  | B  | C  | D  | E  | F  | G  | H  | I  | J  | K  | L  | M  | N  | O  | P  | Q  | R  | S  | T  | U  | V  | 
| X  | X  | Y  | Z  | A  | B  | C  | D  | E  | F  | G  | H  | I  | J  | K  | L  | M  | N  | O  | P  | Q  | R  | S  | T  | U  | V  | W  | 
| Y  | Y  | Y  | Y  | Y  | Y  | Y  | Y  | Y  | Y  | Y  | Y  | Y  | Y  | Y  | Y  | Y  | Y  | Y  | Y  | Y  | Y  | Y  | Y  | Y  | Y  | Y  | 
| Z  | Z  | A  | B  | C  | D  | E  | F  | G  | H  | I  | J  | K  | L  | M  | N  | O  | P  | Q  | R  | S  | T  | U  | V  | W  | X  | Y  |

The encoded letter can be found at the intersection between the plaintext letter and corresponding key letter, as follows:

```
Key     B A D B A D B A
Text    H A D A F E E D
Cipher  I A G B F H F D
```

#### Properties
```
Ci: Letter at position i of the ciphertext
Mi: Letter at position i of the plaintext message

Ci = E(k, Mi) = (Mi + Ki) mod 26
Mi = D(k, Ci) = (Ci - Ki) mod 26
```

If the length of the repeated key is known, then the cipher is reduced to just be a group of interleaved monoalphabetic substitution ciphers, and frequency analysis can be done on each letter in the key.  In this case:
- **Keyspace** = 26<sup>k</sup>
- **Useful keyspace** = 25<sup>k</sup> (where `k` is the length of the repeated key).

### Beaufort Cipher
Used in the WWII M-209 cipher machine. Similar to Vigenère Cipher, but using the following tableau:

|    | A  | B  | C  | D  | E  | F  | G  | H  | I  | J  | K  | L  | M  | N  | O  | P  | Q  | R  | S  | T  | U  | V  | W  | X  | Y  | Z  | 
|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----| 
| A  | Z  | Y  | X  | W  | V  | U  | T  | S  | R  | Q  | P  | O  | N  | M  | L  | K  | J  | I  | H  | G  | F  | E  | D  | C  | B  | A  | 
| B  | A  | Z  | Y  | X  | W  | V  | U  | T  | S  | R  | Q  | P  | O  | N  | M  | L  | K  | J  | I  | H  | G  | F  | E  | D  | C  | B  | 
| C  | B  | A  | Z  | Y  | X  | W  | V  | U  | T  | S  | R  | Q  | P  | O  | N  | M  | L  | K  | J  | I  | H  | G  | F  | E  | D  | C  | 
| D  | C  | B  | A  | Z  | Y  | X  | W  | V  | U  | T  | S  | R  | Q  | P  | O  | N  | M  | L  | K  | J  | I  | H  | G  | F  | E  | D  | 
| E  | D  | C  | B  | A  | Z  | Y  | X  | W  | V  | U  | T  | S  | R  | Q  | P  | O  | N  | M  | L  | K  | J  | I  | H  | G  | F  | E  | 
| F  | E  | D  | C  | B  | A  | Z  | Y  | X  | W  | V  | U  | T  | S  | R  | Q  | P  | O  | N  | M  | L  | K  | J  | I  | H  | G  | F  | 
| G  | F  | E  | D  | C  | B  | A  | Z  | Y  | X  | W  | V  | U  | T  | S  | R  | Q  | P  | O  | N  | M  | L  | K  | J  | I  | H  | G  | 
| H  | G  | F  | E  | D  | C  | B  | A  | Z  | Y  | X  | W  | V  | U  | T  | S  | R  | Q  | P  | O  | N  | M  | L  | K  | J  | I  | H  | 
| I  | H  | G  | F  | E  | D  | C  | B  | A  | Z  | Y  | X  | W  | V  | U  | T  | S  | R  | Q  | P  | O  | N  | M  | L  | K  | J  | I  | 
| J  | I  | H  | G  | F  | E  | D  | C  | B  | A  | Z  | Y  | X  | W  | V  | U  | T  | S  | R  | Q  | P  | O  | N  | M  | L  | K  | J  | 
| K  | J  | I  | H  | G  | F  | E  | D  | C  | B  | A  | Z  | Y  | X  | W  | V  | U  | T  | S  | R  | Q  | P  | O  | N  | M  | L  | K  | 
| L  | K  | J  | I  | H  | G  | F  | E  | D  | C  | B  | A  | Z  | Y  | X  | W  | V  | U  | T  | S  | R  | Q  | P  | O  | N  | M  | L  | 
| M  | L  | K  | J  | I  | H  | G  | F  | E  | D  | C  | B  | A  | Z  | Y  | X  | W  | V  | U  | T  | S  | R  | Q  | P  | O  | N  | M  | 
| N  | M  | L  | K  | J  | I  | H  | G  | F  | E  | D  | C  | B  | A  | Z  | Y  | X  | W  | V  | U  | T  | S  | R  | Q  | P  | O  | N  | 
| O  | N  | M  | L  | K  | J  | I  | H  | G  | F  | E  | D  | C  | B  | A  | Z  | Y  | X  | W  | V  | U  | T  | S  | R  | Q  | P  | O  | 
| P  | O  | N  | M  | L  | K  | J  | I  | H  | G  | F  | E  | D  | C  | B  | A  | Z  | Y  | X  | W  | V  | U  | T  | S  | R  | Q  | P  | 
| Q  | P  | O  | N  | M  | L  | K  | J  | I  | H  | G  | F  | E  | D  | C  | B  | A  | Z  | Y  | X  | W  | V  | U  | T  | S  | R  | Q  | 
| R  | Q  | P  | O  | N  | M  | L  | K  | J  | I  | H  | G  | F  | E  | D  | C  | B  | A  | Z  | Y  | X  | W  | V  | U  | T  | S  | R  | 
| S  | R  | Q  | P  | O  | N  | M  | L  | K  | J  | I  | H  | G  | F  | E  | D  | C  | B  | A  | Z  | Y  | X  | W  | V  | U  | T  | S  | 
| T  | S  | R  | Q  | P  | O  | N  | M  | L  | K  | J  | I  | H  | G  | F  | E  | D  | C  | B  | A  | Z  | Y  | X  | W  | V  | U  | T  | 
| U  | T  | S  | R  | Q  | P  | O  | N  | M  | L  | K  | J  | I  | H  | G  | F  | E  | D  | C  | B  | A  | Z  | Y  | X  | W  | V  | U  | 
| V  | U  | T  | S  | R  | Q  | P  | O  | N  | M  | L  | K  | J  | I  | H  | G  | F  | E  | D  | C  | B  | A  | Z  | Y  | X  | W  | V  | 
| W  | V  | U  | T  | S  | R  | Q  | P  | O  | N  | M  | L  | K  | J  | I  | H  | G  | F  | E  | D  | C  | B  | A  | Z  | Y  | X  | W  | 
| X  | W  | V  | U  | T  | S  | R  | Q  | P  | O  | N  | M  | L  | K  | J  | I  | H  | G  | F  | E  | D  | C  | B  | A  | Z  | Y  | X  | 
| Y  | X  | W  | V  | U  | T  | S  | R  | Q  | P  | O  | N  | M  | L  | K  | J  | I  | H  | G  | F  | E  | D  | C  | B  | A  | Z  | Y  | 
| Z  | Y  | X  | W  | V  | U  | T  | S  | R  | Q  | P  | O  | N  | M  | L  | K  | J  | I  | H  | G  | F  | E  | D  | C  | B  | A  | Z  |

The encoded letter is found by taking the plaintext letter, going down the column to find the corresponding key letter, and the letter at the leftmost column is the encoded letter.

```
Key     B A D B A D B A
Text    H A D A F E E D
Cipher  J B H C G I G E
```

Note that the ciphertext with the same key is simply the ciphertext from the Vigenère cipher + 1 mod 26.

### M-94 Cipher
Contains 25 disks, each containing a random sequence of the 26 alphabets A-Z around it. Used by the US Army from 1922 to 1942.

#### Properties
- **Keyspace**: 25!  
  *(Note that the 26 individual rows do not further apply as separate combinations.)*
- **Useful keyspace**: 25!  
  *(Note that even for the correct key `k`, `E(k, m)` cannot be equal to `m`, since there are 25 other rows for the particular `k`.)*

### Enigma Machine
A machine with (3 to 5) rotors that can be positioned in any order. Substitutes each letter with another depending on a few different settings.

#### Properties *(For 3 rotors)*
Let P = plugboard transformation; U = reflector; L, M, R = left, mid and right rotors respectively.  
Keyspace = PRMLUL<sup>-1</sup>M<sup>-1</sup>R<sup>-1</sup>P<sup>-1</sup>  
Total settings = 159 quntillion (159 × 10<sup>18</sup>)

### Kasiski Method
A method used to break polyalphabetic substitution ciphers.

1. Find trigrams (or more generally, repetitions) to guess repeated key length.
2. Arrange cipher text letters into table with `K` columns.
3. Use frequency analysis to guess rotation eriod for each column.
  1. Find singular letter words or short words to guess letters (rotation)
  2. Find long words with most letters in the word decrypted to guess letters

## One-Time Pad (Vernam Cipher)
An **unconditionally secure** scheme, if the following rules are abode to:

- Requires that the OTP is known only to sender and receiver.
- Requires that the OTP is truly random.
- Requires that the OTP is used at most once.

Provides **perfect secrecy**, in that the ciphertext reveals absolutely no information about the plaintext, and so is immune against attacks even against attackers with infinite computation power.

## Transposition cipher
Transposition ciphers only re-order the letters in the original message. This is also known as an anagram.

## Rail-fence cipher
Write out plaintext letters in a zigzag fashion, and read them off by rows. Similar to SAF's "number-off 1-2-1-2" method of arranging the parade.