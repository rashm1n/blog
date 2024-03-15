---
author: Rashmin Mudunkotuwa
pubDatetime: 2022-09-23T15:22:00Z
modDatetime: 2023-12-21T09:12:47.400Z
title: gRPC — Why, What and How
slug: grpc-why-what-and-how
featured: true
draft: false
image: ../../assets/images/pic4.jpg
ogImage: ../../assets/images/pic4.jpg
tags:
  - golang
  - grpc
  - microservices
description: Introduction to gRPC
---

![something](@assets/images/pic4.jpg)

If you have been in the software industry for the last few years you must be well familiar with web services and how they are a major part of every enterprise application these days. SOAP, REST web services have been the front-runners of this web services revolution. But Since 2015 there is a new kid on the block ! Who is slowly getting traction and turning heads in the software industry. Its non other than the Google brain-child gRPC.

In this article lets go through the Whys, Whats and Hows of gRPC

## Remote Procedure Calls

```go
func main() {

}
```

Before we dig our hands into gRPC. Lets start with the basics. What is really a remote procedure call or in short, a RPC ?

A procedure call is a sequence of code which does some kind of specific task. The simplest example of a procedure call is any simple method. When you add the “remote” prefix to that word, it gives it whole another meaning.

When a procedure in another address space (another computer, another node in the network etc.) could be invoked from a different address space as if that procedure call was a local one, we say that it is a Remote Procedure Call. Essentially what a RPC does it it gives the “illusion” to the client that it is invoking a local method, but in reality it invokes a method in a remote machine while hiding the network communication details in the background.

If we dig inside a simple RPC call, it can be divided into several sub steps.

In a RPC there is generally a client and a server. The client is the code/program which is invoking the RPC and the server who is really invoking procedure call and sends the answer back.

### Steps

- Client invoking a mock code in the client side.

We normally call this mock implementation as a Stub. A stub can be defined as a block of code which stands for another set of code. And it is a Stub’s whole purpose, “act” as a real function, a facade so that the RPC can be invoked and processed in the background.

- Client stub packaging the parameters and necessary metadata into a message

In this step, the client stub packages the parameters and other metadata the client has injected into the stub method, into a compact message.

- Transporting the message via the network to the Server.

After the packaging of the metadata, the message is sent to the server through a network. And received by the server side.

- Unpackaging and invoking the procedure on the server side.

After the message is received by the server side, it un-packages the message and invokes the “real” method/procedure call in the server side.

- Re-packaging the response and sending to the client side.

After the response is recieved in the server side, the server code re-packages the response into a message and sends it through the same network to the client side

- Un-packages the response and returning the response

After the response message is recieved, it is un-packaged by the client stub code and returned as the response of the invoked client-stub method.

A fact we got to remember is that most of the times the above details are done in background in a RPC invocation, so that all the client has to do is invoke the stub method and wait for a result!

### gRPC

Now lets get into the topic of gRPC.

gRPC is a high performance RPC framework/technology built by Google. It uses Google’s own “Protocol Buffers”, which is an opensource message format for data serialization ,as the default method of communication between the client and the server. Similar to REST APIs mostly using JSON as the message format, gRPC uses Protocol Buffer (ProtoBuf for short) format as the message format and the IDL (interface definition language) to describe the payload parameters and response parameters.

As for the method of communication between the client stub and the server code, gRPC uses HTTP/2 as the default protocol. Since gRPC uses HTTP/2 features it supports 4 types of APIs.

1. Unary — Simple Client Server Communication. We will only focus on this format for simplicity, in this article.
2. Client Steaming
3. Server Steaming
4. Bi-Directional Streaming

### Protocol Buffers

In a simple Protocol Buffer message, data is structured as a message with name, value pairs.

After the developer has created the Protocol Buffer files, we can use a “protocol buffer compiler” to compile the written protocol buffer file which will generate all the utility classes and methods which are needed to work with the message. For example , getter and setter methods for messages (getName, setName, setId) are all automatically generated for that specific programming language.

If we take Java as an example, after compiling the .proto file, a class named Person would be auto-generated with all utility methods which needs to manipulate the Person message/data-type.

### Service Methods

After writing the necessary request and response message types, the next step is to write the service itself.

gRPC services are also defined in Protocol Buffers and they use the “service” and “rpc” keywords to define a service.

Above I have first created my two message types, HelloRequest and HelloResponse . Then starting with the rpc keyword, a simple gRPC service method has been declared with HelloRequest as the input and the HelloResponse as the output.

To learn more about the syntax and the structure of the Protocol Buffer format, you could refer the official protocol buffer docs.

After completing the ProtoBuf file, a programming language specific API has been provided by Google which will convert/transform this protobuf file to server methods and client stubs in that language. After the necessary files are generated by the gRPC API, you could implement those methods and create your method implementation as you wish. This will vary from a certain programming language to another.

When a service is invoked, as I talked above, the network communication is happened in the background and the invoked method would give the response after going the full-cycle.

This has been a simple introduction to the world of gRPC and RPCs. I have not explained the code level details and how to create and implement a gRPC service, and hope to talk about that in a future article.

Thank you very much for reading the article and follow for more ! Cheers.
