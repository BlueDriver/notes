# java笔记

## Install JDK

### Windows

...

### Linux
1. unzip 
`tar -zxvf jdk-8u151-linux-x64.tar.gz`

2. add to etc/environment
```
PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:$JAVA_HOME/bin"
export CLASSPATH=.:$JAVA_HOME/lib:$JAVA_HOME/jre/lib
export JAVA_HOME=/home/soft/jdk/jdk1.8.0_151
```
3. then execute
`source /etc/environment`


4. add to /etc/profile for set Java environment
```
export JAVA_HOME=/home/soft/jdk/jdk1.8.0_151
export JRE_HOME=$JAVA_HOME/jre
export CLASSPATH=.:$JAVA_HOME/lib:$JRE_HOME/lib:$CLASSPATH
export PATH=$JAVA_HOME/bin:$JRE_HOME/bin:$PATH
```
5. then execute 
`source /etc/profile`

6. test java
```
java -version
```

## 异常
### 异常说明
#### Disable SSL`Certificates does not conform to algorithm constraints ...`

> JDK安全设置，在jre/lib/security/java.security, 取消禁用加密算法

```
remove:
jdk.tls.disabledAlgorithms=SSLv3, RC4, MD5withRSA, DH keySize < 1024, \
    EC keySize < 224, DES40_CBC, RC4_40
```

