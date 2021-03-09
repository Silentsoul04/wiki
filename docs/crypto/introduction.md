Cryptography can generally be divided into classical cryptography and modern cryptography.

Among them, classical cryptography, as a practical art, its coding and deciphering usually depends on the creativity and
skill of designers and adversaries, and does not clearly define the original cryptography. Classical cryptography mainly
includes the following aspects:

- Monoalphabetic Cipher
- Polyalphabetic Cipher
- Strange encryption

Modern cryptography originated from many related theories in the middle and late 20th century. In 1949, Shan Shan
published a classic paper entitled "Communication Theory of Security Systems", marking the beginning of modern
cryptography. Modern cryptography mainly includes the following aspects:

- Symmetric Cryptography, represented by DES, AES, and RC4.
- Asymmetric Cryptography, represented by RSA, ElGamal, elliptic curve encryption.
- Hash function, represented by MD5, SHA-1, SHA-512, etc.
- Digital Signature, represented by RSA signature, ElGamal signature, and DSA signature.

Among them, the symmetric encryption system is mainly divided into two ways:

- Block Cipher, also known as block cipher.
- Stream Cipher, also known as stream cipher.

In general, the fundamental goal of password designers is to protect information and information systems.

- Confidentiality (Confidentiality)
- Integrity
- Availability
- Authentication
- Non-repudiation

Among them, the first three are called the three elements of CIA for information security.

For password crackers, it is generally necessary to find a way to identify the cryptographic algorithm, and then brute
force, or use the cryptosystem vulnerability to crack. Of course, it is also possible to bypass the corresponding
detection by constructing a false hash value or a digital signature.

In general, we will assume that the attacker knows the cryptosystem to be cracked, and the attack types are usually
divided into the following four types:

| Attack Type              | Description                                                                                |
| :----------------------: | :----------------------------------------------------------------------------------------: |
| Ciphertext attack        | Only has ciphertext                                                                        |
| Known plaintext attack   | Have ciphertext and corresponding plaintext                                                |
| Select plaintext attack  | Have encryption permission, can encrypt the plaintext and get the corresponding ciphertext |
| Select ciphertext attack | Have decryption permission, can decrypt the ciphertext and get the corresponding plaintext |

Recommend some information here

- [Khan Academy Open Class](<http://open.163.com/special/Khan/moderncryptography.html>)
- [Cryptolaps](<https://cryptopals.com/>) a bunch of cryptography exercises.
- [Wikipedia-Cryptography](<https://en.wikipedia.org/wiki/Cryptography>)