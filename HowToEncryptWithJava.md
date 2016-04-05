## The source ##

The source code snippets you can find on this page are cut-and-pastes from the Java source code that can be downloaded from the Source repository or from the Downloads section.

## The Walk Through ##
### Import the Standard Libraries ###
All we need is three import statements:
```
import javax.crypto.*;
import javax.crypto.spec.*;
import java.io.*;
```
### The [CipherMode](CipherModes.md) (or Feedback mode) ###

First you need to choose which feedback mode you are going to use in your application. If the data is short messages with irregular structure, then ECB is probably the best choice, because you do not need an initialization vector (another piece of information that has to be known to both sides - the encrypting and the decrypting ones). In this walk through we use ECB. (See the second line of the _[getAESECBEncryptor](#getAESECBEncryptor_and_encrypt_methods.md)_ method below)

### The Padding ###

We will agree on a padding method, and the good one can be the ISO 10126 padding, which is available on all the popular platforms - so, we stick to it here.

### How to Encrypt ###

```
    String sDemoMesage = "This is a demo message from Java!";
    byte[] demoMesageBytes = sDemoMesage.getBytes();
    //shared secret
    byte[] demoIVBytes = new byte[] { 0x00, 0x01, 0x02, 0x03, 0x04, 0x05, 0x06, 0x07,
                                        0x08, 0x09, 0x0a, 0x0b, 0x0c, 0x0d, 0x0e, 0x0f};
    String sPadding = "ISO10126Padding";
    byte[] demo2EncryptedBytes = demo2encrypt(demoKeyBytes, sPadding, demoMesageBytes);
```

**Pay attention:** The shared secret is an array of 16 random bytes - that corresponds to 128 bit key size supported by default by all the platforms. (this is compliant to US government requirement for transmission of unclassified data)

### demo2encrypt ###
This would be the method you will call differently in your implementation, but, essentially, it will do the same to things - the cipher instantiation and actual encryption
```
public static byte[] demo2encrypt(byte[] keyBytes, String sPadding, byte[] messageBytes) throws Exception {
    Cipher cipher = getAESECBEncryptor(keyBytes, sPadding); 
    return encrypt(cipher, messageBytes);
}
```
### _getAESECBEncryptor_ and _encrypt_ methods ###
The following methods are just fine to be borrowed, unless you'd like to implement your own exception/error handling and improve performance (if the last one is critical):
```
public static Cipher getAESECBEncryptor(byte[] keyBytes, String padding) throws Exception{
    SecretKeySpec key = new SecretKeySpec(keyBytes, "AES");
    Cipher cipher = Cipher.getInstance("AES/ECB/"+padding);
    cipher.init(Cipher.ENCRYPT_MODE, key);
    return cipher;
}

public static byte[] encrypt(Cipher cipher, byte[] dataBytes) throws Exception{
    ByteArrayInputStream bIn = new ByteArrayInputStream(dataBytes);
    CipherInputStream cIn = new CipherInputStream(bIn, cipher);
    ByteArrayOutputStream bOut = new ByteArrayOutputStream();
    int ch;
    while ((ch = cIn.read()) >= 0) {
        bOut.write(ch);
    }
    return bOut.toByteArray();
} 
```

### How To Decrypt? ###

Now that would be a good time to read HowToDecryptWithJava or [How To Decrypt With C#](HowToDecryptWithCSharp.md) ...

Unfortunately, the articles are not written yet - give me few more days, please.