# Cyrptography

## What I learned

I learned about various cryptographic attacks including Cipher-text Only, Chosen-Plaintext, and Known-Plaintext attacks, and how each attack provides different levels of information to the attacker. I explored block cipher modes of operation, such as CBC, and their importance in ensuring secure encryption. Additionally, I practiced encryption and decryption using Caesar Cipher, Vigenère Cipher, and mono-alphabetic ciphers. I computed RSA key pairs and learned the process for Diffie-Hellman key exchange. These exercises helped solidify my understanding of cryptographic techniques, cipher mechanisms, and key management.

## Q1. Types of Cryptographic Attacks

### Q1.1 Cipher-text Only Attack
**Definition:** In a cipher-text only attack, the attacker only has access to the ciphertext. The goal is to deduce the plaintext or the encryption key without any knowledge of the plaintext.

**Choice:**
- **Choice 3:** Attacker has ciphertext that she can analyze.

### Q1.2 Chosen-Plaintext Attack
**Definition:** In a chosen-plaintext attack, the attacker can choose arbitrary plaintexts to be encrypted and obtain the corresponding ciphertexts. This can help the attacker deduce information about the encryption key or algorithm.

**Choice:**
- **Choice 1:** Attacker can get the ciphertext from some chosen plaintext.

### Q1.3 Known-Plaintext Attack
**Definition:** In a known-plaintext attack, the attacker has access to both the plaintext and its corresponding ciphertext. The goal is to deduce the encryption key or understand the encryption algorithm.

**Choice:**
- **Choice 2:** Attacker has some plaintext corresponding to some ciphertext.

## Q2. Block Cipher Modes of Operation
**Definition:** Block cipher modes of operation are techniques used to securely encrypt data with block ciphers. They ensure that identical plaintext blocks produce different ciphertext blocks to improve security.

**True Statements:**

- **Choice 1:** To ensure blocks of the same plaintext will give different ciphertext.

- **Choice 2:** The Initialization Vector (IV) does not have to be kept a secret.

## Q3. Caesar Cipher Encryption

**Task:** Encrypt the text “MALWARE” using Caesar’s Cipher with a key of +3.

**Solution:** Caesar Cipher shifts each letter in the plaintext by 3 positions in the alphabet.
```
- M → P
- A → D
- L → O
- W → Z
- A → D
- R → U
- E → H
```
**Encrypted text:** `PDOZDUH`

## Q4. Mono-Alphabetic Cipher Decryption

**Cipher Table:**

- **Plaintext:** `abcdefghijklmnopqrstuvwxyz`

- **Ciphertext:** `mnbvcxzasfdghjklpoiuytrewq`

**Task:** Decrypt `“icbyosuw”` using the provided mono-alphabetic cipher.

**Solution:**
```
- i → s
- c → e
- b → c
- y → u
- o → r
- s → i
- u → t
- w → y
```
**Decrypted text:** `security`

## Q5. Vigenère Cipher Encryption
**Task:** Encrypt `“PENTEST”` using the Vigenère Cipher with the key `“NETSEC”`.

**Solution:**

1. Write the key repeatedly to match the length of the plaintext.

2. Use the Vigenère table or shift each letter in the plaintext by the corresponding letter in the key.

**Encrypted text:** CIGLIUG

## Q6. Vigenère Cipher Decryption
**Task:** Decrypt `“IMKMW”` using the key `“NETSEC”`.

**Solution:**
1. Use the Vigenère table or reverse the shifts applied during encryption.

**Decrypted text:** `VIRUS`

## Q7. RSA Modulo Calculation
**Task:** Compute `\(47^{11} \mod 13\)`.

**Solution:** Use modular exponentiation tools or software.

**Result:** `5`

## Q8. Ciphertext without CBC
**Task:** Without using Cipher Block Chaining (CBC), find the ciphertext for `“111100011100”` using the given table.

**Solution:** Apply the given input-output mapping directly.

**Ciphertext:** `010001111011`

## Q9. CBC Encryption
**Task:** Encrypt `“111100011100”` using CBC mode with `IV=101`.

**Solution:**
1. Apply the XOR operation between the IV and the first block of plaintext.

2. Encrypt this result, then XOR the result with the next plaintext block, and so on.

**Ciphertext:** `101111011010`

## Q10. RSA Key Generation
**Given primes:** `\( p=7 \), \( q=11 \)`

**10.1 Compute n:**
`\[ n = p \times q = 7 \times 11 = 77 \]`

**10.2 Compute φ(n):**
`\[ \phi(n) = (p-1) \times (q-1) = (7-1) \times (11-1) = 60 \]`

**10.3 First five smallest possible values of `e (coprime with φ(n))`:**
`\[ e = 7, 11, 13, 17, 19 \]`

**10.4 Private exponent d (using e=7 and φ(n)=60):** `Find \( d \) such that \( e \times d \mod \phi(n) = 1 \).` The value is **43**.

**10.5 Encrypt m=10:**
`\[ c = m^e \mod n = 10^7 \mod 77 = 10 \]`

## Q11. Diffie-Hellman Key Exchange
**Given:** `\( g=5 \), \( n=18 \), Alice’s secret \( a=5 \), Bob’s secret \( b=8 \)`

**11.1 Alice’s public key A:**
`\[ A = g^a \mod n = 5^5 \mod 18 = 11 \]`

**11.2 Bob’s public key B:**
`\[ B = g^b \mod n = 5^8 \mod 18 = 7 \]`

**11.3 Shared key K:**
`\[ K = B^a \mod n = 7^5 \mod 18 = 13 \]`
