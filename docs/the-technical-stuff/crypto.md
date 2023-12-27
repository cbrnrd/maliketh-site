---
title: Crypto
layout: default
parent: The Technical Stuff
---

# Crypto
{: .fw-700}

Ahh crypto. The inspiration for the name of this project. Difficult to handle, feels great when you finally get it, but you'll cry a few times along the way (like Maliketh the Black Blade in Elden Ring, IYKYK). 

Below is a flow diagram for how the cryptography works for implant-c2 communication:

![Crypto flow diagram](/data/maliketh_implant_flow.png)


Each instance of the implant is responsible for a few different keys:
1. The hard-coded registration password
2. The implant's own public/private keypair
3. The server's public key

Each of these keys are used for different purposes. The registration password is used to encrypt the implant's public key when it registers with the server. The implant's keypair is used to decrypt all traffic FROM the server TO the implant. The server's public key is used to encrypt all communication FROM the implant TO the server.


## Libraries/Curves/Algorithms used

* [libsodium](https://libsodium.org/) for all crypto on the implant
* [pynacl](https://pynacl.readthedocs.io/en/latest/) for all crypto on the server
* [XSalsa20 and Poly1305 MAC](https://pynacl.readthedocs.io/en/latest/secret/#algorithm-details) for Password-Authenticated Key Exchange (registration)
* [Curve25519](https://pynacl.readthedocs.io/en/latest/public/#algorithm) for all other crypto


## Registration

See [here](/docs/the-technical-stuff/api-specifications/implant-c2/#c2register) for more information on the registration process.

Essentially, each implant generates a keypair and sends the public key to the server. This public key is encrypted with the hard-coded registration password. This means that the the registration password only has one use throughout the implant's lifetime. However, this also means that if the registration password is compromised, then ONLY the implant public key is compromised. This is not a huge deal, as the implant's public key is only used to encrypt traffic from the server to the implant.

Once the server receives the implant's key, it generates a new, unique serverside keypair **for that implant only**. It then encrypts the serverside public key with the implant's public key and sends it back to the implant. The implant then decrypts the serverside public key with its private key and stores it in memory. This is the key that will be used to encrypt all traffic from the implant to the server. 