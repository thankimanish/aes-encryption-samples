### The Source ###
The source code snippets you can find on this page are cut-and-pastes from the Java source code that can be downloaded from the Source repository or from the Downloads section.
### The Walkthrough ###
First, you have to import following namespaces:

```
using System;
using System.IO;
using System.Text;
using System.Security.Cryptography;
```

We assume our module has received encrypted data defined as following:
```
byte[] demo2EncryptedBytes;
```

The decryption is done by the following code. Look at the [How To Encrypt With Java](HowToEncryptWithJava.md) page - we have have to agree on the shared secret and the padding method; also, we use block size 128 bit (16 bytes):

```
byte[] demoKeyBytes = new byte[] {  0x00, 0x01, 0x02, 0x03, 0x04, 0x05, 0x06, 0x07,
                                    0x08, 0x09, 0x0a, 0x0b, 0x0c, 0x0d, 0x0e, 0x0f};
PaddingMode padding = PaddingMode.ISO10126;
byte[] demo2DecryptedBytes = demo2decrypt(demoKeyBytes, padding, demo2EncryptedBytes);
```


```
public static RijndaelManaged getAESECBCipher(byte[] keyBytes, PaddingMode padding)
{
    RijndaelManaged cipher = new RijndaelManaged();
    cipher.KeySize = 128;
    cipher.BlockSize = 128;
    cipher.Mode = CipherMode.ECB;
    cipher.Padding = padding;
    cipher.Key = keyBytes;
    return cipher;
}

public static byte[] decrypt(RijndaelManaged cipher, byte[] encrypted)
{
    ICryptoTransform decryptor = cipher.CreateDecryptor();
    MemoryStream msDecrypt = new MemoryStream(encrypted);
    CryptoStream csDecrypt = new CryptoStream(msDecrypt, decryptor, CryptoStreamMode.Read);
    byte[] fromEncrypt = new byte[encrypted.Length];
    csDecrypt.Read(fromEncrypt, 0, fromEncrypt.Length);
    return fromEncrypt;
}
```

```
public static byte[] demo2decrypt(byte[] keyBytes, PaddingMode padding, byte[] encryptedMessageBytes)
{
    RijndaelManaged decipher = getAESECBCipher(keyBytes, padding);
    return decrypt(decipher, encryptedMessageBytes);
}
```