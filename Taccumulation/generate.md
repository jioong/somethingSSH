## 命令行创建WEB项目 ##

**使用命令:**  
  
		`mvn archetype:create -DgroupId=com.github.jioong -DartifactId=somethingSSH -DarchetypeArtifactId=maven-archetype-webapp`  
  
创建WEB项目报错:  

> [ERROR] Failed to execute goal org.apache.maven.plugins:maven-archetype-plugin:2
> .4:create (default-cli) on project standalone-pom: Unable to parse configuration
> of mojo org.apache.maven.plugins:maven-archetype-plugin:2.4:create for paramete
> r #: Cannot create instance of interface org.apache.maven.artifact.repository.Ar
> tifactRepository: org.apache.maven.artifact.repository.ArtifactRepository.<init>
> () -> [Help 1]

**命令改为**  
  
		`mvn archetype:generate -DgroupId=com.github.jioong -DartifactId=somethingSSH -DarchetypeArtifactId=maven-archetype-webapp`  

成功构建WEB项目。  

--------  
  
**生成Eclipse项目**  
  
		`mvn eclipse:eclipse`  
