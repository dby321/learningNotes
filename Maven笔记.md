# Maven教程

[Baeldung-maven教程](https://www.baeldung.com/maven-series)

# Apache Maven 教程

##  1. 简介

构建一个软件项目通常包括以下任务：下载依赖项、将额外的 jar 文件添加到类路径中、将源代码编译成二进制代码、运行测试、将编译后的代码打包成可部署的工件，如 JAR、WAR 和 ZIP 文件，并将这些工件部署到应用程序服务器或仓库。

[Apache Maven](https://maven.apache.org/) 自动执行这些任务，最大程度地减少人在手动构建软件时犯错误的风险，并将我们的代码编译和打包的工作与代码构建的工作分离。

在这个教程中，我们将探索这个强大的工具，用于描述、构建和管理 Java 软件项目，该工具使用一个中心信息——XML 编写的项目对象模型（POM）——。

##  2. 为什么使用 Maven？

Maven 的关键特性包括：

- 遵循最佳实践的简单项目设置： Maven 尽可能避免配置，通过提供项目模板（称为 原型）
- <强烈 id=0>依赖管理：它包括自动更新、下载和验证兼容性，以及报告依赖关系闭合（也称为传递依赖关系）
- 项目依赖和插件之间的隔离：使用 Maven，项目依赖从依赖仓库中检索，而任何插件的依赖从插件仓库中检索，当插件开始下载额外的依赖时，导致冲突更少
- 中央仓库系统：：项目依赖可以从本地文件系统或公共仓库加载，例如 [Apache Maven 中央仓库](https://search.maven.org/)

为了了解如何在您的系统上安装 Maven，请查看[在 Baeldung 上的这篇教程](https://www.baeldung.com/install-maven-on-windows-linux-mac)。

##  3. 项目对象模型

Maven 项目的配置是通过一个项目对象模型（POM）完成的，该模型由一个pom.xml文件表示。《POM》描述了项目，管理依赖关系，并为构建软件配置插件。

The POM 还定义了多模块项目的模块之间的关系。让我们看看典型 POM 文件的基本结构：

```xml
<project>
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.baeldung</groupId>
    <artifactId>baeldung</artifactId>
    <packaging>jar</packaging>
    <version>1.0-SNAPSHOT</version>
    <name>com.baeldung</name>
    <url>http://maven.apache.org</url>
    <dependencies>
        <dependency>
            <groupId>org.junit.jupiter</groupId>
            <artifactId>junit-jupiter-api</artifactId>
            <version>5.8.2</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
    <build>
        <plugins>
            <plugin>
            //...
            </plugin>
        </plugins>
    </build>
</project>Copy
```

让我们更仔细地看看这些结构。

###  3.1. 项目标识符

Maven 使用一组标识符，也称为坐标，来唯一标识一个项目并指定项目工件应该如何打包：

- groupId – 项目创建的公司或组的一个唯一基本名称
- artifactId – 项目的一个唯一名称
- 版本 – 该项目的版本
- 包装 – 一种包装方法（例如 WAR/JAR/ZIP）

这些中的前三个（groupId:artifactId:version）组合形成唯一的标识符，这是您指定项目将使用哪些外部库（例如 JAR）版本的机制。

###  3.2. 依赖项

这些项目使用的外部库被称为依赖项。Maven 中的依赖管理功能确保从中央仓库自动下载这些库，因此您无需在本地存储它们。

这是 Maven 的一个关键特性，并提供了以下好处：

- 通过显著减少从远程仓库下载的次数，使用更少的存储空间
- 使检查项目更快
- 提供了一个有效的平台，用于在您的组织内部以及更广泛范围内交换二进制工件，无需每次都从源代码构建工件

为了声明对外部库的依赖，您需要提供库的 groupId、artifactId 和 version。让我们来看一个例子：

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-core</artifactId>
    <version>5.3.16</version>
</dependency>Copy
```

当 Maven 处理依赖项时，它将 Spring Core 库下载到您的本地 Maven 仓库中。

###  3.3. 代码仓库

Maven 中的仓库用于存储不同类型的构建工件和依赖项。默认的本地仓库位于用户主目录下的.m2/repository文件夹中。

如果本地仓库中有该工件或插件，Maven 会使用它。否则，它将从中央仓库下载并存储在本地仓库中。默认的中央仓库是[ Maven 中央仓库](https://mvnrepository.com/search?q=centra)。

一些库，如 JBoss 服务器，在中央仓库不可用，但在备用仓库可用。对于这些库，您需要在pom.xml文件中提供备用仓库的 URL：

```xml
<repositories>
    <repository>
        <id>JBoss repository</id>
        <url>http://repository.jboss.org/nexus/content/groups/public/</url>
    </repository>
</repositories>Copy
```

请注意，您可以在项目中使用多个仓库。

###  3.4. 属性

自定义属性可以帮助您使 pom.xml 文件更容易阅读和维护。在经典的使用场景中，您会使用自定义属性来定义项目中依赖项的版本。

Maven 属性是值占位符，可以通过使用${name}的表示法在任何pom.xml内部访问它们，其中name是属性。

 让我们看看一个例子：

```xml
<properties>
    <spring.version>5.3.16</spring.version>
</properties>

<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-core</artifactId>
        <version>${spring.version}</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>${spring.version}</version>
    </dependency>
</dependencies>Copy
```

现在如果您想将 Spring 升级到新版本，只需更改<spring.version>属性标签内的值，所有在<version>标签中使用该属性的依赖项都将更新。

属性也常用于定义构建路径变量：

```xml
<properties>
    <project.build.folder>${project.build.directory}/tmp/</project.build.folder>
</properties>

<plugin>
    //...
    <outputDirectory>${project.resources.build.folder}</outputDirectory>
    //...
</plugin>Copy
```

###  3.5. 构建

构建部分也是 Maven POM。 的一个非常重要的部分。它提供了关于默认 Maven 目标、编译项目的目录以及应用程序的最终名称的信息。默认的构建部分看起来像这样：

```xml
<build>
    <defaultGoal>install</defaultGoal>
    <directory>${basedir}/target</directory>
    <finalName>${artifactId}-${version}</finalName>
    <filters>
      <filter>filters/filter1.properties</filter>
    </filters>
    //...
</build>Copy
```

默认输出文件夹名为target，打包后的工件最终名称由artifactId和version组成，但您可以在任何时候更改它。

### 3.6. 使用 配置文件

Maven 的另一个重要特性是其对配置文件的支持。一个配置文件基本上是一组配置值。通过使用配置文件，您可以针对不同的环境（如生产/测试/开发）自定义构建：

```xml
<profiles>
    <profile>
        <id>production</id>
        <build>
            <plugins>
                <plugin>
                //...
                </plugin>
            </plugins>
        </build>
    </profile>
    <profile>
        <id>development</id>
        <activation>
            <activeByDefault>true</activeByDefault>
        </activation>
        <build>
            <plugins>
                <plugin>
                //...
                </plugin>
            </plugins>
        </build>
     </profile>
 </profiles>Copy
```

如上例所示，默认配置文件设置为开发。如果您想运行生产配置文件，可以使用以下 Maven 命令：

```bash
mvn clean install -PproductionCopy
```

## 4. Maven 构建生命周期

每个 Maven 构建都遵循一个指定的生命周期。您可以执行多个构建生命周期目标，包括编译项目的代码、创建包以及将存档文件安装到本地 Maven 依赖项仓库中的目标。

### 4.1. 生命周期阶段

以下列表显示了最重要的 Maven 生命周期阶段：

- 验证 – 检查项目的正确性
- 编译 – 将提供的源代码编译成二进制工件
- 测试 – 执行单元测试
- 包 – 将编译后的代码打包成归档文件
- 集成测试 – 执行额外的测试，这些测试需要打包
- 验证 – 检查包是否有效
- 安装 – 将包文件安装到本地 Maven 仓库中
- 部署 – 将包文件部署到远程服务器或仓库

### 4.2. 插件 和 目标

A Maven plugin is a collection of one or more goals. Goals are executed in phases, which helps to determine the order in which the goals are executed. 

The rich list of plugins that are officially supported by Maven is available [here](https://maven.apache.org/plugins/). There is also an interesting article on how to [build an executable JAR on Baeldung](https://www.baeldung.com/executable-jar-with-maven) using various plugins. 

To gain a better understanding of which goals are run in which phases by default, take a look at the [default Maven lifecycle bindings](http://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html#Built-in_Lifecycle_Bindings). 

只需调用一个命令即可通过上述任何阶段：

```bash
mvn <phase>Copy
```

例如，mvn clean install 将删除之前创建的 jar/war/zip 文件和编译后的类 (clean) 并执行安装新存档所需的所有阶段 (install)。

Please note that goals provided by plugins can be associated with different phases of the lifecycle. 

## 5. 您的第一个 Maven 项目

在这个部分，我们将使用 Maven 的命令行功能来创建一个 Java 项目。

### 5.1. 生成一个简单的 Java 项目

In order to build a simple Java project, let’s run the following command: 

```java
mvn archetype:generate 
  -DgroupId=com.baeldung 
  -DartifactId=baeldung 
  -DarchetypeArtifactId=maven-archetype-quickstart 
  -DarchetypeVersion=1.4 
  -DinteractiveMode=falseCopy
```

The groupId is a parameter indicating the group or individual that created a project, which is often a reversed company domain name. The artifactId is the base package name used in the project, and we use the standard archetype. Here we are using the latest archetype version to ensure our project is created with updated and latest structure. 

由于我们没有指定版本和打包类型，这些将默认设置 — 版本将被设置为 1.0-SNAPSHOT, 打包类型默认为 jar 。

If you don’t know which parameters to provide, you can always specify interactiveMode=true, so that Maven asks for all the required parameters. 

命令完成后，我们有一个包含一个App.java类的 Java 项目，这个类只是一个简单的“Hello World”程序，位于src/main/java文件夹中。

我们还有一个示例测试类在 src/test/java 中。本项目的 pom.xml 将与此类似：

```xml
<project>
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.baeldung</groupId>
    <artifactId>baeldung</artifactId>
    <version>1.0-SNAPSHOT</version>
    <name>baeldung</name>
    <url>http://www.example.com</url>
    <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.11</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
</project>Copy
```

如您所见，JUnit 依赖项默认提供。

### 5.2. 编译和打包项目

下一步是编译项目：

```plaintext
mvn compileCopy
```

Maven will run through all lifecycle phases needed by the compile phase to build the project’s sources. If you want to run only the test phase, you can use: 

```plaintext
mvn testCopy
```

Now let’s invoke the package phase, which will produce the compiled archive jar file: 

```bash
mvn packageCopy
```

### 5.3. 执行应用程序

最后，我们将使用exec-maven-plugin执行我们的 Java 项目。让我们在pom.xml中配置必要的插件：

```xml
<build>
    <sourceDirectory>src</sourceDirectory>
    <plugins>
        <plugin>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>3.12.1</version>
            <configuration>
                <source>1.8</source>
                <target>1.8</target>
            </configuration>
        </plugin>
        <plugin>
            <groupId>org.codehaus.mojo</groupId>
            <artifactId>exec-maven-plugin</artifactId>
            <version>3.1.0</version>
            <configuration>
                <mainClass>com.baeldung.java.App</mainClass>
            </configuration>
        </plugin>
    </plugins>
</build>Copy
```

The first plugin, [maven-compiler-plugin](https://mvnrepository.com/search?q=maven-compiler-plugin), is responsible for compiling the source code using Java version 1.8. The [exec-maven-plugin](https://mvnrepository.com/search?q=exec-maven-plugin) searches for the mainClass in our project. 

执行应用程序，我们运行以下命令：

```bash
mvn exec:javaCopy
```

##  6. 结论

在这篇文章中，我们讨论了 Apache Maven 构建工具的一些更受欢迎的功能。

All code examples on Baeldung are built using Maven, so you can easily check our [GitHub project website](https://github.com/eugenp/tutorials/tree/master/maven-modules/maven-simple) to see various Maven configurations. 

# Apache Maven 标准目录结构

##  1. 简介

Apache Maven 是最受欢迎的 Java 项目构建工具之一。除了仅仅去中心化依赖和仓库外，促进项目间统一的目录结构也是其重要方面之一。

在这篇简短的文章中，我们将探讨典型 Maven 项目的标准目录布局。

##  2. 目录结构

一个典型的 Maven 项目有一个文件和基于定义约定的目录结构：

```plaintext
└───maven-project
    ├───pom.xml
    ├───README.txt
    ├───NOTICE.txt
    ├───LICENSE.txt
    └───src
        ├───main
        │   ├───java
        │   ├───resources
        │   ├───filters
        │   └───webapp
        ├───test
        │   ├───java
        │   ├───resources
        │   └───filters
        ├───it
        ├───site
        └───assemblyCopy
```

默认目录结构可以通过项目描述符进行覆盖，但这不常见且不建议这样做。

在本文中继续前进，我们将揭示每个标准文件和子目录的更多细节。

##  3. 根目录

这个目录是每个 Maven 项目的根目录。

让我们更仔细地看看通常在根目录下找到的标准文件和子目录：

- maven-project/pom.xml – 定义了 Maven 项目构建生命周期所需的依赖和模块
- maven-project/LICENSE.txt – 项目许可信息
- 项目/README.txt – 项目摘要
- maven-project/NOTICE.txt – 项目中使用的第三方库信息
- maven-project/src/main – 包含成为构件一部分的源代码和资源
- maven-project/src/test – 存放所有测试代码和资源
- maven-project/src/it – 通常保留用于由 Maven Failsafe 插件 执行的集成测试
- maven-project/src/site – 使用Maven Site 插件创建的网站文档
- maven-project/src/assembly – 二进制打包的配置文件

## 4. 《src/main》目录

根据名称指示，src/main 是 Maven 项目的最重要的目录。任何应该作为工件一部分的内容，无论是 jar 还是 war，都应该在这里存在。

 其子目录包括：

- src/main/java – 该工件 Java 源代码
- src/main/resources – 配置文件以及其他，如 i18n文件、按环境配置文件以及 XML 配置
  
- src/main/webapp – 用于 Web 应用程序，包含 JavaScript、CSS、HTML 文件、视图模板和图像等资源
- src/main/filters – 包含在构建阶段将值注入到资源文件夹中配置属性的文件

## 5. 《src/test》目录 

目录 src/test 是应用程序中每个组件测试所在的位置。

请注意，这些目录或文件都不会成为工件的一部分。让我们看看它的子目录：

- src/test/java – 测试用例的 Java 源代码
- src/test/resources – 测试所使用的配置文件和其他文件
- src/test/filters – 包含在测试阶段将值注入到资源文件夹中配置属性的文件

##  6. 结论

在这篇文章中，我们探讨了 Apache Maven 项目的标准目录布局。

多个 Maven 项目结构示例可以在 [GitHub 项目](https://github.com/eugenp/tutorials/tree/master/maven-modules) 中找到。

# Maven 目标和阶段

##  1. 概述

在这个教程中，我们将探讨不同的 Maven 构建生命周期及其阶段。

我们将讨论目标和阶段之间的核心关系。

##  2. Maven 构建生命周期

Maven 构建遵循特定的生命周期来部署和分发目标项目。

有三个内置生命周期：

- 默认：主生命周期，因为它负责项目部署
- clean：清理项目并删除之前构建生成的所有文件
- 网站：创建项目的站点文档

每个生命周期由一系列阶段组成。默认构建生命周期由 23 个阶段组成，因为它是主要的构建生命周期。

另一方面，清洁生命周期包括 3 个阶段，而网站生命周期由 4 个阶段组成。

##  3. Maven 阶段

< strong id = 0 > Maven 阶段表示 Maven 构建生命周期中的一个阶段 。每个阶段都负责特定的任务。

以下是默认构建生命周期中最重要的几个阶段：

- 验证：检查构建所需的所有信息是否可用
- 编译：编译源代码
- 测试编译：编译测试源代码
- 测试：运行单元测试
- 包：将编译后的源代码打包成可分发格式（jar、war 等）
- 集成测试：如有需要，处理并部署该包以运行集成测试
- 安装：将软件包安装到本地仓库
- 部署：将软件包复制到远程仓库

有关每个生命周期阶段的完整列表，请查看[Maven 参考](http://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html#Lifecycle_Reference)。

阶段按特定顺序执行。这意味着如果我们使用命令运行特定阶段：

```bash
mvn <PHASE>Copy
```

它不仅将执行指定的阶段，还将执行所有前面的阶段。

例如，如果我们运行部署阶段，这是默认构建生命周期的最后一个阶段，它将执行部署阶段之前的所有阶段，即整个默认生命周期：

```bash
mvn deployCopy
```

##  4. Maven 目标

每个阶段是一系列目标，每个目标负责一项特定任务。

当我们运行一个阶段时，所有绑定到该阶段的目标都将按顺序执行。

这里有一些与之相关的阶段和默认目标：

- 编译器：编译 – 编译器插件中的编译目标绑定到编译阶段
- 编译器：testCompile 绑定到 test-compile 阶段
- surefire:test 绑定到 test 阶段
- 安装:安装 绑定到 安装 阶段
- jar:jar 和 war:war 与 包 阶段绑定

我们可以使用命令列出绑定到特定阶段的所有目标和它们的插件：

```bash
mvn help:describe -Dcmd=PHASENAMECopy
```

例如，要列出绑定到编译阶段的全部目标，我们可以运行：

```bash
mvn help:describe -Dcmd=compileCopy
```

然后我们会得到示例输出：

```bash
compile' is a phase corresponding to this plugin:
org.apache.maven.plugins:maven-compiler-plugin:3.1:compileCopy
```

如上所述，这意味着来自编译器插件的编译目标绑定到编译阶段。

##  5. Maven 插件

<强烈 id=0>一个 Maven 插件是一组目标；然而，这些目标并不一定都绑定到相同的阶段。

例如，这是一个 Maven Failsafe 插件的简单配置，该插件负责运行集成测试：

```xml
<build>
    <plugins>
        <plugin>
            <artifactId>maven-failsafe-plugin</artifactId>
            <version>${maven.failsafe.version}</version>
            <executions>
                <execution>
                    <goals>
                        <goal>integration-test</goal>
                        <goal>verify</goal>
                    </goals>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>Copy
```

如您所见，Failsafe 插件在此配置了两个主要目标：

- 集成测试：运行集成测试
- 验证：所有集成测试已通过

我们可以使用以下命令来列出特定插件中的所有目标：列出所有目标：

```bash
mvn <PLUGIN>:helpCopy
```

例如，要列出 Failsafe 插件中的所有目标，我们可以运行：

```bash
mvn failsafe:helpCopy
```

并且输出将是：

```bash
This plugin has 3 goals:

failsafe:help
  Display help information on maven-failsafe-plugin.
  Call mvn failsafe:help -Ddetail=true -Dgoal=<goal-name> to display parameter
  details.

failsafe:integration-test
  Run integration tests using Surefire.

failsafe:verify
  Verify integration tests ran using Surefire.Copy
```

要运行特定的目标而不执行其整个阶段（以及前面的阶段），我们可以使用以下命令：

```bash
mvn <PLUGIN>:<GOAL>Copy
```

例如，要从 Failsafe 插件运行集成测试目标，我们需要运行：

```plaintext
mvn failsafe:integration-testCopy
```

## 6. 构建 Maven 项目

为了构建一个 Maven 项目，我们需要通过运行它们的一个阶段来执行生命周期之一：

```bash
mvn deployCopy
```

这将执行整个默认生命周期。或者，我们可以在安装阶段停止：

```bash
mvn installCopy
```

但是通常，我们会在新构建之前先通过运行清理生命周期来清理项目：

```bash
mvn clean installCopy
```

我们也可以只运行插件的一个特定目标：

```bash
mvn compiler:compileCopy
```

请注意，如果我们尝试构建一个没有指定阶段或目标的 Maven 项目，将会出现错误：

```bash
[ERROR] No goals have been specified for this build. You must specify a valid lifecycle phase or a goalCopy
```

##  7. 结论

在这篇文章中，我们讨论了 Maven 构建生命周期，以及 Maven 阶段和目标之间的关系。

# Maven 发布到 Nexus

##  1. 概述

在前一篇文章中，我们使用 Maven 将部署过程设置到了 Nexus。在这篇文章中，我们将配置 Maven 的发布过程——既在项目的*pom*文件中，也在 Jenkins 作业中。

## 2. 仓库在中

为了使 Maven 能够向 Nexus 仓库服务器发布，我们需要通过*分发管理*元素定义仓库信息：

```xml
<distributionManagement>
   <repository>
      <id>nexus-releases</id>
      <url>http://localhost:8081/nexus/content/repositories/releases</url>
   </repository>
</distributionManagement>Copy
```

Nexus 上的托管发布版本库是默认安装的，因此无需显式创建。

## 3. Maven 的 pom中的 SCM

发布流程将与项目的源代码管理交互——这意味着我们首先需要在我们的 *pom.xml* 中定义 *<scm>* 元素：

```xml
<scm>
   <connection>scm:git:https://github.com/user/project.git</connection>
   <url>http://github.com/user/project</url>
   <developerConnection>scm:git:https://github.com/user/project.git</developerConnection>
</scm>Copy
```

或者，使用 git 协议：

```xml
<scm>
   <connection>scm:git:git@github.com:user/project.git</connection>
   <url>scm:git:git@github.com:user/project.git</url>
   <developerConnection>scm:git:git@github.com:user/project.git</developerConnection>
</scm>Copy
```

##  4. 发布插件

The standard Maven plugin used by a Release Process is the *maven-release-plugin* – the configuration for this plugin is minimal: 标准的 Maven 插件，用于发布流程的是 *maven-release-plugin* – 此插件的配置是最小的：

```xml
<plugin>
   <groupId>org.apache.maven.plugins</groupId>
   <artifactId>maven-release-plugin</artifactId>
   <version>2.4.2</version>
   <configuration>
      <tagNameFormat>v@{project.version}</tagNameFormat>
      <autoVersionSubmodules>true</autoVersionSubmodules>
      <releaseProfiles>releases</releaseProfiles>
   </configuration>
</plugin>Copy
```

这里重要的是，*releaseProfiles* 配置实际上会强制 Maven 配置文件 – *releases* 配置文件 – 在发布过程中生效。此外，将 *autoVersionSubmodules* 配置设置为 true 确保每个模块的版本与父版本相同，如果设置为 false，则用户将为项目的每个模块提示版本号。

在这个过程中，使用 *nexus-staging-maven-plugin* 将部署到 *nexus-releases* Nexus 仓库：

```xml
<profiles>
   <profile>
      <id>releases</id>
      <build>
         <plugins>
            <plugin>
               <groupId>org.sonatype.plugins</groupId>
               <artifactId>nexus-staging-maven-plugin</artifactId>
               <version>1.5.1</version>
               <executions>
                  <execution>
                     <id>default-deploy</id>
                     <phase>deploy</phase>
                     <goals>
                        <goal>deploy</goal>
                     </goals>
                  </execution>
               </executions>
               <configuration>
                  <serverId>nexus-releases</serverId>
                  <nexusUrl>http://localhost:8081/nexus/</nexusUrl>
                  <skipStaging>true</skipStaging>
               </configuration>
            </plugin>
         </plugins>
      </build>
   </profile>
</profiles>Copy
```

插件配置为执行发布过程，不使用预发布机制，与[之前](https://www.baeldung.com/maven-deploy-nexus)相同，用于部署过程（*skipStaging=true*）。

同样，与部署过程类似，将软件发布到 Nexus 是一个安全操作——因此我们将再次使用 Nexus 的即开即用部署用户表单。

我们还需要在全局 *nexus-releases* 服务器中配置凭证，在 *settings.xml* 文件中（*%USER_HOME%/.m2/settings.xml*）：

```xml
<servers>
   <server>
      <id>nexus-releases</id>
      <username>deployment</username>
      <password>the_pass_for_the_deployment_user</password>
   </server>
</servers>Copy
```

这是完整的配置

##  5. 发布流程

让我们将发布过程分解成小而专注的步骤。当项目的当前版本是快照版本时，我们进行发布——比如说 *0.1-SNAPSHOT*。

### 5.1. 发布：清洁

清理发布将：

- 删除发布描述符（*release.properties*）
- 删除任何备份的 POM 文件

### 5.2. 发布：准备

下一部分发布流程是 准备发布；这将：

- 执行一些检查 - 应该没有未提交的更改，并且项目不应依赖于任何 SNAPSHOT 依赖项
- 将 pom 文件中项目的版本改为完整的发布号（移除 SNAPSHOT 后缀）- 在我们的例子中 - *0.1*
- 运行项目 测试 套件
- 提交并推送更改
- 创建此非 SNAPSHOT 版本代码中的 标签
- 增加项目的版本在 pom 中 - 在我们的例子中 - *0.2-SNAPSHOT*
- 提交并推送更改

### 5.3. 发布：执行

发布过程的后期是 执行发布；这将：

- 检出 SCM 中的发布标签
- 构建和部署发布代码

此过程的第二步依赖于准备步骤的输出 - *release.properties*。

##  6. 在 Jenkins 上

Jenkins 可以通过两种方式之一执行发布流程 - 它可以使用自己的发布插件，或者简单地通过运行正确的发布步骤的标准 Maven 作业来执行发布。

现有的 Jenkins 插件主要关注发布流程，包括：

- [ 发布插件](https://wiki.jenkins-ci.org/display/JENKINS/Release+Plugin)
- [ M2 发布插件](https://wiki.jenkins-ci.org/display/JENKINS/M2+Release+Plugin)

然而，由于执行发布的 Maven 命令足够简单，我们可以简单地定义一个标准的 Jenkins 作业来执行此操作 - 不需要插件。

因此，对于一个新的 Jenkins 任务（构建 maven2/3 项目）——我们将定义 2 个字符串参数：*releaseVersion=0.1* 和 *developmentVersion=0.2-SNAPSHOT*。

在《构建》配置部分，我们可以简单地配置以下 Maven 命令来运行：

```shell
release:clean release:prepare release:perform 
   -DreleaseVersion=${releaseVersion} -DdevelopmentVersion=${developmentVersion}Copy
```

在运行参数化作业时，Jenkins 将提示用户指定这些参数的值 - 因此每次运行作业时，我们都需要填写正确的值，为 *发布版本* 和 *开发版本*。

此外，值得使用[工作区清理插件](https://wiki.jenkins-ci.org/display/JENKINS/Workspace+Cleanup+Plugin)并检查此构建的*在构建开始前删除工作区*选项。然而，请注意，发布中的*执行步骤必须与*准备*步骤使用相同的命令运行——这是因为后者的*执行*步骤将使用由*准备*创建的*release.properties*文件。这意味着我们不能让 Jenkins 作业同时运行*准备*和另一个运行*执行*。

##  7. 结论

本文展示了如何使用或不用 Jenkins 实现发布 Maven 项目的过程。类似于[部署](https://www.baeldung.com/maven-deploy-nexus)，这个过程使用*nexus-staging-maven-plugin*与 Nexus 交互，并专注于 git 项目。

# Maven 配置指南

##  1. 概述

< strong id = 0 > < a id = 1 > Maven 配置文件可用于创建自定义构建配置 ，例如针对测试粒度级别或特定部署环境。

在这个教程中，我们将学习如何使用 Maven 配置文件。

##  2. 一个基本示例

通常 **当我们运行 \*mvn package,\* 单元测试也会被执行**。但 **如果我们想快速打包工件并运行它** 来查看它是否工作呢？

首先，我们将创建一个设置 *no-tests* 属性为 *maven.test.skip* 并将其设置为 *true:* 的 *no-tests* 配置文件：

```xml
<profile>
    <id>no-tests</id>
    <properties>
        <maven.test.skip>true</maven.test.skip>
    </properties>
</profile>Copy
```

接下来，我们将通过运行*mvn package -Pno-tests*命令来执行配置文件。**现在已创建工件并跳过了测试。**在这种情况下，使用*mvn package -Dmaven.test.skip*命令会更简单。

然而，这仅仅是对 Maven 配置文件的介绍。让我们看看一些更复杂的设置。

##  3. 声明配置文件

在上一节中，我们看到了如何创建一个配置文件。我们可以通过赋予它们唯一的 ID 来配置尽可能多的配置文件。

让我们假设我们想要创建一个仅运行我们的集成测试的配置文件，另一个用于一组突变测试。

我们首先会在我们的文件中为每个指定一个*id*：

```xml
<profiles>
    <profile>
        <id>integration-tests</id>
    </profile>
    <profile>
        <id>mutation-tests</id>
    </profile>
</profiles>Copy
```

在每一个*配置文件*元素中，**我们可以配置许多元素，例如\*依赖项\*、\*插件\*、\*资源\*、\*最终名称\***。

因此，对于上面的例子，我们可以分别为*集成测试*和*突变测试*单独添加插件及其依赖项。

将测试分离到配置文件中可以使默认构建更快，例如，只需关注单元测试即可。

###  3.1. 个人资料范围

现在，我们只是将这些配置文件放入了我们的 *pom.xml* 文件中，该文件仅为我们项目声明它们。

但是，在 Maven 3 中，我们实际上可以将配置文件添加到三个位置中的任何一个：

1. 特定项目的配置文件存放在项目的 *pom.xml* 文件中
2. 用户特定的配置文件存放在用户的*settings.xml*文件中
3. 全局配置文件存入全局 *settings.xml* 文件

请注意，Maven 2 曾经支持第四个位置，但这一功能在 Maven 3 中被移除。

我们尽可能在中配置配置文件。原因是我们想在开发机器和构建机器上同时使用这些配置文件。使用更困难且容易出错，因为我们必须自己将其分发到构建环境中。

##  4. 激活配置文件

在创建一个或多个配置文件后，我们可以开始使用它们，换句话说，*激活*它们。

### 4.1. 查看哪些配置文件处于活动状态

让我们使用*帮助：活动配置文件*目标来查看哪些配置文件在我们的默认构建中处于活动状态

```bash
mvn help:active-profilesCopy
```

实际上，因为我们还没有激活任何东西，所以我们得到：

```bash
The following profiles are active:Copy
```

 嗯，没什么。

我们将很快激活它们。但快速查看已激活内容的一种方法是，在我们的 *pom.xml* 中包含 *maven-help-plugin*，并将 *active-profiles* 目标与 *编译* 阶段关联：

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-help-plugin</artifactId>
            <version>3.2.0</version>
            <executions>
                <execution>
                    <id>show-profiles</id>
                    <phase>compile</phase>
                    <goals>
                        <goal>active-profiles</goal>
                    </goals>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>Copy
```

现在，让我们开始使用它们！我们将探讨几种不同的方法。

###  4.2. 使用 -P

实际上，我们在一开始就已经看到了一种方法，那就是我们可以使用**-P参数来\*激活配置文件\*。**
