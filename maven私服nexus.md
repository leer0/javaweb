#### docker compose安装nexus

```
docker pull sonatype/nexus3
```

在/usr/local/docker/nexus目录下创建docker-compose.yml文件，添加如下内容

```yml
version: '3.1'
services:
  nexus:
    restart: always
    image: sonatype/nexus3
    container_name: nexus
    ports:
      - 8081:8081
    volumes:
      - /usr/local/docker/nexus/data:/nexus-data
```

然后执行如下命令

```
docker-compose up -d
```

若nexus启动失败，可能是/data文件夹的权限问题，修改器权限即可

```
chmod 777 data
```

#### 配置maven私服

打开maven安装目录下的conf\setting.xml文件，在servers节点中加入如下内容

```xml
<server>
	<id>nexus-releases</id>
    <username>admin</username>
    <password>admin123</password>
</server>
<server>
	<id>nexus-snapshots</id>
    <username>admin</username>
    <password>admin123</password>
</server>
```

在项目的pom文件中加入如下内容

```xml
<distributionManagement>
	<repository>
    	<id>nexus-releases</id>
        <name>Nexus Release Repository</name>
        <url>http://192.168.48.133:8081/repository/maven-releases</url>
    </repository>
    <snapshotRepository>
    	<id>nexus-snapshots</id>
        <name>Nexus Snapshot Repository</name>
        <url>http://192.168.48.133:8081/repository/maven-releases</url>
    </snapshotRepository>
</distributionManagement>
```

注意：

- 这里的id必须对应上面setting.xml中servers配置的id名称

**部署到仓库**

```maven
mvn deploy
```

**上传第三方JAR包**

Nexus3.0不支持页面上传，可使用maven命令：

```
mvn deploy:deploy-file -DgroupId=com.aliyun.oss -DartifactId=aliyun-sdk-oss -Dversion=2.2.3 -Dpackaging=jar -Dfile=D:\aliyun-sdk-oss-2.2.3.jar -Durl=http://192.168.48.133:8081/repository/maven-releases/ -DrepositoryId=nexus-releases
```

注意事项：

- 建议在上传第三方JAR包时，创建单独的第三方JAR包管理仓库，便于管理与维护

- 这里的-DrepositoryId必须对应上面setting.xml中servers配置的id名称

  

**配置代理仓库**

在项目的pom.xml文件中添加如下内容

```xml
<repositories>
	<repository>
    	<id>nexus</id>
        <name>Nexus Repository</name>
        <url>http://192.168.48.133:8081/repository/maven-public</url>
        <snapshots>
        	<enabled>true</enabled>
        </snapshots>
        <release>
        	<enabled>true</enabled>
        </release>
    </repository>
</repositories>
<pluginRepositories>
	<pluginRepository>
    	<id>nexus</id>
        <name>Nexus Plugin Repository</name>
        <url>http://192.168.48.133:8081/repository/maven-public</url>
        <snapshots>
        	<enabled>true</enabled>
        </snapshots>
        <release>
        	<enabled>true</enabled>
        </release>
    </pluginRepository>
</pluginRepositories>
```

