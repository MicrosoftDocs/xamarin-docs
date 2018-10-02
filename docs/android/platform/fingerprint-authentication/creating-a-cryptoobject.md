---
title: "Creating a CryptoObject"
ms.prod: xamarin
ms.assetid: 4D159B80-FFF4-4136-A7F0-F8A5C6B86838
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
---

# Creating a CryptoObject

The integrity of the fingerprint authentication results is important to an application &ndash; it is how the application knows the identity of the user. It is theoretically possible for third-party malware to intercept and tamper with the results returned by the fingerprint scanner. This section will discuss one technique for preserving the validity of the fingerprint results. 

The `FingerprintManager.CryptoObject` is a wrapper around the Java cryptography APIs and is used by the `FingerprintManager` to protect the integrity of the authentication request. Typically, a `Javax.Crypto.Cipher` object is the mechanism for encrypting the results of the fingerprint scanner. The `Cipher` object itself will use a key that is created by the application using the Android keystore APIs.

To understand how these classes all work together, let's first look at the following code which demonstrates how to create a `CryptoObject`, and then explain in more detail:

```csharp
public class CryptoObjectHelper
{
    // This can be key name you want. Should be unique for the app.
    static readonly string KEY_NAME = "com.xamarin.android.sample.fingerprint_authentication_key";

    // We always use this keystore on Android.
    static readonly string KEYSTORE_NAME = "AndroidKeyStore";

    // Should be no need to change these values.
    static readonly string KEY_ALGORITHM = KeyProperties.KeyAlgorithmAes;
    static readonly string BLOCK_MODE = KeyProperties.BlockModeCbc;
    static readonly string ENCRYPTION_PADDING = KeyProperties.EncryptionPaddingPkcs7;
    static readonly string TRANSFORMATION = KEY_ALGORITHM + "/" +
                                            BLOCK_MODE + "/" +
                                            ENCRYPTION_PADDING;
    readonly KeyStore _keystore;

    public CryptoObjectHelper()
    {
        _keystore = KeyStore.GetInstance(KEYSTORE_NAME);
        _keystore.Load(null);
    }

    public FingerprintManagerCompat.CryptoObject BuildCryptoObject()
    {
        Cipher cipher = CreateCipher();
        return new FingerprintManagerCompat.CryptoObject(cipher);
    }

    Cipher CreateCipher(bool retry = true)
    {
        IKey key = GetKey();
        Cipher cipher = Cipher.GetInstance(TRANSFORMATION);
        try
        {
            cipher.Init(CipherMode.EncryptMode | CipherMode.DecryptMode, key);
        } catch(KeyPermanentlyInvalidatedException e)
        {
            _keystore.DeleteEntry(KEY_NAME);
            if(retry)
            {
                CreateCipher(false);
            } else
            {
                throw new Exception("Could not create the cipher for fingerprint authentication.", e);
            }
        }
        return cipher;
    }

    IKey GetKey()
    {
        IKey secretKey;
        if(!_keystore.IsKeyEntry(KEY_NAME))
        {
            CreateKey();
        }

        secretKey = _keystore.GetKey(KEY_NAME, null);
        return secretKey;
    }

    void CreateKey()
    {
        KeyGenerator keyGen = KeyGenerator.GetInstance(KeyProperties.KeyAlgorithmAes, KEYSTORE_NAME);
        KeyGenParameterSpec keyGenSpec =
            new KeyGenParameterSpec.Builder(KEY_NAME, KeyStorePurpose.Encrypt | KeyStorePurpose.Decrypt)
                .SetBlockModes(BLOCK_MODE)
                .SetEncryptionPaddings(ENCRYPTION_PADDING)
                .SetUserAuthenticationRequired(true)
                .Build();
        keyGen.Init(keyGenSpec);
        keyGen.GenerateKey();
    }
}
```

The sample code will create a new `Cipher` for each `CryptoObject`, using a key that was created by the application. The key is identified by the `KEY_NAME` variable that was set in the beginning of the `CryptoObjectHelper` class. The method `GetKey` will try and retrieve the key using the Android Keystore APIs. If the key does not exist, then the method `CreateKey` will create a new key for the application.

The cipher is instantiated with a call to `Cipher.GetInstance`, taking a _transformation_ (a string value that tells the cipher how to encrypt and decrypt data). The call to `Cipher.Init` will complete the initialization of the cipher by providing a key from the application. 

