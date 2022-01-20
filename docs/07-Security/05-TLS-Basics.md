# 1. TLS Basics
  - Take me to [Video Tutorial](https://kodekloud.com/topic/tls-basics/)
  
In this section, we will take a look at TLS Basics

- [1. TLS Basics](#1-tls-basics)
  - [1.1. Certificate](#11-certificate)
  - [1.2. Symmetric Encryption](#12-symmetric-encryption)
  - [1.3. Asymmetric Encryption](#13-asymmetric-encryption)
  - [1.4. Public Key Infrastructure](#14-public-key-infrastructure)
  - [1.5. Certificates naming convention](#15-certificates-naming-convention)
- [TLS Handshake explained](#tls-handshake-explained)

## 1.1. Certificate
- A certificate is used to guarantee trust between 2 parties during a transaction.
- Example: when a user tries to access web server, tls certificates ensure that the communication between them is encrypted.

  ![cert1](../../images/cert1.PNG)
  
  
## 1.2. Symmetric Encryption
- It is a secure way of encryption, but it uses the same key to encrypt and decrypt the data and the key has to be exchanged between the sender and the receiver, there is a risk of a hacker gaining access to the key and decrypting the data.

  ![cert2](../../images/cert2.PNG)
  
## 1.3. Asymmetric Encryption
- Instead of using single key to encrypt and decrypt data, asymmetric encryption uses a pair of keys, a private key and a public key.

  ![cert3](../../images/cert3.PNG)
  
  ![cert4](../../images/cert4.PNG)
  
  ![cert5](../../images/cert5.PNG)
  
  ![cert6](../../images/cert6.PNG)
  

**How do you look at a certificate and verify if it is legit?**
- who signed and issued the certificate.
- If you generate the certificate then you will have it sign it by yourself; that is known as self-signed certificate.

  ![cert7](../../images/cert7.PNG)
  
**How do you generate legitimate certificate? How do you get your certificates singed by someone with authority?**
- That's where **`Certificate Authority (CA)`** comes in for you. Some of the popular ones are Symantec, DigiCert, Comodo, GlobalSign etc.

  ![cert8](../../images/cert8.PNG)
  
  ![cert9](../../images/cert9.PNG)
  
  ![cert10](../../images/cert10.PNG)
  
## 1.4. Public Key Infrastructure
   
   ![pki](../../images/pki.PNG)
   
## 1.5. Certificates naming convention

  ![cert11](../../images/cert11.PNG)
  
  
# TLS Handshake explained

[Transport Layer Security (TLS) - Computerphile](https://www.youtube.com/watch?v=0TLDTodL7Lc)

When you connect to a website the following usually happens: 

HTTP payload is going to be put inside TCP packets which is going to be put inside IP packets which tell details about destination, etc. and then Ethernet lives at the bottom.

|||||
|-----|----|-----|------|
| ETH | IP | TCP | HTTP |
|||||

[Computerphile](https://www.youtube.com/watch?v=86cQJ0MMses)

This video explains TLS 1.2 and not 1.3 as apparently 1.2 is more intuitive to understand than 1.3

| TLS 	| ECDHE 	| RSA 	| WITH 	| AES 	| 128 	| GCM 	| SHA256 	|
|---	|---	|---	|---	|---	|---	|---	|---	|
|  	| Key exchange 	| Public Key Authentication mechanism 	|  	| Cipher 	| key size 	| Mode of operation 	| hash function 	|
|  	| Elliptic curve Diffie Hellman ephemeral 	|  	|  	|  	|  	|  	|  	|

- Begin with a TLS handshake
  - Send Client hello message
    - max TLS version supported
    - Random number (to make sure replay attacks for example doesn't work)
    - List of cipher suites supported
  - The server now knows what the client can do and the server choses and sends (If the server doesn't support the client supported versions or cipher suites, the server sends TLS Alert failure message) a server hello
    - Chosen TLS version
    - Random number
    - Chose one of the cipher suites
  - The server also sends a server certificate with a public key attached to it
    - RSA Key
    - Parameters for Diffie Hellman (Eg: `g` and `n` from [this video](https://www.youtube.com/watch?v=NmM9HA2MQGI))
    - Digital signature: This is essentially a set of the previous messages summarized using a hash function using the servers' private key. Only the server can create this with their private key. 
    - Server hello done
  - Client now replies with client key exchange
    - Usually the client public key
  - Client sends, Change cipher spec message
  - Client sends, Finished : Summary of all the messages seen so far
  - Server sends, finished message

TLS 1.3 does this even faster with just one round trip

  
   

  
  
  

  
  
  
  
  
  

  
  
