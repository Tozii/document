# BouncyCastle.Crypto.dll 签名&验签
```c#
        /// <summary>
        /// 基于BouncyCastle的RSA签名
        /// </summary>
        /// <param name="data"></param>
        /// <param name="privateKeyJava"></param>
        /// <param name="hashAlgorithm">JAVA的和.NET的不一样，如：MD5(.NET)等同于MD5withRSA(JAVA)</param>
        /// <param name="encoding"></param>
        /// <returns></returns>
        public string RSASignJavaBouncyCastle(string data, string privateKeyJava, string hashAlgorithm = "MD5withRSA", string encoding = "UTF-8")
        {
            RsaKeyParameters privateKeyParam = (RsaKeyParameters)PrivateKeyFactory.CreateKey(Convert.FromBase64String(privateKeyJava));
            ISigner signer = SignerUtilities.GetSigner("SHA256WithRSA");
            signer.Init(true, privateKeyParam);
            var dataByte = Encoding.GetEncoding(encoding).GetBytes(data);
            signer.BlockUpdate(dataByte, 0, dataByte.Length);
            //return Encoding.GetEncoding(encoding).GetString(signer.GenerateSignature()); //签名结果 非Base64String
            return Convert.ToBase64String(signer.GenerateSignature());
        }

        /// <summary>
        /// 基于BouncyCastle的RSA验签
        /// </summary>
        /// <param name="data">源数据</param>
        /// <param name="publicKeyJava"></param>
        /// <param name="signature">base64签名</param>
        /// <param name="hashAlgorithm">JAVA的和.NET的不一样，如：MD5(.NET)等同于MD5withRSA(JAVA)</param>
        /// <param name="encoding"></param>
        /// <returns></returns>
        public bool VerifyJavaBouncyCastle(string data, string publicKeyJava, string signature, string hashAlgorithm = "MD5withRSA", string encoding = "UTF-8")
        {
            RsaKeyParameters publicKeyParam = (RsaKeyParameters)PublicKeyFactory.CreateKey(Convert.FromBase64String(publicKeyJava));
            ISigner signer = SignerUtilities.GetSigner("SHA256WithRSA");
            signer.Init(false, publicKeyParam);
            byte[] dataByte = Encoding.GetEncoding(encoding).GetBytes(data);
            signer.BlockUpdate(dataByte, 0, dataByte.Length);
            //byte[] signatureByte = Encoding.GetEncoding(encoding).GetBytes(signature);// 非Base64String
            byte[] signatureByte = Convert.FromBase64String(signature);
            return signer.VerifySignature(signatureByte);
        }
```