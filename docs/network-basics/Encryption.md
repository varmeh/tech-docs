# Encryption

## Symmetric Encryption

Symmetric encryption, also known as `private-key`, `single-key`, or `secret-key` encryption, uses the same key for both encryption and decryption.

This means that both the sender and the receiver need to know, and use, the same secret key.

Here's a simple explanation of how symmetric encryption works:

1. The sender uses a key (a series of numbers) along with an encryption algorithm to encrypt the original message (*plaintext*) into a scrambled message (*ciphertext*).
2. The encrypted message is then sent over an insecure network to the receiver.
3. The receiver, who also knows the secret key, uses it along with the same encryption algorithm to decrypt the message and get back the original plaintext.

Symmetric encryption is widely used due to its speed and efficiency.

It is typically used in cases where large amounts of data need to be encrypted quickly.

Examples of symmetric encryption algorithms include AES (Advanced Encryption Standard), DES (Data Encryption Standard), and 3DES.

However, symmetric encryption has a major drawback: `key distribution`.

That is, you need a secure way to share the secret key with the recipient. If an unauthorized party gets hold of the key, they can decrypt the messages.

## Asymmetric Encryption

Asymmetric encryption, also known as `public key encryption`, uses two keys: one for encryption and one for decryption.

One key (`the public key`) is used to `encrypt the message`, and a separate key (`the private key`) is used to `decrypt` it.

Here's a simple explanation of how asymmetric encryption works:

1. The sender uses the recipient's public key (*known to everyone*) and an encryption algorithm to encrypt the original message into a scrambled message.
2. The encrypted message is then sent over an insecure network to the receiver.
3. The receiver uses their private key (*known only to the*m) to decrypt the message and get back the original plaintext.

Asymmetric encryption is mainly used for secure key exchange and confidentiality, and for digital signatures that authenticate the sender's identity.

Examples include RSA (Rivest–Shamir–Adleman), DSA (Digital Signature Algorithm), and ECC (Elliptic Curve Cryptography).

The main disadvantage of asymmetric encryption is that it is `slower` and more `computationally intensive` than symmetric encryption.

## Symmetric vs Asymmetric Encryption

|   | Symmetric Encryption | Asymmetric Encryption |
|---|---|---|
| **Key Usage** | Uses the same key for encryption and decryption. | Uses a pair of keys: public key for encryption and private key for decryption. |
| **Speed** | Generally faster due to straightforward and less computational algorithms. | Slower because of the complex mathematical operations involved. |
| **Resource Usage** | Uses less computational resources. | Uses more computational resources due to complex algorithms. |
| **Scalability** | Scales well with larger amounts of data. | Less scalable as resource needs increase with data size. |
| **Security** | Key exchange is a challenge as it requires a secure method to share the encryption key. | More secure because the private key is never shared. |
| **Examples** | AES, DES, 3DES, RC4, etc. | RSA, DSA, ECC, ElGamal, etc. |

## Role in HTTPS Protocol

In an HTTPS connection, both symmetric and asymmetric encryption are used together for their respective advantages.

Initially, `asymmetric encryption is used to exchange a symmetric key` in a secure manner.

Once the key exchange is done, the `actual data transmission is encrypted using symmetric encryption` with the exchanged key.

This approach utilizes the speed and efficiency of symmetric encryption for encrypting the bulk of the data, while using the secure key exchange capabilities of asymmetric encryption to ensure the symmetric key can be safely transmitted.

Refer [HTTPS Explained](./HttpsExplained.md) for details.