It is important to realize that there are some situations where Android may invalidate the key: 

* A new fingerprint has been enrolled with the device.
* There are no fingerprints enrolled with the device.
* The user has disabled the screen lock.
* The user has changed the screen lock (the type of the screenlock or the PIN/pattern used).

When this happens, `Cipher.Init` will throw a [`KeyPermanentlyInvalidatedException`](http://developer.android.com/reference/android/security/keystore/KeyPermanentlyInvalidatedException.html). The above sample code will trap that exception, delete the key, and then create a new one.

The next section will discuss how to create the key and store it on the device.

## Creating a Secret Key

The `CryptoObjectHelper` class uses the Android [`KeyGenerator`](https://developer.xamarin.com/api/type/Javax.Crypto.KeyGenerator/) to create a key and store it on the device. The `KeyGenerator` class can create the key, but needs some meta-data about the type of key to create. This information is provided by an instance of the [`KeyGenParameterSpec`](http://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.html) class. 

A `KeyGenerator` is instantiated using the `GetInstance` factory method. The sample code uses the [_Advanced Encryption Standard_](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) (_AES_) as the encryption algorithm. AES will break the data up into blocks of a fixed size and encrypt each of those blocks.

Next, a `KeyGenParameterSpec` is created using the `KeyGenParameterSpec.Builder`. The `KeyGenParameterSpec.Builder` wraps the following information about the key that is to be created:

* The name of the key.
* The key must be valid for both encrypting and decrypting.
* In the sample code the  `BLOCK_MODE` is set to _Cipher Block Chaining_ (`KeyProperties.BlockModeCbc`), meaning that each block is XORed with the previous block (creating dependencies between each block). 
* The `CryptoObjectHelper` uses [_Public Key Cryptography Standard #7_](https://tools.ietf.org/html/rfc2315) (_PKCS7_) to generate the bytes that will pad out the blocks to ensure that they are all of the same size.
* `SetUserAuthenticationRequired(true)` means that user authentication is required before the key can be used.

Once the `KeyGenParameterSpec` is created, it is used to initialize the `KeyGenerator`, which will generate a key and  securely store it on the device. 

## Using the CryptoObjectHelper

Now that the sample code has encapsulated much of the logic for creating a `CryptoWrapper` into the `CryptoObjectHelper` class, let's revisit the code from the start of this guide and use the `CryptoObjectHelper` to create the Cipher and start a fingerprint scanner: 

```csharp
protected void FingerPrintAuthenticationExample()
{
    const int flags = 0; /* always zero (0) */
    
    CryptoObjectHelper cryptoHelper = new CryptoObjectHelper();
    cancellationSignal = new Android.Support.V4.OS.CancellationSignal();
    
    // Using the Support Library classes for maximum reach
    FingerprintManagerCompat fingerPrintManager = FingerprintManagerCompat.From(this);
    
    // AuthCallbacks is a C# class defined elsewhere in code.
    FingerprintManagerCompat.AuthenticationCallback authenticationCallback = new MyAuthCallbackSample(this);

    // Here is where the CryptoObjectHelper builds the CryptoObject. 
    fingerprintManager.Authenticate(cryptohelper.BuildCryptoObject(), flags, cancellationSignal, authenticationCallback, null);
}
```

Now that we have seen how to create a `CryptoObject`, lets move on to see how the `FingerprintManager.AuthenticationCallbacks` are used to transfer the results of fingerprint scanner service to an Android application.



## Related Links

- [Cipher](https://developer.xamarin.com/api/type/Javax.Crypto.Cipher/)
- [FingerprintManager.CryptoObject](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.CryptoObject.html)
- [FingerprintManagerCompat.CryptoObject](http://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.CryptoObject.html)
- [KeyGenerator](https://developer.xamarin.com/api/type/Javax.Crypto.KeyGenerator/)
- [KeyGenParameterSpec](http://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.html)
- [KeyGenParameterSpec.Builder](http://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.Builder.html)
- [KeyPermanentlyInvalidatedException](http://developer.android.com/reference/android/security/keystore/KeyPermanentlyInvalidatedException.html)
- [KeyProperties](http://developer.android.com/reference/android/security/keystore/KeyProperties.html)
- [AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)
- [RFC 2315 - PCKS #7](https://tools.ietf.org/html/rfc2315)
