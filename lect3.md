# CS2107 Lecture 3

## Scams & phishing
- **Spoofing**: By creating counterfeits of legitimate sites, attackers trick users into taking ill-advised actions on the spoofed site
- **Phishing**: A subset of spoofing. Attempt to extract private user information through spoofing real sites.

## Attackers' goals
- **Total break**: Attacker finds the key
- **Partial break**: Attacker finds some specific information about the plaintext
- **Distinguishability**: Attacker can tell apart encryption of two given plaintext, or between encryption of a given plaintext and a random string.

## Attack models
- **Ciphertext only**: The attacker only has collection of ciphertext `c`.
- **Known plaintext**: The attacker has collection of plaintext `m` and their corresponding ciphertext `c`.
- **Chosen plaintext**: The attacker is able to encrypt any `m` → `c` for a reasonably long period of time (through a black box).
- **Chosen ciphertext**: The attacker is able to decrypt `c` → `m` through a black box.
- **Chosen text**: Attacker is able to encrypt and decrypt both ways through a black box.

## Symmetric and asymmetric key systems
In symmetric key systems, the sender and receivers use the same key for encryption and decryption, whereas in asymmetric key systems, different keys are used by different parties.

```
c = E(Ki, m)       c = E(Kpub, m)
m = D(Ki, c)       m = D(Kpriv, c)
```
- **Asymmetric encryption**: Encrypt plaintext with public key, transfer ciphertext securely. Only intended receiver is able to decrypt with his own private key.
- **Asymmetric authentication**: Encrypts a chosen plaintext (by challenger) with own private key, in order to prove one's identity (e.g. digital signatures).

### Sending email with digital signatures

| Alice | →    | Bob |
|-------|------|-----|
| Has: `Jpriv`, `Jpub`, `Kpub` | | Has: `Kpriv`, `Kpub`, `Jpub` |
| Writes message `m`, encrypts to `c = E(Kpub, m)`, and generate signature `Sc = E(Jpriv, h(c))` | → | Decrypts message `m = D(Kpriv, c)` |
| | | Makes sure that decrypted signature `Sd = D(Jpub, Sc)` matches hash generated from encrypted message `h(c)` |

### Pitfalls of PKI

#### Attack motivations
A MITM attack can be carried out to intercept and modify the public key that is transmitted, such that messages encoded with the unauthentic public key can be decoded by the attacker. Hence, there needs to be some trusted authority to certify that public keys are correct.

#### Certificate Authorities (CAs)
The CA digitally signs a certificate using its own private key. The certificate binds a public key and the identity of the user together. Well-known certificate authorities (e.g. Verisign) already have their public keys on each machine (e.g. computers, phones).

#### Process
1. The user requests a certificate from the Registration Authority (RA), and sends a public key he wishes to sign.
2. The RA verifies the user who wishes to generate a certificate (e.g. through email, domain ownership, etc).
3. The RA requests a certificate from the CA, which generates a certificate with the user's identity and public key provided (signed together with the CA's private key).
4. The certificate is returned to the user, which can be subsequently served to public domain.
5. Anyone can check the user's public key authenticity by verifying with the CA public key on the local machine.