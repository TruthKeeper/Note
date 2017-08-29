- 从浏览器导出证书.cer
- 生成Android支持的格式.bks [工具](http://www.bouncycastle.org/latest_releases.html)

```
keytool -importcert -v -trustcacerts -alias 别名  
-file cer或crt证书的全地址  
-keystore 生成后bks文件的位置,建议写全地址 -storetype BKS 
-providerclass org.bouncycastle.jce.provider.BouncyCastleProvider \  
-providerpath 上面下载JCE Provider包的位置 -storepass 生成后证书的密码  
```

- [下载工具KeyStore Explorer](http://keystore-explorer.sourceforge.NET/)，打开原来的BKS证书，选择Tools->Change Type，选择BKS-V1

其余配置

[SSLProtocolException: SSL handshake](http://blog.csdn.net/guozhaohui628/article/details/54571176)

[Https踩坑记录](http://www.jianshu.com/p/41bb549317ff)