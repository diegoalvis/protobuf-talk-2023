---
theme: default
layout: cover
title: 'Diego Alvis'
titleTemplate: '%s - #droidconBLN23'
canvasWidth: 1140
transition: fade-out
---

# Protocol Buffers on Android using gRPC

---

## Introductory Session
<br><br>

<v-clicks>

### What to expect?

Overview. What is Protobuf and gRPC?

Why and When to use them?

</v-clicks>

<br><br>

<div v-click>

### Don't expect

<v-clicks>

Become an expert after 40 min

gRPC solves all your problems

</v-clicks>

</div>
---

# About me
<br><br>
<br><br>

<v-clicks>

Diego Alvis

Mobile Engineer at Zing (HSBC)

Moved recently to Hong Kong (August). Lived in Berlin for some years. Worked at Delivery Hero.

Running. Muay Thai. Spicy food. Mario Kart...

<br>

## Let's go!

</v-clicks>

---

# Agenda
<br><br>


- What is Protobuf?
- Where can we use it?
- What is gRPC?
- Useful Tips
- Benefits of Protobuf and gRPC
- Implementtation on Android
- Benchmarks & Learnings

---

## We are here

<div v-click>

<img src="/res/images/diagram.jpeg" width="650" height="100" />

</div>

<div v-click>
<br>

### Why is this important?
<br>

- Mobile engineers are also Software engineers
- You can't build an amazing product if you API layer is not amazing too
- Leverage knowledge about the software in general

</div>


---

# What is Protobuf?
<br><br><br><br>

<v-clicks>

- Protocol Buffers (protobuf) is a language-agnostic data serialization format developed by Google

- It allows you to define structured data schemas in a simple language

- Protobuf messages are more compact and efficient compared to traditional JSON or XML formats

</v-clicks>

<!-- 
Server to server communication is other common case
-->
---

# Message Definition

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
Protobuf uses up to 2 bytes for the identifier.

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

<br>

<v-clicks>

Taking a Average Http call Response of 3KB (JSON)  -> 1.3KB (Protobuf)

3000 - 1350 =  ~1.6 KB saved

100 request per day

1 month  -->  1600 * 100 * 30 = 4.800.000 bytes  = 4.8 MB

5K users -> 24 GB data saved

### Up to 7 times faster

</v-clicks>

<br>
Source: https://www.thoughtworks.com/en-es/insights/blog/microservices/scaling-microservices-gRPC-part-one

---

# What is gRPC?
<br>

<v-clicks>


- gRPC is a framework for Remote Procedure Calls developed by Google

- gRPC by default uses Protobuf

- Other examples of Remote Procedure Calls are GraphQL and Rest

</v-clicks>

<br>
<div v-click>

