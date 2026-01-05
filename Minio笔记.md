https://developer.aliyun.com/article/1648673 安装步骤

联想PC：


```java
D:\minio\bin>setx MINIO_ROOT_PASSWORD Admin!@#456

成功: 指定的值已得到保存。

D:\minio\bin>.\minio.exe server D:\minio\data --console-address "127.0.0.1:9000" --address "
FATAL Unable to split host port : invalid argument

D:\minio\bin>.\minio.exe server D:\minio\data --console-address "127.0.0.1:9000" --address "127.0.0.1:9090"
    
    
    
PS D:\minio\bin> .\mc.exe alias set 'myminio' 'http://127.0.0.1:9090' 'minioadmin' 'minioadmin'
Added `myminio` successfully.
PS D:\minio\bin> .\mc admin user add ALIAS ACCESSKEY SECRETKEY
Added user `FBW6KjfmRXI5IaCMjAzp` successfully.
```

