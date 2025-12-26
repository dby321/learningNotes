# JavaNIO笔记

## 概述

IO的分类：

- BIO
- NIO(Non-blocking IO/New IO) 基于IO多路复用的同步非阻塞IO模型
- AIO

重要概念：

- Channel 可以类比BIO中的Stream,流是单向的，通道是双向的
  - FileChannel
  - DatagramChannel UDP
  - SocketChannel TCP
  - ServerSocketChannel 
- Selector
- Buffer
- Pipe
- FileLock

## 示例

- filp()切换读模式
- clear()切换写模式

https://javabetter.cn/nio/BIONIOAIO.html#%E4%B8%89%E7%A7%8D-io-%E7%9A%84%E5%8C%BA%E5%88%AB

```java
package com.test;

import java.io.FileNotFoundException;
import java.io.IOException;
import java.io.RandomAccessFile;
import java.nio.ByteBuffer;
import java.nio.channels.FileChannel;

/**
 * @author waterplants/dby
 * @description
 * @date 2025/12/26
 */
public class FileChannelDemo1 {
    /**
     * NIO文件读取示例程序
     * 使用FileChannel和ByteBuffer来读取文件内容
     * 
     * @param args 命令行参数
     * @throws IOException 文件读取过程中可能抛出的IO异常
     */
    public static void main(String[] args) throws IOException {
      //NIO
        RandomAccessFile accessFile=new RandomAccessFile("D:\\idea_workspace\\****\\****-business\\pom.xml","rw");
        FileChannel channel=accessFile.getChannel();
        
        // 创建一个容量为1024字节的ByteBuffer缓冲区
        ByteBuffer allocate = ByteBuffer.allocate(1024);
        int read = channel.read(allocate);
        
        // 循环读取文件内容直到文件末尾
        while(read!=-1){
            System.out.println("已读取："+read);
            
            // 切换缓冲区模式，准备读取数据
            allocate.flip();
            
            // 逐字节输出缓冲区中的内容
            while(allocate.hasRemaining()){
                System.out.print((char) allocate.get());
            }
            System.out.println("---------");
            
            // 清空缓冲区，准备下一次读取
            allocate.clear();
            read=channel.read(allocate);
        }
        accessFile.close();
        System.out.println("读取完毕");
    }
}

```

输出

```java
已连接到地址为 ''127.0.0.1:7969'，传输: '套接字'' 的目标虚拟机
已读取：1024
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>****</artifactId>
        <groupId>com.****</groupId>
        <version>3.8.8</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>****-business</artifactId>
    <dependencies>
        <dependency>
            <groupId>com.****</groupId>
            <artifactId>****-common</artifactId>
        </dependency>

        <dependency>
            <groupId>com.****</groupId>
            <artifactId>****-framework</artifactId>
        </dependency>
        <dependency>
            <groupId>cn.hutool</groupId>
            <artifactId>hutool-all</artifactId>
            <version>5.5.8</version>
            <scope>compile</s---------
已读取：222
cope>
        </dependency>
    </dependencies>

    <properties>
        <maven.compiler.source>11</maven.compiler.source>
        <maven.compiler.target>11</maven.compiler.target>
    </properties>

</project>
---------
读取完毕
已与地址为 ''127.0.0.1:7969'，传输: '套接字'' 的目标虚拟机断开连接

进程已结束，退出代码为 0

```

