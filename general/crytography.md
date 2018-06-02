# Crytography

## Symmetric Key Encryption

对称密钥加密：采用单钥密码系统的加密方法，同一个密钥可以同时用作信息的加密和解密，也称为单密钥加密。

Keeper 使用广泛被接受为最强加密的 256 位 AES 加密和 PBKDF2 SHA-256 算法来保护您的信息。 

## HASH

A hash function is any function that can be used to map data of arbitrary size to data of fixed size.

A cryptographic hash function is a special class of hash function that has certain properties which make it suitable for use in cryptography.

The input data is often called the message, and the output \(the hash value or hash\) is often called the message digest or simply the digest（摘要）.

## Public-key cryptography

Public key cryptography, or asymmetrical cryptography, is any cryptographic system that uses pairs of keys: public keys which may be disseminated widely, and private keys which are known only to the owner.

Public key algorithms, unlike symmetric key algorithms, _do not require a secure channel for the initial exchange of one or more secret keys between the parties_.

Because of the computational complexity of asymmetric encryption, _it is usually used only for small blocks of data_, typically the transfer of a symmetric encryption key \(e.g. a session key\). This symmetric key is then used to encrypt the rest of the potentially long message sequence. The symmetric encryption/decryption is based on simpler algorithms and is much faster.

### Authentication

The public key verifies that a holder of the paired private key sent the message. 

Linux SSH: When a client attempts to authenticate using SSH keys, the server can test the client on whether they are in possession of the private key. If the client can prove that it owns the private key, a shell session is spawned or the requested command is executed.

### Encryption

Only the paired private key holder can decrypt the message encrypted with the public key.