![image](https://grpc.io/img/landing-2.svg)
</div>
---

# Unary and Stream
<br>
<div v-click>

![image](https://i0.wp.com/miro.medium.com/max/861/1*rdflKABGrhOfFdhyzeBUQw.png?w=1230&ssl=1)
</div>
---

# Proto File 
Booking system

<div v-click>

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
  rpc GetPassenger (MyRequest) returns (Passenger);
}
```
</div>

<!-- 

# Proto File 
Booking system

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
-->

---

# Schema
<br>

![grpc](https://www.mulesoft.com/sites/default/files/cmm_files/how-to-auto-generate-grpc-code-using-protoc-figure-proto.png)
<br>

Both sides use the same schema to auto generate code

---

# Useful Tips
<br><br><br><br>

<v-clicks>


Generate files separately using 
```proto 
  option java_multiple_files = true;
  option java_outer_classname = "TravelProto"; // Specify class name
```

<br>
If you have a multimodule project is better to create  a module to store all the proto files. Using Setup in gradle
<br><br>
Use optional to prevent breaking changes and better backwards compatibility support
<br><br>
Be careful with repeated. Large arrays don't perform well

<br> <br>
<br> <br>
Docs: https://protobuf.dev/programming-guides/encoding/


</v-clicks>

---

# Benefits of gRPC and Protobuf
<br><br>

<div v-click>


- Efficient serialization: Smaller message size and faster parsing compared to JSON or XML
- Language-agnostic: gRPC supports multiple programming languages
- Versioning and backward compatibility: Easy to evolve and modify data schemas without breaking existing clients
- Auto-generated code: Protobuf schemas can be used to generate code for serialization and deserialization
<br><br>

</div>
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
background: https://i.giphy.com/media/v1.Y2lkPTc5MGI3NjExbzkyeGluNnkydmMyOTRqNG96d3dyN3JlZGt3aWoxbmhncmdmMncxNiZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/SWdNAbwRVQbODxsi4g/giphy.gif
---
---

# Integrating Protobuf and gRPC on Android
<br>

![image](https://source.android.com/static/docs/setup/images/Android_symbol_green_RGB.png)

---
layout: cover
---

# Travel App 🛫
---

# Integrating Protobuf and gRPC on Android

### 1. Schema. Define messages: Create `travel.proto` file
<br>
<div v-click>

```protobuf {lines:true}
syntax = "proto3";
package com.example.grpc;

message Destination {
  string id = 1;
  string title = 2;
  string image_url = 4;
  ...
  repeated ThingToDo things_to_do = 7;
  repeated FoodAndDrink food_and_drinks = 8;
}

message ThingToDo {
  ...
  int32 reviews_count = 5;
  float score = 6;
}

message FoodAndDrink {
  ...
  string name = 2;
  string image_url = 3;
}
```

</div>
---

# Integrating Protobuf and gRPC on Android
Travel App


### 2. Schema. Define service
<br>
<div v-click>

```protobuf
service TravelService {
  rpc GetDestinations (GetDestinationsRequest) returns (GetDestinationsResponse);
}

message GetDestinationsRequest {
  // TODO
}

message GetDestinationsResponse {
  repeated Destination destinations = 1;
}
```   
</div>

---

# Integrating Protobuf and gRPC on Android
Travel App

### 3. Adding gRPC Dependencies to Android Project
<div v-click>

Include the gRPC dependencies in your project:
   - `implementation "io.grpc:grpc-okhttp:$grpc_version"`
   - `implementation "io.grpc:grpc-protobuf-lite:$grpc_version`
   - `implementation "io.grpc:grpc-stub:$grpc_version"`

</div>
<br><br>
<div v-click>
Use the Protocol Buffers compiler (`protoc`) with the gRPC plugin to generate Java/Kotlin code for messages and service.
</div>

---

# Integrating Protobuf and gRPC on Android
Travel App

### 4. Setup protobuf compiler plugin in Gradle
<div v-click>

```ts
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

</div>

---

# Integrating Protobuf and gRPC on Android
Travel App

### 5. Gradle task to build project and get auto-generated code
<div v-click>

![image](/screenshot_1.png)
</div>

---

# Integrating Protobuf and gRPC on Android
Travel App

### 6. Configure gRPC connection
<br>
<div v-click>

## Channel

- A gRPC channel provides a connection to a gRPC server on a specified host and port
- A channel should be reused when making gRPC calls
- Clients created from the channel can make multiple simultaneous calls

```java
  val channel: ManagedChannel = ManagedChannelBuilder.forAddress("localhost", 50051).build()
```

</div>
<br>
<div v-click>

## Client Stub

- Entry point for initiating RPC calls from client side
- Auto-generated by `protoc` compiler 
- Coroutine based

```java {2}
  val channel: ManagedChannel = ManagedChannelBuilder.forAddress("localhost", 50051).build()
  val coroutineStub = TravelServiceGrpcKt.TravelServiceCoroutineStub(channel)  
```


</div>

<!--
Opening a socket

Establishing TCP connection

Negotiating TLS (Transport Layer Security)

Starting HTTP/2 connection

Making the gRPC call
-->

---

# Integrating Protobuf and gRPC on Android
Travel App

### 7. Implementing functions
<br>
<v-clicks>

```java
  val channel: ManagedChannel = ManagedChannelBuilder.forAddress("localhost", 50051).build()
  val coroutineStub = TravelServiceGrpcKt.TravelServiceCoroutineStub(channel)
```

```java
  suspend fun getDestinations(location: String): List<Destination> {
      try {
          val request = getDestinationsRequest {                
              location = location
          }
          return coroutineStub.getDestinations(request).destinationsList
      } catch (e: Exception) {            
          throw e
      }
  }


  fun close() {
      channel.shutdownNow()
  }
```
</v-clicks>

---
layout: cover
---

# Run application

---
layout: two-cols
---

<template v-slot:default>
<img src="https://github.com/diegoalvis/travel-app-grpc/blob/main/screens/travel_app_gif.gif?raw=true" width="270" />
</template>

<template v-slot:right>

```java
class TravelService : Closeable {

    private val channel = createChannel(SERVER_URL)
    private val coroutineStub = TravelServiceGrpcKt.TravelServiceCoroutineStub(channel)

    suspend fun getDestinations(location: String): List<Destination> {
        try {
            val request = getDestinationsRequest { }
            return coroutineStub.getDestinations(request).destinationsList
        } catch (e: Exception) {
            throw e
        }
    }

    override fun close() {
        channel.shutdownNow()
    }
}

fun createChannel(serverUrl: String): ManagedChannel {
    val uri = Uri.parse(serverUrl)
    val builder = ManagedChannelBuilder.forAddress(uri.host, uri.port)
    if (uri.scheme == "https") {
        builder.useTransportSecurity()
    } else {
        builder.usePlaintext()
    }
    return builder
        .intercept(LoggingInterceptor())
        .executor(Dispatchers.IO.asExecutor())
        .build()
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
<img src="/res/images/chart_1.png" width="470" />
</div>
<div v-click>
CPU, Memory and Battery
<img src="/res/images/chart_5.png" width="470" />
</div>
</template>

<template v-slot:right>
<div v-click>

### Low Specs Device
Response time in ms
<img src="/res/images/chart_2.png" width="470" />
</div>
<div v-click>
CPU, Memory and Battery
<img src="/res/images/chart_6.png" width="470" />
</div>
</template>

---

# Trend comparison
Google search terms in the past 5 years

<br><br>
<img src="/res/images/chart_4_1.png" width="670" />

https://trends.google.com/trends/explore?cat=1227&date=today%205-y&q=GraphQL,REST,gRPC&hl=en

---

# Trend comparison
Google search terms in the past 5 years

<br><br>
<img src="/res/images/chart_4_2.png" width="670" />

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

- Needs more support from the mobile community

</v-clicks>

---
layout: two-cols
---

<template v-slot:default>
<br><br>

# Thank you!

<br><br>


<img src="/res/images/github_qr_code.png" width="150" />
<br>
Github: https://github.com/diegoalvis/travel-app-grpc

</template>

<template v-slot:right>
<br><br><br><br><br><br><br><br><br>
<br><br><br><br><br><br><br><br><br>

- <grommet-icons-mail />  diegoalvispal@gmail.com
- <grommet-icons-github />  https://github.com/diegoalvis
- <grommet-icons-linkedin />  https://www.linkedin.com/in/diegoalvispal
</template>
