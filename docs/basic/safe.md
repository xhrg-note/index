## 简单安全相关


> 注意点：一般情况下加密解密的串,对于算法而言都是以byte数组进出,我们为了就可以显示的传输存储，一般要做base64处理，切勿用16禁止或者其他方式，可以参考apache dcode库或者spring库等的做法


## 一、签名和验签

数据在网络传输的时候，可能会被劫持，这个时候，不发份子可能会把数据进行修改。比如说，传递数据是"1",他修改为"1000"。这种是无法接收的。所以一般需要把书籍进行签名。签名后另一端再进行验证签名。

某些业务数据 + 算法 = 字符串

简单签名算法如：签名端把业务数据和字符串传递过去，另一端再次签名，然后对比签名结果是否相同。


#### 1.1、md5签名

利用需要加签的数据，生成一个固定的字符串，或者加上一个盐(secret,salt),把该字符串进行,md5后做base64，得到一个字符串。2端都这样做，然后对比这个字符串。

#### 1.2、HmacSha1

这个也是一个签名算法，比如开源的apollo就是用这个算法。HmacSha1Utils.signString(stringToSign, secret);这里的secret也是双方约定的一个随机字符串
```
public class HmacSha1Utils {
  private static final String ALGORITHM_NAME = "HmacSHA1";
  private static final String ENCODING = "UTF-8";
  public static String signString(String stringToSign, String accessKeySecret) {
    try {
      Mac mac = Mac.getInstance(ALGORITHM_NAME);
      mac.init(new SecretKeySpec(
          accessKeySecret.getBytes(ENCODING),
          ALGORITHM_NAME
      ));
      byte[] signData = mac.doFinal(stringToSign.getBytes(ENCODING));
      return BaseEncoding.base64().encode(signData);
    } catch (NoSuchAlgorithmException | UnsupportedEncodingException | InvalidKeyException e) {
      throw new IllegalArgumentException(e.toString());
    }
  }
}
```


#### 1.3、SHA256withRSA签名算法

公钥和私钥

## 二、加解密

* 业务数据 + 盐 + 算法 = 密文
* 密文 + 盐 + 算法 = 业务数据

#### 2.1、AES加解密

```
golang例子：https://www.jianshu.com/p/9c1c8958b279

package main

import (
    "bytes"
    "crypto/cipher"
    "crypto/aes"
    "encoding/base64"
    "fmt"
)

func PKCS7Padding(ciphertext []byte, blockSize int) []byte {
    padding := blockSize - len(ciphertext) % blockSize
    padtext := bytes.Repeat([]byte{byte(padding)}, padding)
    return append(ciphertext, padtext...)
}

func PKCS7UnPadding(origData []byte) []byte {
    length := len(origData)
    unpadding := int(origData[length-1])
    return origData[:(length - unpadding)]
}

func AesEncrypt(origData, key []byte) ([]byte, error) {
    block, err := aes.NewCipher(key)
    if err != nil {
        return nil, err
    }
    blockSize := block.BlockSize()
    origData = PKCS7Padding(origData, blockSize)
    blockMode := cipher.NewCBCEncrypter(block, key[:blockSize])
    crypted := make([]byte, len(origData))
    blockMode.CryptBlocks(crypted, origData)
    return crypted, nil
}

func AesDecrypt(crypted, key []byte) ([]byte, error) {
    block, err := aes.NewCipher(key)
    if err != nil {
        return nil, err
    }
    blockSize := block.BlockSize()
    blockMode := cipher.NewCBCDecrypter(block, key[:blockSize])
    origData := make([]byte, len(crypted))
    blockMode.CryptBlocks(origData, crypted)
    origData = PKCS7UnPadding(origData)
    return origData, nil
}

func main() {
    key := []byte("0123456789abcdef")
    result, err := AesEncrypt([]byte("hello world"), key)
    if err != nil {
        panic(err)
    }
    fmt.Println(base64.StdEncoding.EncodeToString(result))
    origData, err := AesDecrypt(result, key)
    if err != nil {
        panic(err)
    }
    fmt.Println(string(origData))
}
```
 

<<<<<<< HEAD
#### 2.2、java语言利用RSA加解密和验签

