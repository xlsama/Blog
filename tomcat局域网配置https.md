# Tomcat配置SSL证书实现局域网下 https 访问

# 一、创建证书

## 1. 下载 [mkcert](https://github.com/FiloSottile/mkcert) 

mkcert 是一个简单的零配置工具，可以使用您想要的任何名称制作本地信任的开发证书。

## 2. 生成本地证书

**以管理员身份运行：**

```shell
mkcert -install
```

结果为`Created a new local CA 💥`表示生成成功

```shell
mkcert -install
Created a new local CA 💥
The local CA is now installed in the system trust store! ⚡️
The local CA is now installed in Java's trust store! ☕️
```

## 3. 生成自签证书

可以先创建一个空目录来存放生成的文件，然后运行：

```shell
mkcert 192.166.1.25
```

会在当前目录下生成一个`name.pem`和`name-key.pem`文件

```shell
PS D:\mkcert> mkcert 192.166.1.25

Created a new certificate valid for the following names 📜
 - "192.166.1.25"

The certificate is at "./192.166.1.25.pem" and the key at "./192.166.1.25-key.pem" ✅

It will expire on 20 April 2023 🗓
```

# 二、配置tomcat

## 1. 下载 [OpenSSL](http://slproweb.com/products/Win32OpenSSL.html)

安装好以后进入`bin`目录，运行`openssl.exe` 启动程序

## 2. 使用openssl转换pem为pfx证书

```shell
# 命令格式：openssl pkcs12 -export -out 输出文件名称 -inkey key文件名称 -in pem文件名称

OpenSSL> pkcs12 -export -out D:\mkcert\mycert.pfx -inkey D:\mkcert\192.166.1.25-key.pem -in D:\mkcert\192.166.1.25.pem
```

回车执行会提示设置密码，执行完成后就会在当前目录生成`.pfx`证书文件了

## 3. 修改tomcat配置文件

### 3.1. 下载[tomcat](http://tomcat.apache.org/)

我这里用的版本是`apache-tomcat-8.5.61`（**tomcat8.5以上的版本不好使！！！**）

### 3.2. 修改 conf/server.xml

1. 修改 `tomcat` 访问端口，将 8080 改为 80，在浏览器访问时不需要添加端口。将 `redirectPort="8443"`的端口改为 443，因为 https 的端口为 443。最终修改内容如下：

```xml
    <!-- A "Connector" represents an endpoint by which requests are received
         and responses are returned. Documentation at :
         Java HTTP Connector: /docs/config/http.html
         Java AJP  Connector: /docs/config/ajp.html
         APR (HTTP/AJP) Connector: /docs/apr.html
         Define a non-SSL/TLS HTTP/1.1 Connector on port 8080
    -->
    <Connector port="80" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="443" />
```

2. 添加 `ssl`证书和密码，将 `port` 值改为 443，`keystoreFile` 的值为之前生成的 *.pfx文件,`keystorePass`的值为生成.pfx文件时输入的密码，`keystoreType` 的值为 `pfx-password.txt` 的内容，最终修改内容如下：

```xml
<!-- Define an SSL/TLS HTTP/1.1 Connector on port 8443
         This connector uses the NIO implementation. The default
         SSLImplementation will depend on the presence of the APR/native
         library and the useOpenSSL attribute of the
         AprLifecycleListener.
         Either JSSE or OpenSSL style configuration may be used regardless of
         the SSLImplementation selected. JSSE style configuration is used below.
-->
<Connector port="443"
    protocol="org.apache.coyote.http11.Http11Protocol"
    SSLEnabled="true"
    scheme="https"
    secure="true"
    keystoreFile="D:\mkcert\mycert.pfx"
    keystoreType="PKCS12"
    keystorePass="123123"
    clientAuth="false"
    SSLProtocol="TLSv1+TLSv1.1+TLSv1.2"
    ciphers="TLS_RSA_WITH_AES_128_CBC_SHA,TLS_RSA_WITH_AES_256_CBC_SHA,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256,TLS_RSA_WITH_AES_128_CBC_SHA256,TLS_RSA_WITH_AES_256_CBC_SHA256"/>
```

### 3.3. 修改 conf/web.xml

在 `conf/web.xml`的最底部，`welcome-file-list` 下面添加如下内容，可从 http 跳转到 https

```xml
<login-config>
	<!-- Authorization setting for SSL -->
	<auth-method>CLIENT-CERT</auth-method>
	<realm-name>Client Cert Users-only Area</realm-name>
</login-config>
<security-constraint>
	<!-- Authorization setting for SSL -->
	<web-resource-collection >
		<web-resource-name >SSL</web-resource-name>
		<url-pattern>/*</url-pattern>
	</web-resource-collection>
	<user-data-constraint>
		<transport-guarantee>CONFIDENTIAL</transport-guarantee>
	</user-data-constraint>
</security-constraint>
```

### 3.4. 重启tomcat后，访问即可

# 三、局域网内访问

## 1. 查看CA证书的存放路径

```shell
mkcert -CAROOT
```

## 2. 分享证书给局域网下的其他电脑

可以看到CA路径下有两个文件`rootCA-key.pem`和`rootCA.pem`两个文件，用户需要信任`rootCA.pem`这个文件。将`rootCA.pem`拷贝一个副本，并命名为`rootCA.crt`(因为windows并不识别`pem`扩展名，并且Ubuntu也不会将`pem`扩展名作为CA证书文件对待)，将`rootCA.crt`文件分发给其他用户，手工导入。

windows导入证书的方法是双击这个文件，在证书导入向导中将证书导入**`受信任的根证书颁发机构`**:

MacOS的做法也一样，同样选择将CA证书导入到受信任的根证书办法机构。

Ubuntu的做法可以将证书文件(必须是`crt`后缀)放入`/usr/local/share/ca-certificates/`，然后执行`sudo update-ca-certificates`处。

Android和IOS信任CA证书的做法参考[官方文档](https://links.jianshu.com/go?to=https%3A%2F%2Fgithub.com%2FFiloSottile%2Fmkcert%23mobile-devices)。