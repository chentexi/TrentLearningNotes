# crypto

提供加密操作的类和接口。 此程序包中定义的加密操作包括加密，密钥生成和密钥协议以及消息验证代码（MAC）生成。

对加密的支持包括对称，非对称，块和流密码。 该软件包还支持安全流和密封对象。

此包中提供的许多类都是基于提供程序的。 该类本身定义了应用程序可以编写的编程接口。 然后，实现本身可以由独立的第三方供应商编写，并根据需要无缝地插入。 因此，应用程序开发人员可以利用任意数量的基于提供程序的实现，而无需添加或重写代码。

| 类                                                           | 描述                                                         |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [Cipher](https://www.apiref.com/java11-zh/java.base/javax/crypto/Cipher.html) | 此类提供用于加密和解密的加密密码的功能。                     |
| [CipherInputStream](https://www.apiref.com/java11-zh/java.base/javax/crypto/CipherInputStream.html) | CipherInputStream由InputStream和Cipher组成，因此read（）方法返回从底层InputStream读入但已由Cipher另外处理的数据。 |
| [CipherOutputStream](https://www.apiref.com/java11-zh/java.base/javax/crypto/CipherOutputStream.html) | CipherOutputStream由OutputStream和Cipher组成，因此write（）方法在将数据写入底层OutputStream之前首先处理数据。 |
| [CipherSpi](https://www.apiref.com/java11-zh/java.base/javax/crypto/CipherSpi.html) | 此类定义 `Cipher`类的 （ SPI ）。                            |
| [EncryptedPrivateKeyInfo](https://www.apiref.com/java11-zh/java.base/javax/crypto/EncryptedPrivateKeyInfo.html) | 此类实现PKCS＃8中定义的 `EncryptedPrivateKeyInfo`类型。      |
| [ExemptionMechanism](https://www.apiref.com/java11-zh/java.base/javax/crypto/ExemptionMechanism.html) | 此类提供免除机制的功能，其示例包括 *密钥恢复* ， *密钥弱化*和 *密钥托管* 。 |
| [ExemptionMechanismSpi](https://www.apiref.com/java11-zh/java.base/javax/crypto/ExemptionMechanismSpi.html) | 此类定义 `ExemptionMechanism`类的 （ SPI ）。                |
| [KeyAgreement](https://www.apiref.com/java11-zh/java.base/javax/crypto/KeyAgreement.html) | 此类提供密钥协议（或密钥交换）协议的功能。                   |
| [KeyAgreementSpi](https://www.apiref.com/java11-zh/java.base/javax/crypto/KeyAgreementSpi.html) | 此类定义 `KeyAgreement`类的 （ SPI ）。                      |
| [KeyGenerator](https://www.apiref.com/java11-zh/java.base/javax/crypto/KeyGenerator.html) | 此类提供秘密（对称）密钥生成器的功能。                       |
| [KeyGeneratorSpi](https://www.apiref.com/java11-zh/java.base/javax/crypto/KeyGeneratorSpi.html) | 此类定义 `KeyGenerator`类的 （ SPI ）。                      |
| [Mac](https://www.apiref.com/java11-zh/java.base/javax/crypto/Mac.html) | 此类提供“消息验证代码”（MAC）算法的功能。                    |
| [MacSpi](https://www.apiref.com/java11-zh/java.base/javax/crypto/MacSpi.html) | 此类定义 `Mac`类的 （ SPI ）。                               |
| [NullCipher](https://www.apiref.com/java11-zh/java.base/javax/crypto/NullCipher.html) | NullCipher类是一个提供“身份密码”的类 - 一个不转换纯文本的密码。 |
| [SealedObject](https://www.apiref.com/java11-zh/java.base/javax/crypto/SealedObject.html) | 该类使程序员能够使用加密算法创建对象并保护其机密性。         |
| [SecretKeyFactory](https://www.apiref.com/java11-zh/java.base/javax/crypto/SecretKeyFactory.html) | 此类表示密钥的工厂。                                         |
| [SecretKeyFactorySpi](https://www.apiref.com/java11-zh/java.base/javax/crypto/SecretKeyFactorySpi.html) | 此类定义 `SecretKeyFactory`类的 （ SPI ）。                  |

| 异常                                                         | 描述                                                         |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [AEADBadTagException](https://www.apiref.com/java11-zh/java.base/javax/crypto/AEADBadTagException.html) | 当以AEAD模式（例如GCM / CCM）运行的[`Cipher`](https://www.apiref.com/java11-zh/java.base/javax/crypto/Cipher.html)无法验证提供的身份验证标记时，将引发此异常。 |
| [BadPaddingException](https://www.apiref.com/java11-zh/java.base/javax/crypto/BadPaddingException.html) | 当输入数据需要特定的填充机制但数据没有正确填充时，抛出此异常。 |
| [ExemptionMechanismException](https://www.apiref.com/java11-zh/java.base/javax/crypto/ExemptionMechanismException.html) | 这是一般的ExemptionMechanism例外。                           |
| [IllegalBlockSizeException](https://www.apiref.com/java11-zh/java.base/javax/crypto/IllegalBlockSizeException.html) | 当提供给块密码的数据长度不正确时，即与密码的块大小不匹配时，抛出此异常。 |
| [NoSuchPaddingException](https://www.apiref.com/java11-zh/java.base/javax/crypto/NoSuchPaddingException.html) | 当请求特定填充机制但在环境中不可用时，抛出此异常。           |
| [ShortBufferException](https://www.apiref.com/java11-zh/java.base/javax/crypto/ShortBufferException.html) | This exception is thrown when an output buffer provided by the user is too short to hold the operation result. |