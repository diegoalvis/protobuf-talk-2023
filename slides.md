---
theme: default
layout: cover
title: 'Diego Alvis'
titleTemplate: '%s - #droidconBLN23'
canvasWidth: 1140
# transition: fade-out
---

# Protocol Buffers on Android using gRPC

---

# Intro
<br><br><br>

<v-clicks>

Diego Alvis

Engineering Manager @ Delivery Hero

Colombian

Spicy food enthusiastic

Bus driver before becoming a developer

</v-clicks>

---

# Agenda
- What is Protobuf?
- What is gRPC?
- Where do we use Protobuf?

---

# What are Protocol Buffers?
<br><br><br><br>

- Protocol Buffers (protobuf) is a language-agnostic data serialization format developed by Google.
<br>
- It allows you to define structured data schemas in a simple language.
<br>
- Protobuf messages are more compact, efficient, and extensible compared to traditional XML or JSON formats.

---
layout: two-cols
image: https://grpc.io/img/landing-2.svg
---

# Where can we use it?
<br><br><br><br>

- Protocol Buffers (protobuf) is a language-agnostic data serialization format developed by Google.
<br>
- It allows you to define structured data schemas in a simple language.
<br>
- Protobuf messages are more compact, efficient, and extensible compared to traditional XML or JSON formats.

::right::
<br><br><br><br>

