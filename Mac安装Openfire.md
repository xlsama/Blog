# Mac安装Openfire

[下载地址](https://www.igniterealtime.org/downloads/)   下载完双击安装即可

## 启动失败解决方案

![image-20210514164744135](https://i.loli.net/2021/05/14/uhwjFGvSBK1qXJz.png)

![image-20210514164854679](https://i.loli.net/2021/05/14/rDyUHkSMAzGNbmv.png)

解决方法：

依次执行以下代码：

1. ```shell
   sudo chmod -R 777 /usr/local/openfire/bin
   ```

2. ```shell
   sudo su
   # ctrl+d 退出
   ```

3. ```shell
   cd /usr/local/openfire/bin
   ```

4. ```shell
   export JAVA_HOME=`/usr/libexec/java_home`
   # 注意标点
   ```

5. ```shell
   echo $JAVA_HOME=/Library/Java/JavaVirtualMachines/zulu-16.jdk/Contents/Home
   # ⚠️注意：这里填的是你的jdk版本，可以输入 /usr/libexec/java_home 来查看java_home安装目录
   ```

6. ```shell
   cd /usr/local/openfire/bin
   ```

7. ```shell
   ./openfire.sh
   ```

然后打开 http://localhost:9090 查看

![image-20210514170223657](https://i.loli.net/2021/05/14/ebdRhk2mLIunG6S.png)

![image-20210514170349733](https://i.loli.net/2021/05/14/rYc8NbDC6I2pVF7.png)

## 问题

https://discourse.igniterealtime.org/t/fail-to-start-openfire-4-3-0-on-macos-mojave-version-10-14-2/83863