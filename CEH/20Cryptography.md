# Module 20: Cryptography

- Generate hashes and checksum files
- Calculate the encrypted value of the selected file
- Use encrypting/decrypting techniques
- Perform file and data encryption
- Create self-signed certificates
- Perform email encryption
- Perform disk encryption
- Perform cryptanalysis

## Encrypt the Information using Various Cryptography Tools

### Calculate One-way Hashes using HashCalc
  Message digest (One-way Hash)
  - If any given bit of the function’s input is changed, every output bit has a 50% chance of changing.

  supports the Secure Hash Algorithm family: 
  - MD2, MD4, MD5, SHA1, SHA2 (SHA256, SHA384, SHA512), 
  - RIPEMD160, PANAMA, TIGER, CRC32, ADLER32, 
  - The hash used in the peer-to-peer file sharing applications, eDonkey and eMule.

### Calculate MD5 Hashes using MD5 Calculator

### Calculate MD5 Hashes using HashMyFiles

### Other tools
  - MD6 Hash Generator (https://www.browserling.com), 
  - All Hash Generator (https://www.browserling.com), 
  - MD6 Hash Generator (https://convert-tool.com), and 
  - md5 hash calculator (https://onlinehashtools.com) 

### Perform File and Text Message Encryption using CryptoForge
  protect the privacy of sensitive files, folders, or email messages by encrypting.\
  Once the information has been encrypted, it can be stored on insecure media or transmitted on an insecure network.
  
  https://www.cryptoforge.com/
  
  encrypt to a .cfd file
  
### Encrypt and Decrypt Data using BCTextEncoder

### Other tools
  - AxCrypt (https://www.axcrypt.net), 
  - Microsoft Cryptography Tools (https://docs.microsoft.com), and 
  - Concealer (https://www.belightsoft.com)

## Create a Self-signed Certificate
  useful only in a self-controlled testing environment.
  
### Create and Use Self-signed Certificates
  
  Adobe Acrobat Reader, Java’s keytool, Apple’s Keychain
  
  1. start Internet Information Services (IIS) Manager.
  2. double-click Server Certificates
  3. Create Self-Signed Certificate
  4. Click Bindings…
  5. type: https;  SSL certificate: just created one

## Perform Email Encryption

  methods:
  - Digital Signature: Uses asymmetric cryptography to simulate the security properties of a signature in digital, rather than written form
  - Secure Sockets Layer (SSL): Uses RSA asymmetric (public key) encryption to encrypt data transferred over SSL connections
  - Transport Layer Security (TLS): Uses a symmetric key for bulk encryption, an asymmetric key for authentication and key exchange, and message authentication codes for message integrity
  - Pretty Good Privacy (PGP): Used to encrypt and decrypt data that provides authentication and cryptographic privacy
  - GNU Privacy Guard (GPG): Software replacement of PGP and free implementation of the OpenPGP standard that is used to encrypt and decrypt data
  
### Perform Email Encryption using RMail
  
  https://rmail.com/
  
  Add Chrome extension. "RPost for Gmail"  

  Other:
  - Virtru (https://www.virtru.com), 
  - ZixMail (https://www.zixcorp.com), 
  - Egress Secure Email and File Transfer (https://www.egress.com), and 
  - Proofpoint Email Protection (https://www.proofpoint.com)

## Perform Disk Encryption

### Perform Disk Encryption using VeraCrypt
  
  1. Create an encrypted file container
  2. mount the container to a disk, like (I:)
  3. if an attacker manages to gain remote access or complete access to the machine, he/she will not be able to find the encrypted volume—including its files—unless he/she is able to obtain the password. 
  
### Perform Disk Encryption using BitLocker Drive Encryption

  ensure that data stored on a computer that is running Windows is not revealed if the computer is tampered with when the installed OS is offline.
  
  uses a microchip that is called a **Trusted Platform Module (TPM)** to provide enhanced protection for your data and to preserve early boot-component integrity. 
  
  1. encrypt a driver, like D:
  2. reboot computer
  3. input password to unlock the disk
  4. The disk will remain unlocked until the next time you restart the system.

### Perform Disk Encryption using Rohos Disk Encryption
  
  1. click Create new disk
  2. This drive appears only when you are connected to Rohos Disk Encryption, and disappears when you exit.

### Disk Encryption Other tools:
  - FinalCrypt (http://www.finalcrypt.org), 
  - Seqrite Encryption Manager (https://www.seqrite.com), 
  - FileVault (https://support.apple.com), and 
  - Gillsoft Full Disk Encryption (http://www.gilisoft.com)

## Perform Cryptanalysis using Various Cryptanalysis Tools

  methods:
  - **Linear** Cryptanalysis: A known plaintext attack that uses a linear approximation to describe the behavior of the block cipher
  - **Differential** Cryptanalysis: The examination of differences in an input and how this affects the resultant difference in the output
  - **Integral** Cryptanalysis: This attack is useful against block ciphers based on substitution-permutation networks and is an extension of differential cryptanalysis

  ### Perform Cryptanalysis using CrypTool
  
  ### Perform Cryptanalysis using AlphaPeeler

  ### Other tools:
  - Cryptosense (https://cryptosense.com)
  - RsaCtfTool (https://github.com) 
  - Msieve (https://sourceforge.net) 
  - Cryptol (https://cryptol.net) 
