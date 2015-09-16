# 概述
##### RSA是目前最有影响力的公钥加密算法，该算法基于一个十分简单的数论事实：将两个大素数相乘十分容易，但那时想要对其乘积进行因式分解却极其困 难，因此可以将乘积公开作为加密密钥，即公钥，而两个大素数组合成私钥。公钥是可发布的供任何人使用，私钥则为自己所有，供解密之用。关于RSA其它需要了解的知识，参考维基百科:http://zh.wikipedia.org/zh-cn/RSA%E5%8A%A0%E5%AF%86%E6%BC%94%E7%AE%97%E6%B3%95 

#####在项目开发中对于一些比较敏感的信息需要对其进行加密处理，我们就可以使用RSA这种非对称加密算法来对数据进行加密处理。

# 使用
####秘钥对的生成

#####1、我们可以在代码里随机生成密钥对

###### 
    /** 
     * 随机生成RSA密钥对 
     *  
     * @param keyLength 
     *            密钥长度，范围：512～2048<br> 
     *            一般1024 
     * @return 
     */  
    public static KeyPair generateRSAKeyPair(int keyLength)  
    {  
        try  
        {  
            KeyPairGenerator kpg = KeyPairGenerator.getInstance(RSA);  
            kpg.initialize(keyLength);  
            return kpg.genKeyPair();  
        } catch (NoSuchAlgorithmException e)  
        {  
            e.printStackTrace();  
            return null;  
        }  
    }  
#####2、通过OpenSSl工具生成密钥对

生成私钥：genrsa -out rsa_private_key.pem 1024 
生成公钥：rsa -in rsa_private_key.pem -out rsa_public_key.pem -pubout

 这样密钥就基本生成了，不过这样密钥对的私钥是无法在代码中直接使用的，要想使用它需要借助RSAPrivateKeyStructure这个类，java是不自带的。所以为了方便使用，我们需要对私钥进行PKCS#8编码，
 
 命令如下：pkcs8 -topk8 -in rsa_private_key.pem -out pkcs8_rsa_private_key.pem -nocrypt 
