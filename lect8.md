# CS2107 Lecture 8

## Diffie-Hellman Key Exchange

### Outline

| Alice |   | Bob |
|-------|---|-----|
| Has g, p, K<sub>a</sub> | | Has g, p, K<sub>b</sub> |
| Sends g<sup>K<sub>a</sub></sup> mod p | → &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ← | Sends g<sup>K<sub>b</sub></sup> mod p |
| (g<sup>K<sub>b</sub></sup> mod p)<sup>K<sub>a</sub></sup> mod p = g<sup>K<sub>a</sub> × K<sub>b</sub></sup> mod p | **Shared key** | (g<sup>K<sub>a</sub></sup> mod p)<sup>K<sub>b</sub></sup> mod p = g<sup>K<sub>a</sub> × K<sub>b</sub></sup> mod p |
| | ↓ *Attacker* ↓ | |
| | Has g, p, <br>g<sup>K<sub>a</sub></sup> mod p, <br>g<sup>K<sub>b</sub></sup> mod p | |

### Why it works
The **discrete logarithm problem** makes calculating `n = g^k mod p` easy, given g, k, p (p is prime), but makes it hard to calculate k in the same equation, given g, n, p.

Hence, even if an attacker manages to intercept the key exchanges and relevant numbers g and p, he is unable to get the shared key from these values.

Note that K<sub>a</sub> and K<sub>b</sub> are never revealed to Alice or Bob throughout.

## Hiding Secrets

Possible locations for secrets:
- File system
- Program (source code)
- OS
- Human brain
- Secondary devices (dongles, smart cards, SIM cards, etc)
  - Harder to attack and/or steal

## Security hardware

### Types of attacks
- **Side-channel**: Use some other property of the device (current consumption, time, radiation).
- **Microprobing**: Access the chip surface directly, to manipulate the device.
- **Software**: Use the normal device IO to exploit (software) vulnerabilities in the device.
- **Fault generation**: Generate errors to get access.
- **Reverse engineering**: Deduce, or re-create the electronic circuit of the device.

### Classes of attacks on security hardware
- **Class 1 attacks**: Outsiders who may not have detailed knowledge about the inner workings of the system
- **Class 2 attacks**: Insiders with detailed system knowledge
- **Class 3 attacks**: Governments/mafia/etc. - organisations with massive amounts of time and money

### Invasive attacks

#### Fuses (Class 3 attack)
- Imaging
  - Decapsulation with chemicals or laser cutters
  - Use chemical etch to expose the ROM mask layout
- Rewiring
  - Decapsulation
  - Re-connect the fuse (may need FIB - focused ion beam)
  - Read the memory using the serial port
- Probing
  - Decapsulation
  - Expose internal wiring (using FIB) and create landing pads for probes
  - Use microprobes to observe internal busses

### Non-invasive attacks

#### Physical glitching (Class 1 attack)
Use a pulse/glitch to occur exactly at a point in time when the processor checks for predicate in a `while` loop, etc., and make the loop run more than it is supposed to. This can lead to a buffer overflow attack to read bits beyond the string boundaries (i.e. don't terminate on NUL character).

### PK space attack
PK encryption on smartcards are slow:
- Exponentiation operation takes time proportional to the number of *set* bits (i.e. bit = 1) in a large prime number.
- Developers would choose to select only prime numbers with a small number of set bits.
- However this would make the smartcard open to attack, due to the small number of prime numbers.

### Timing attack
When loops in the program are not running in constant time, attackers can pass in different inputs to the program, and measure the time taken for program execution, in order to figure out program logic and/or secret data.