# Data Integrity and Privacy

>## Payload Encryption
Sensitive data sent in any API call will be field-level encrypted using Elliptic Curve Diffie-Hellman (id-ecDH 1.3.132.1.12). A standard P-256 curve will be used. UTF-8 encoded bytes of the plaintext JSON "encryptedData" dictionary shall be encrypted and included as a Base64 encoded String in the actual JSON payload sent. Field level encryption shall use AES–256 (id-aes256-GCM 2.16.840.1.101.3.4.1.46), with an initialization vector of 16 null bytes and no associated authentication data.

**Encryption Version "EC_v1" Key Derivation Function **
Use the key derivation function described in NIST SP 800-56A, section 5.8.1, with the following input values:

| Input Name | Value |
|---------|----------|
|Hash Function	     | SHA-256.       |
|Z	                 | The shared secret calculated above using ECDH. |
|Algorithm ID	       | The byte (0x0D) followed by the ASCII string "id-aes256-GCM". The first byte of this value is an unsigned integer that indicates the string’s length in bytes; the remaining bytes are a variable-length string.|
|Party U Info	       | The ASCII string "PayForward". This value is a fixed-length string. |
|Party V Info	       | The SHA-256 hash of the UTF-8 encoded bytes of the Person Identifier. |
| Supplemental Public and Private Info| 	Unused |

> ## Signature Validation
**PKCS#7 detached ECC Signature covering Transaction request** 
>
>Signatures will cover a SHA-256 hash of a concatenation of the following elements in the order they are presented in the table.

| Field | Description |
|---------|----------|
| UTF-8 encoded bytes of personId	|Sender's Person Identifier |
| UTF-8 encoded bytes of targetLinkId	| Recipient's Person Identifier |
| UTF-8 encoded bytes of amount	| Transactionamount |
| UTF-8 encoded bytes of currencyCode	| Currency Code |
| UTF-8 encoded bytes of transactionId |	Transaction identifier | 

