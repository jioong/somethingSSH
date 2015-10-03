# Maven 快速入门 #
  
[Maven 快速入门](http://maven.apache.org/guides/getting-started/maven-in-five-minutes.html "快速入门")  
  
## 先决条件 ##
  
你需要先懂得怎么在电脑上安装软件:)(如此有道理，尽无言以对)  
  
## 安装 ##
  
*Maven* 是一个基于 *Java* 的工具，因此必须要先安装 *Java*。  
[安装 *Maven*]("http://maven.apache.org/install.html" 安装) 过程简单，下载*Maven* 压缩文件并解压，然后将 *mvn* 命令的 *bin* 文件夹路径添加到 *PATH* 环境变量。  
检查 *Maven* 是否正确安装,在命令行输入:  
  
```xml
mvn --version
```
查看其输出，若输出 *Maven* 版本信息， *Java* 信息等则表示成功安装。
  
## 构建项目 ##
  
在想要的地方创建一个文件夹，并在该文件夹中启动命令行。执行如下命令：  

```xml
mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=my-app -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
```
第一次运行会需要一会时间，因为 *Maven* 会下载插件和其他文件的最新 *artifact* 到本地仓库。上述命令会新建一个与指定的 *artifactId* 相同的文件夹。  
  
### POM ###
  
在 *Maven* 项目中 *pom.xml* 为核心。该文件包含了构建项目所需的大部分信息。*POM* 是巨大的且令人生畏，但是没有必要了解所有的复杂关系，只需要高效的利用它。*pom.xml* 文件结构如下：  

```XML
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
 
  <groupId>com.mycompany.app</groupId>
  <artifactId>my-app</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>jar</packaging>
 
  <name>Maven Quick Start Archetype</name>
  <url>http://maven.apache.org</url>
  
  //指定依赖的 JAR 包
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.8.2</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
</project>
```

## Maven 工具 ##
  
### Maven 阶段 ###
  
**default 声明周期：**  
* *validate*: 验证项目是正确的切所有必须信息是可用的。  
* *compile*： 编译项目的源代码。  
* *test*： 用单元测试框架测试编译的源代码。这些测试不应该要求代码被打包或部署。  
* *package*: 将编译的代码打包成可发布格式，如 *JAR*。  
* *integration-test*: 如果需要则将其发布到一个可运行集成测试的环境。  
* *verify*: 运行检查以验证该包是有效的，符合质量标准。  
* *install*: 将包安装到本地仓库，以便本地其他项目可以使用。  
* *deploy*: 发布，可供其他开发者或项目使用。  

还有两个别的声明周期：  
1. *clean*：清理之前构建时创建的 *artifact*。  
2. *site*:为项目生成站点文档。  

**阶段被映射到基本目标。**