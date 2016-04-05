# Introduction #

Standard implementations of the AES cipher on different platforms may use different default mode (Cipher Mode). Depending on a practical application, the default cipher mode can be a wrong choice.

Following article on wikipedia covers the block cipher modes - explaining the problem and illustrating it with a good real life example: [Block Cipher Modes of Operation](http://en.wikipedia.org/wiki/Block_cipher_modes_of_operation)

# Details #

The following quote from a Adobe's tech note about **"Strong encryption in ColdFusion MX 7"** (http://kb2.adobe.com/cps/546/e546373d.html) explains well when ECB should be chosen:

|_**ECB: Electronic Code Book**. This is the default mode that does not use any feedback to encrypt the data. This mode encrypts whole blocks, so a padding method like PKCS5Padding must also be used. ECB is fine for encrypting short strings of data, or for strings that do not contain any predictable or repeating groups of characters._|
|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|

# Code Samples #

In both supplied samples (for Java and for C#) use of the ECB mode is illustrated in **demo2encrypt** and **demo2decrypt** functions. And, so those functions are compatible with default parameters of **Decrypt** and **Encrypt** methods of ColdFusion.