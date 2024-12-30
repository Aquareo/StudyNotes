# Maven `pom.xml` 文件详解教程

`pom.xml` 是 **Maven** 构建工具的核心配置文件，它定义了项目的所有依赖、插件、构建生命周期以及项目的其他元数据。在使用 Maven 构建 Java 项目时，`pom.xml` 是必不可少的。以下是 `pom.xml` 文件的详细教程，涵盖常见的元素和配置项。

## 1. 基本结构

一个最基本的 `pom.xml` 文件结构大致如下：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">

    <!-- 项目信息 -->
    <modelVersion>4.0.0</modelVersion>

    <!-- 项目的基础信息 -->
    <groupId>com.example</groupId>       <!-- 项目组ID -->
    <artifactId>my-project</artifactId>  <!-- 项目ID -->
    <version>1.0-SNAPSHOT</version>      <!-- 项目版本 -->
    <packaging>jar</packaging>           <!-- 打包类型，默认为 jar -->

    <!-- 描述和名称 -->
    <name>My Project</name>              <!-- 项目名称 -->
    <description>My Project Description</description> <!-- 项目描述 -->
    <url>http://www.example.com</url>    <!-- 项目网址 -->

    <!-- 管理项目的依赖 -->
    <dependencies>
        <!-- 依赖项放在这里 -->
    </dependencies>

    <!-- 构建配置 -->
    <build>
        <plugins>
            <!-- 插件配置放在这里 -->
        </plugins>
    </build>

</project>
```

## 2 核心元素

### 2.1 modelVersion

定义 POM 的模型版本，几乎所有的 `pom.xml` 都使用 4.0.0 版本。

```xml

<modelVersion>4.0.0</modelVersion>

### 2.2 `groupId`, `artifactId`, `version`

这三个元素定义了项目的唯一标识。

- groupId：通常是组织或公司域名的反向形式，用于唯一标识项目所在的组织。
- artifactId：项目的唯一标识符，通常是项目的名称。
- version：项目的版本号，通常使用 1.0.0 或 1.0-SNAPSHOT（SNAPSHOT 表示开发中的版本）。

```xml
<groupId>com.example</groupId>
<artifactId>my-project</artifactId>
<version>1.0-SNAPSHOT</version>
```

### 2.3 packaging

定义构建项目时生成的文件类型，常见的类型有：

- jar：生成 JAR 文件。
- war：生成 WAR 文件（用于 Web 项目）。
- pom：如果当前模块是父 POM，使用此类型。
```xml

<packaging>jar</packaging>
```

### 2.4 name, description, url
这几个元素用于描述项目的基本信息。
```xml

<name>My Project</name>
<description>A description of my project</description>
<url>http://www.example.com</url>
```


## 3.父 POM (parent)

如果你的项目继承自一个父项目（例如 Spring Boot），你可以在 `pom.xml` 中使用 `<parent>` 元素来声明父 POM。这样可以继承父项目的配置和依赖。

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.5.4</version>  <!-- 使用的 Spring Boot 版本 -->
    <relativePath/>  <!-- 相对路径，通常为空，表示从中央仓库获取 -->
</parent>


```
## 4.依赖 (dependencies)
dependencies 元素定义了项目所需的库和框架。每个依赖项由 `<dependency>` 元素定义。

```xml
<dependencies>
    <!-- 示例：Spring Boot Starter Web 依赖 -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
        <version>2.5.4</version> <!-- 版本号可以根据需要调整 -->
    </dependency>
</dependencies>

```

依赖管理：
- `groupId`：依赖所在的组织或公司。
- `artifactId`：依赖的唯一标识符。
- `version`：依赖的版本号。
你可以在 `parent` POM 中管理版本，这样可以避免在每个模块中重复写版本号。

## 5.插件 (build/plugins)

`plugins` 元素定义了构建过程中使用的插件，如 `maven-compiler-plugin` 或 `maven-spring-boot-plugin`。

```xml
<build>
    <plugins>
        <!-- 编译插件 -->
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>3.8.1</version>
            <configuration>
                <source>1.8</source>
                <target>1.8</target>  <!-- 配置 JDK 版本 -->
            </configuration>
        </plugin>
    </plugins>
</build>


## 6. 属性 (properties)

在 POM 中，你可以使用 `<properties>` 元素定义一些全局属性（例如 Java 编译版本），这些属性可以在整个 POM 文件中使用。
```xml
<properties>
    <java.version>1.8</java.version> <!-- 定义 Java 版本 -->
</properties>

```

