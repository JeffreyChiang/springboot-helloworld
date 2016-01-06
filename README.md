使用SpringBoot快速搭建Web服务
======
springboot只需很少的配置就能搭建一个基于spring的应用。本文使用gradle和spring boot来快速搭建一个web服务。

### 1.创建项目文件夹并初始化项目

```
mkdir helloworld
cd helloworld
gradle init
```

### 2.修改build.gradle


```
    buildscript {
        repositories {
            mavenLocal()
            maven {
                setUrl 'http://maven.oschina.net/content/groups/public/'
            }
            maven {
                setUrl 'http://repo1.maven.org/maven2'
            }
            maven {
                setUrl "https://repo.spring.io/libs-release"
            }
            jcenter {
                setUrl 'http://jcenter.bintray.com'
            }
        }
        dependencies {
            classpath("org.springframework.boot:spring-boot-gradle-plugin:1.3.1.RELEASE")
        }
    }

    apply plugin: 'java'
    apply plugin: 'idea'
    apply plugin: 'spring-boot'
    jar {
        baseName = 'helloworld'
        version = '0.1.0'
    }
    repositories {
        mavenLocal()
        maven {
            setUrl 'http://maven.oschina.net/content/groups/public/'
        }
        maven {
            setUrl 'http://repo1.maven.org/maven2'
        }
        maven {
            setUrl "https://repo.spring.io/libs-release"
        }
        jcenter {
            setUrl 'http://jcenter.bintray.com'
        }

    }

    dependencies {
        compile("org.springframework.boot:spring-boot-starter-web:1.3.1.RELEASE")
        testCompile("junit:junit")
    }

```
这里唯一的依赖项是`spring-boot-starter-web`，项目下载下来的依赖包如下图，这些都是`spring-boot-starter-web`默认项。
![依赖包](http://7xpuft.com1.z0.glb.clouddn.com/spring__spring_boot_dependencies.jpg)


### 3.创建项目所需文件


```
mkdir -p src/main/java/helloworld

```

#### 3.1 Greeting.java


```
package helloworld;

public class Greeting {
    private final long id;
    private final String content;

    public Greeting(long id, String content) {
        this.id = id;
        this.content = content;
    }

    public long getId() {
        return id;
    }

    public String getContent() {
        return content;
    }
}

```

#### 3.2 GreetingController.java

```
package helloworld;

import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

import java.util.concurrent.atomic.AtomicLong;

@RestController
public class GreetingController {
    private static final String template = "Hello, %s!";
    private final AtomicLong counter = new AtomicLong();

    @RequestMapping("/greeting")
    public Greeting greeting(@RequestParam(value = "name", required = false, defaultValue = "World") String name) {
        return new Greeting(counter.incrementAndGet(),
                String.format(template, name));
    }
}
```

#### 3.3 Application.java

```
package helloworld;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.EnableAutoConfiguration;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;

@Configuration
@ComponentScan
@EnableAutoConfiguration
public class Application {
    public static void main(String[] args) {
            SpringApplication.run(Application.class, args);
    }
}
```

### 4.编译并启动应用

```
gradle build
java -jar build/libs/helloworld-0.1.0.jar

```
这时一个使用springboot搭建的web服务就已经启动了，可以打开浏览器访问`http://localhost:8080/greeting?name=Jeffrey`就可以了
