## 简单安全相关


## 签名和验签

数据在网络传输的时候，可能会被劫持，这个时候，不发份子可能会把数据进行修改。比如说，传递数据是"1",他修改为"1000"。这种是无法接收的。所以一般需要把书籍进行签名。签名后另一端再进行验证签名。

某些业务数据 + 算法 = 字符串

简单签名算法如：签名端把业务数据和字符串传递过去，另一端再次签名，然后对比签名结果是否相同。


#### md5签名

利用需要加签的数据，生成一个固定的字符串，或者加上一个盐(secret,salt),把该字符串进行,md5后做base64，得到一个字符串。2端都这样做，然后对比这个字符串。

#### HmacSha1

这个也是一个签名算法，比如开源的apollo就是用这个算法。HmacSha1Utils.signString(stringToSign, secret);这里的secret也是双方约定的一个随机字符串


## 加解密

* 业务数据 + 盐 + 算法 = 密文
* 密文 + 盐 + 算法 = 业务数据

#### AES加解密

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