## 7. 构建生命周期和阶段（build）

在 build 元素中，你可以定义构建的生命周期、插件和目标。常用的插件有：

- maven-compiler-plugin：用于 Java 编译。
- maven-jar-plugin：用于创建 JAR 包。
- spring-boot-maven-plugin：用于打包 Spring Boot 应用。

```xml
<build>
    <plugins>
        <!-- Spring Boot 打包插件 -->
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
            <version>2.5.4</version>
        </plugin>
    </plugins>
</build>

```



## 8. 构建配置示例

下面是一个完整的 `pom.xml` 示例，包括常见的配置：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">

    <modelVersion>4.0.0</modelVersion>

    <groupId>com.example</groupId>
    <artifactId>my-springboot-app</artifactId>
    <version>1.0.0</version>
    <packaging>jar</packaging>

    <name>My Spring Boot Application</name>
    <description>A simple Spring Boot application</description>
    <url>http://www.example.com</url>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.5.4</version>
        <relativePath/>
    </parent>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.8.1</version>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

</project>
```


## 9.总结

直接影响程序运行的部分：
- 依赖（dependencies）
- 构建插件（build plugins）
- 构建过程配置（如编译器配置）
- 插件执行配置（如测试插件）
- 资源文件目录配置

仅为标记或说明的部分：
- 项目信息（如 `groupId`、`artifactId`、`version`）
- 属性配置（如 `java.version`）
- 注释
- 依赖管理（dependencyManagement）




# 一个项目

```plaintext
├─.idea
├─bydcbsc-business-api
├─bydcbsc-business-common
│  ├─src
│  │  └─main
│  │      └─java
│  │          └─com
│  │              └─bydauto
│  │                  ├─application
│  │                  │  ├─dto
│  │                  │  ├─enums
│  │                  │  └─vo
│  │                  ├─enums
│  │                  └─model
│  └─target
│      ├─classes
│      │  └─com
│      │      └─bydauto
│      │          ├─application
│      │          │  ├─dto
│      │          │  ├─enums
│      │          │  └─vo
│      │          ├─enums
│      │          └─model
│      └─generated-sources
│          └─annotations
├─bydcbsc-business-process
│  ├─src
│  │  └─main
│  │      ├─java
│  │      │  └─com
│  │      │      └─bydauto
│  │      │          ├─authorize
│  │      │          ├─config
│  │      │          ├─dao
│  │      │          ├─eunm
│  │      │          ├─exception
│  │      │          ├─listener
│  │      │          ├─model
│  │      │          │  ├─dto
│  │      │          │  ├─entity
│  │      │          │  ├─param
│  │      │          │  └─vo
│  │      │          │      └─mobile
│  │      │          ├─service
│  │      │          │  ├─business
│  │      │          │  ├─client
│  │      │          │  ├─cmm
│  │      │          │  ├─dictionary
│  │      │          │  ├─email
│  │      │          │  ├─files
│  │      │          │  ├─flowable
│  │      │          │  ├─form
│  │      │          │  ├─message
│  │      │          │  ├─report
│  │      │          │  ├─seq
│  │      │          │  ├─spareParts
│  │      │          │  └─undertaker
│  │      │          ├─utils
│  │      │          └─web
│  │      └─resources
│  │          ├─processes
│  │          ├─static
│  │          └─templates
│  └─target
│      ├─classes
│      │  ├─com
│      │  │  └─bydauto
│      │  │      ├─authorize
│      │  │      ├─config
│      │  │      ├─dao
│      │  │      ├─eunm
│      │  │      ├─exception
│      │  │      ├─listener
│      │  │      ├─model
│      │  │      │  ├─dto
│      │  │      │  ├─entity
│      │  │      │  ├─param
│      │  │      │  └─vo
│      │  │      │      └─mobile
│      │  │      ├─service
│      │  │      │  ├─business
│      │  │      │  ├─client
│      │  │      │  ├─cmm
│      │  │      │  ├─dictionary
│      │  │      │  ├─email
│      │  │      │  ├─files
│      │  │      │  ├─flowable
│      │  │      │  ├─form
│      │  │      │  ├─message
│      │  │      │  ├─report
│      │  │      │  ├─seq
│      │  │      │  ├─spareParts
│      │  │      │  └─undertaker
│      │  │      ├─utils
│      │  │      └─web
│      │  ├─processes
│      │  ├─static
│      │  └─templates
│      └─generated-sources
│          └─annotations
└─logs  
```