![grpc](https://grpc.io/img/landing-2.svg)
<br>

<br><br><br>

       ##### Image source: https://grpc.io/img/landing-2.svg

<!-- 
Server to server comunication is other common case
-->
---

## Message Defintion

<div v-click>
<div class="grid grid-cols-2 gap-20">
<div>

JSON
```java
  "person": {
    "id": int,
    "name": string,
    "email": string
  }
```
</div>
<div>

XML
```xml
  <person>
    <id>int</id>
    <name>string</name>
    <email>string</email>
  </person>
```
</div>
</div>
</div>

<div v-click>

Protobuf
```protobuf {}
  message Person {
     int32 id = 1;
     string name = 2;
     string email = 3;
   }
```
</div>

<br>
<div v-click>
Protobuf uses up to 2 bytes as identifier. 

* 1 to 15  -->  1 byte  (0x0F)
* 16 to 2047  -->  2 bytes (0x07FF)
</div>

---

<div class="grid grid-cols-2 gap-20">
<div>

              Plain text (XML)
<br><br>
<img src="https://www.thoughtworks.com/content/dam/thoughtworks/images/photography/inline-image/insights/blog/microservices/blg_inline_gRPC_03.png" width="400" height="500" />
</div>
<div>

                Protobuf
<br><br>
<img src="https://www.thoughtworks.com/content/dam/thoughtworks/images/photography/inline-image/insights/blog/microservices/blg_inline_gRPC_04.png" width="400" height="500" />
</div>
</div>

<br><br><br><br>
<br><br><br><br>
Source: https://www.thoughtworks.com/en-es/insights/blog/microservices/scaling-microservices-gRPC-part-one

---

# What is gRPC?
<br><br><br>

- gRPC is a framework for Remote Procedure Calls developed by Google

- gRPC by default uses Protobuf

- Other examples of Remote Procedure Calls are GraphQL and Rest

---

# Proto File 
Booking system

```proto {monaco}
syntax = "proto3";
package com.example.grpc;


message Passenger {
  int32 id = 1;
  string name = 2;
  string email = 3;
}

message MyRequest { }

service MyService {
  rpc GetPassenger (MyRequest) returns (Passenger) {}
}
```

---

# Proto File 

```protobuf
syntax = "proto3";
package com.example.grpc;

message Passenger {
  int32 id = 1;
  string passport = 1;
  repeated string addresses = 3;
}

message Request { }

message GetPassengersRequest { 
  string countryCode = 1;  
}

message GetPassengersResponse {
  repeated Passenger passengers;
}

service MyService {
  rpc GetPassenger (Request) returns (Passenger);
  rpc GetPassengers (GetPassengersRequest) returns (GetPassengersResponse);
}
```

---

## Schema

![grpc](https://www.mulesoft.com/sites/default/files/cmm_files/how-to-auto-generate-grpc-code-using-protoc-figure-proto.png)
<br>

Both sides use the same schema to auto generate code
---

# Benefits of gRPC and Protobuf?
<br><br>

- Efficient serialization: Smaller message size and faster* parsing compared to JSON or XML
- Language-agnostic: Protobuf supports multiple programming languages
- Versioning and backward compatibility: Easy to evolve and modify data schemas without breaking existing clients
- Auto-generated code: Protobuf schemas can be used to generate code for serialization and deserialization
<br><br>
<div v-click>

| C# / .NET   | Dart     | Go       |
| ----------- | -------- | -------- |
| C++         | Java     | Kotlin   |
| Node        | Objective-C | PHP    |
| Python      | Ruby     |          |

<br><br>
Source: https://grpc.io/docs/languages/
</div>

---
layout: cover
background: https://thumbs.gfycat.com/EvergreenCarefulIvorybackedwoodswallow-size_restricted.gif
---
---

## Integrating Protobuf and gRPC on Android

![image](https://source.android.com/static/docs/setup/images/Android_symbol_green_RGB.png)

---
layout: cover
---

# Summer = Ice Cream 🍦
---

# Integrating Protobuf and gRPC on Android
Ice Cream application

### 1. Define messages: Create `ice_cream.proto` file

```protobuf
syntax = "proto3";
package com.example.grpc;

message Cone {
  int32 id = 1;
  string image_url = 2;
  bool available = 3;
}

message Flavor {
  int32 id = 1;
  string name = 1;
  string image_url = 2;
  optional string description = 3;
  double price = 4;
  bool available = 6;
}
...
...
```   
---

# Integrating Protocbuf and gRPC on Android
Ice Cream application

### 2. Define service
```protobuf
...
...

message Request { }

message ConesReply {
  repeated Cone cone = 1;
}

message FlavorsReply {
  repeated Flavor flavor = 1;
}

service IceCream {
  rpc GetCones(Request) returns (ConesReply);
  rpc GetFlavors(Request) returns (FlavorsReply);
}
```   

---

# Integrating Protocol Buffers and gRPC on Android
Ice Cream application

### 3. Generate code from protobuf schema:

Use the Protocol Buffers compiler (`protoc`) with the gRPC plugin to generate Java code for message and service.

<br><br>
<div v-click>

### Adding gRPC Dependencies to Android Project

Include the gRPC dependencies in your project:
   - `implementation "io.grpc:grpc-okhttp:$grpc_version"`
   - `implementation "io.grpc:grpc-protobuf-lite:$grpc_version`
   - `implementation "io.grpc:grpc-stub:$grpc_version"`

</div>
---

### 4. Setup protobuf compiler plugin in Gradle for Kotlin
<br>

```gradle
protobuf {
    protoc {
        artifact = "com.google.protobuf:protoc:${rootProject.ext["protobufVersion"]}"
    }
    plugins {
        id("grpc") {
            artifact = "io.grpc:protoc-gen-grpc-java:${rootProject.ext["grpcVersion"]}"
        }
        id("grpckt") {
            artifact = "io.grpc:protoc-gen-grpc-kotlin:${rootProject.ext["grpcKotlinVersion"]}:jdk8@jar"
        }
    }
    generateProtoTasks {
        all().forEach {
            it.plugins {
                id("grpc")
                id("grpckt")
            }
            it.builtins {
                id("kotlin")
            }
        }
    }
}
```
---

### 5. Run gradle and get auto-generated code
![image](/screenshot_1.png)

---

## 6. Configure gRPC connection
<br>
<div v-click>

## Channel
<br>

- A gRPC channel provides a connection to a gRPC server on a specified host and port
- A channel should be reused when making gRPC calls
- Clients created from the channel can make multiple simultaneous calls

```java
  val channel: ManagedChannel = ManagedChannelBuilder.forAddress("localhost", 50051).build()
```

</div>
<!--
Opening a socket
Establishing TCP connection
Negotiating TLS (Transport Layer Security)
Starting HTTP/2 connection
Making the gRPC call
-->
<br><br>
<div v-click>

## Client Stub
<br>

- Entry point for initiating RPC calls from client sid
- Auto-generated by `protoc` compiler 
- Coroutine based

```java {2}
  val channel: ManagedChannel = ManagedChannelBuilder.forAddress("localhost", 50051).build()
  val coroutineStub = IceCreamGrpcKt.IceCreamCoroutineStub(channel)
```


</div>
---

## 7. Implementing functions
<br>
```kotlin
  val channel: ManagedChannel = ManagedChannelBuilder.forAddress("localhost", 50051).build()
  val coroutineStub = IceCreamGrpcKt.IceCreamCoroutineStub(channel)
```
<div v-click>
```kotlin
  suspend fun getCones(userId: String): List<Cone> {
    val request = request { }
    return coroutineStub.getCones(request).coneList
  }
```
</div>
<div v-click>
```kotlin
  suspend fun getFlavors(userId: String): List<Flavor> {
    val request = request { }
    return coroutineStub.getFlavors(request).flavorList
  }
```
</div>
<div v-click>
```kotlin
  fun close() {
      channel.shutdownNow()
  }
```
</div>

---
layout: cover
---

## Run application

---
layout: two-cols
---

<template v-slot:default>
<img src="https://user-images.githubusercontent.com/6097526/250302965-9154b568-a996-4be8-bb07-7e0396ab32f5.gif " width="270" />
</template>

<template v-slot:right>

```kotlin
class IceCreamRpcService : Closeable {

    private val coroutineStub = IceCreamGrpcKt.IceCreamCoroutineStub(channel)
    
    private val channel = ManagedChannelBuilder
              .forAddress("localhost", 50051)
              .executor(Dispatchers.IO.asExecutor())
              .build()
    
    
    suspend fun getCones(userId: String): List<Cone> {
        val request = request { this.userId = userId }
        return coroutineStub.getCones(request).coneList
    }

    suspend fun getFlavors(userId: String): List<Flavor> {
        val request = request { this.userId = userId }
        return coroutineStub.getFlavors(request).flavorList
    }

    override fun close() {
        channel.shutdownNow()
    }

}
```
</template>

---
layout: two-cols
---

<template v-slot:default>
<div v-click>

### High Specs Device
Response time in ms
<img src="/chart_1.png" width="470" />
</div>
<div v-click>
CPU, Memory and Battery
<img src="/chart_5.png" width="470" />
</div>
</template>

<template v-slot:right>
<div v-click>

### Low Specs Device
Response time in ms
<img src="/chart_2.png" width="470" />
</div>
<div v-click>
CPU, Memory and Battery
<img src="/chart_6.png" width="470" />
</div>
</template>

---

# Trend comparison
Google search terms in the past 5 years

<br><br>
<img src="/chart_4.png" width="670" />

https://trends.google.com/trends/explore?cat=1227&date=today%205-y&q=GraphQL,REST,gRPC&hl=en
---

# Summary
<br><br>

<v-clicks>

## Benefits
</v-clicks>
<br>


<v-clicks>

- Code generation simplifies the integration
- Efficient parsing and less prone to error
- Strongly typed
- Enables high-performance networking

</v-clicks>

<br>
<v-clicks>

## Drawbacks
</v-clicks>
<br>

<v-clicks>

- Limited community support compared to REST and GraphQL
- Requires Protocol Buffers knowledge
- Steeper learning curve
</v-clicks>

---

# Learnings

<br><br><br><br><br>
<v-clicks>

- gRPC will make your app faster and less-prone to error

- Setup might be time consuming. Allocate enough time and capacity

- There is no need to migrate from Rest or GraphQL (JSON). Use gRPC for new features

- Interceptors might not be trivial. Don't leave them for the end

- Hard to debug. Protobuf is designed to be understood by machines not humans

- Helps to leverage knowledge about server side as Mobile Engineer

</v-clicks>

---
layout: two-cols
---
<template v-slot:default>

# Thank You!

</template>

<template v-slot:right>
<br><br><br><br>
<br><br><br><br>
<br><br><br><br>
<br><br><br><br>

- <grommet-icons-mail />  diegoalvispal@gmail.com
- <grommet-icons-github />  [diegoalvis](https://github.com/diegoalvis)
- <grommet-icons-linkedin />  [Diego Alvis](https://www.linkedin.com/in/diego-alvis-7823a5130/)
</template>

