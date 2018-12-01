# BouncyCastle.Crypto
### > 签名一
```c#
/// <summary>
/// 签名
/// </summary>
/// <param name="str">需签名的数据</param>
/// <returns>签名后的值</returns>
public string Sign(string str)
{
    //根据需要加签时的哈希算法转化成对应的hash字符节
    byte[] bt = Encoding.GetEncoding("utf-8").GetBytes(str);
    //byte[] bt = Convert.FromBase64String(str);
    var sha256 = new SHA256CryptoServiceProvider();
    byte[] rgbHash = sha256.ComputeHash(bt);

    RSACryptoServiceProvider key = new RSACryptoServiceProvider();
    key.FromXmlString(privateKey);
    RSAPKCS1SignatureFormatter formatter = new RSAPKCS1SignatureFormatter(key);
    formatter.SetHashAlgorithm("SHA256");//此处是你需要加签的hash算法，需要和上边你计算的hash值的算法一致，不然会报错。
    byte[] inArray = formatter.CreateSignature(rgbHash);
    return Convert.ToBase64String(inArray);
}
```
### > 签名二
```c#
/// <summary>
/// 签名
/// </summary>
/// <param name="str">需签名的数据</param>
/// <returns>签名后的值</returns>
public string Sign(string str)
{
    CspParameters CspParameters = new CspParameters();
    RSACryptoServiceProvider RSA = new RSACryptoServiceProvider(2048, CspParameters);
    byte[] bytes = Encoding.UTF8.GetBytes(str);
    //byte[] bytes = Convert.FromBase64String(str);
    RSA.FromXmlString(privateKey);
    byte[] sign = RSA.SignData(bytes, "SHA256");
    return Convert.ToBase64String(sign);
}
```
### > 验签
```c#
/// <summary>
/// 签名验证
/// </summary>
/// <param name="str">待验证的字符串</param>
/// <param name="sign">加签之后的字符串</param>
/// <returns>签名是否符合</returns>
public bool SignCheck(string str, string sign)
{
    try
    {
        byte[] bt = Encoding.GetEncoding("utf-8").GetBytes(str);
        var sha256 = new SHA256CryptoServiceProvider();
        byte[] rgbHash = sha256.ComputeHash(bt);

        RSACryptoServiceProvider key = new RSACryptoServiceProvider();
        key.FromXmlString(publicKey);
        RSAPKCS1SignatureDeformatter deformatter = new RSAPKCS1SignatureDeformatter(key);
        deformatter.SetHashAlgorithm("SHA256");
        byte[] rgbSignature = Convert.FromBase64String(sign);
        if (deformatter.VerifySignature(rgbHash, rgbSignature))
        {
            return true;
        }
        return false;
    }
    catch(Exception ex)
    {
        return false;
    }
}
```

