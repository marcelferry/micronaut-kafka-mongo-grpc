# Micronaut Tutorial

## Setup

### Requisitos Prévios

#### GraalVM 

```
/usr/libexec/java_home -V
```
Retorno do comando

```
11.0.12 (x86_64) "GraalVM Community" - "GraalVM CE 21.2.0" /Library/Java/JavaVirtualMachines/graalvm-ce-java11-21.2.0/Contents/Home
```

#### Docker & Docker Compose

#### Micronaut CLI

https://micronaut-projects.github.io/micronaut-starter/latest/guide/index.html#installHomebrew

brew install --cask micronaut-projects/tap/micronaut

mn create-app br.com.marcelferry.micronaut.kafka-avro-mongo-grpc --build=maven --lang=java --features kafka,mongo-reactive,lombok,jib,kubernetes,management,openapi

cd kafka-avro-mongo-grpc

### Git Versioning

git remote add origin git@github.com:marcelferry/micronaut-kafka-mongo-grpc.git
git branch -M main
git push -u origin main

### Install Native Image

gu install native-image

### Creating Hello World

```java
package example.micronaut;

import io.micronaut.http.MediaType;
import io.micronaut.http.annotation.Controller;
import io.micronaut.http.annotation.Get;
import io.micronaut.http.annotation.Produces;

@Controller("/hello") 
public class HelloController {
    @Get 
    @Produces(MediaType.TEXT_PLAIN) 
    public String index() {
        return "Hello World"; 
    }
}
```



```java
package example.micronaut;

import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.junit.jupiter.api.Assertions.assertNotNull;

import io.micronaut.http.HttpRequest;
import io.micronaut.http.client.HttpClient;
import io.micronaut.http.client.annotation.Client;
import io.micronaut.test.extensions.junit5.annotation.MicronautTest;
import org.junit.jupiter.api.Test;

import javax.inject.Inject;

@MicronautTest 
public class HelloControllerTest {

    @Inject
    @Client("/")  
    HttpClient client;

    @Test
    public void testHello() {
        HttpRequest<String> request = HttpRequest.GET("/hello");  
        String body = client.toBlocking().retrieve(request);

        assertNotNull(body);
        assertEquals("Hello World", body);
    }
}
```

```
mvn package -Dpackaging=native-image
```



```
./target/kafka-avro-mongo-grpc
```



```
curl http://localhost:8080/hello
```