代码如下：
```

package org.example;

import org.apache.commons.codec.binary.Base64;
import org.apache.commons.codec.digest.DigestUtils;

import javax.crypto.Cipher;
import java.nio.charset.StandardCharsets;
import java.security.*;
import java.security.spec.PKCS8EncodedKeySpec;
import java.security.spec.X509EncodedKeySpec;
import java.util.HashMap;
import java.util.Map;


public class SignatureUtil {

    public static final String PUBLIC_KEY_NAME = "publicKey";

    public static final String PRIVATE_KEY_NAME = "privateKey";

    private static final String SIGNATURE_ALGORITHM_SHA256WITHRSA = "SHA256withRSA";

    private static final String KEY_ALGORITHM_RSA = "RSA";

    //可选1024 或者2048
    private static final int KEY_SIZE = 1024;

    private static Signature getInstance(String algorithm) {
        try {
            return Signature.getInstance(algorithm);
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }

    /**
     * Sign with SHA256withRSA algorithm
     *
     * @param privateKey
     * @param plainText
     * @return
     */
    public static byte[] signSHA256withRSA(String privateKey, String plainText) {
        Signature signature = getInstance(SIGNATURE_ALGORITHM_SHA256WITHRSA);
        PrivateKey key = transformPrivateKey(privateKey);
        byte[] digestData = DigestUtils.sha256(plainText);
        try {
            signature.initSign(key);
            signature.update(digestData);
            return signature.sign();
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }

    /**
     * Verify sign with SHA256withRSA algorithm
     *
     * @param publicKey
     * @param plainText
     * @param signData
     * @return
     */
    public static boolean verifySignSHA256withRSA(String publicKey, String plainText, byte[] signData) {
        Signature signature = getInstance(SIGNATURE_ALGORITHM_SHA256WITHRSA);
        PublicKey key = transformPublicKey(publicKey);
        byte[] digestData = DigestUtils.sha256(plainText);
        try {
            signature.initVerify(key);
            signature.update(digestData);
            return signature.verify(signData);
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }

    /**
     * Transform String private key to PrivateKey with Map cache
     *
     * @param privateKey
     * @return
     */
    public static PrivateKey transformPrivateKey(String privateKey) {
        PKCS8EncodedKeySpec pkcs8EncodedKeySpec = new PKCS8EncodedKeySpec(Base64.decodeBase64(privateKey));
        try {
            KeyFactory keyFactory = KeyFactory.getInstance(KEY_ALGORITHM_RSA);
            return keyFactory.generatePrivate(pkcs8EncodedKeySpec);
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }


    /**
     * Transform String public key to PublicKey with Map cache
     *
     * @param publicKey
     * @return
     */
    public static PublicKey transformPublicKey(String publicKey) {
        X509EncodedKeySpec x509KeySpec = new X509EncodedKeySpec(Base64.decodeBase64(publicKey));
        try {
            KeyFactory keyFactory = KeyFactory.getInstance(KEY_ALGORITHM_RSA);
            return keyFactory.generatePublic(x509KeySpec);
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }

    /**
     * Generate key pair
     *
     * @return Map
     * get(SignatureUtil.PUBLIC_KEY_NAME) get base64 public key
     * get(SignatureUtil.PRIVATE_KEY_NAME) get base64 private key
     */
    public static Map<String, String> generateKeyPairBase64() {
        KeyPairGenerator keyPairGenerator;
        try {
            keyPairGenerator = KeyPairGenerator.getInstance(KEY_ALGORITHM_RSA);
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
        keyPairGenerator.initialize(KEY_SIZE);
        KeyPair keyPair = keyPairGenerator.generateKeyPair();
        PublicKey publicKey = keyPair.getPublic();
        PrivateKey privateKey = keyPair.getPrivate();
        Map<String, String> keyMap = new HashMap<>();
        keyMap.put(PUBLIC_KEY_NAME, Base64.encodeBase64String(publicKey.getEncoded()));
        keyMap.put(PRIVATE_KEY_NAME, Base64.encodeBase64String(privateKey.getEncoded()));
        return keyMap;
    }

    //公钥加密
    public static String encrypt(String plainText, String publicKeyStr) {
        try {
            Cipher cipher = Cipher.getInstance(KEY_ALGORITHM_RSA);
            PublicKey publickKey = transformPublicKey(publicKeyStr);
            if (publickKey != null) {
                cipher.init(Cipher.ENCRYPT_MODE, publickKey);
                byte[] bytes = cipher.doFinal(plainText.getBytes(StandardCharsets.UTF_8));
                return Base64.encodeBase64String(bytes);
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
        return null;
    }

    //私钥解密
    public static String decrypt(String encryptText, String privateKeyStr) {
        try {
            //根据需要选择分组模式、填充。比如：RSA/ECB/NoPadding
            Cipher cipher = Cipher.getInstance(KEY_ALGORITHM_RSA);
            PrivateKey privateKey = transformPrivateKey(privateKeyStr);
            if (privateKey != null) {
                cipher.init(Cipher.DECRYPT_MODE, privateKey);
                byte[] bytes = cipher.doFinal(Base64.decodeBase64(encryptText));
                return new String(bytes, StandardCharsets.UTF_8);
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
        return null;
    }
}

```

测试代码如下：

```
package org.example;

import java.util.Map;

public class SignatureUtilTest {

    public static void main(String[] args) {
        String text = "这个最原始的数据";
        Map<String, String> map = SignatureUtil.generateKeyPairBase64();
        String privateKey = map.get("privateKey");
        String publicKey = map.get("publicKey");
        System.out.println("生成了私钥是:");
        System.out.println(privateKey);
        System.out.println("生成了公钥是:");
        System.out.println(publicKey);
        System.out.println("开始进行私钥签名，和，公钥验证签名");
        byte[] signKey = SignatureUtil.signSHA256withRSA(privateKey, text);
        boolean ok = SignatureUtil.verifySignSHA256withRSA(publicKey, text, signKey);
        System.out.println("验证结果是：");
        System.out.println(ok);
        System.out.println("---------验签逻辑结束，开始加解密----------");
        String ciphertext = SignatureUtil.encrypt(text, publicKey);
        System.out.println("加密后的密文是：");
        System.out.println(ciphertext);
        String newText = SignatureUtil.decrypt(ciphertext, privateKey);
        System.out.println("解后的密文是：");
        System.out.println(newText);
    }
}

```
=======

>>>>>>> parent of f0aa510... 1

