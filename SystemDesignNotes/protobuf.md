# Protobuf - Protocol Buffer

## Introduction
Protocol Buffer present a way to communicate between different services/systems. The abstracting the communication from the language and technology those services run in.

isn't XML/JSON doing the same thing ?

Yes, they are doing the same thing. Then what makes protobuf special ?

The data format XML/JSON are still human readable, for a noobie it might see daunting but still readable. Do we really need that ? As long as we can convert into human readable format we should be good.

For each data point we need additional 5 bytes; "" : "" which makes it unsuitable for vvv large documents.

Protobuf solves this problem by shrinking data and sending it over the network, the receiver can then decode it.

Google advertises its ProtoBuf like this:

```
Protocol buffers are Google's language-neutral, platform-neutral,
extensible mechanism for serializing structured data – think XML, but 
smaller, faster, and simpler. You define how you want your data to be 
structured once …
```

## .proto files and more

The data sent using protobuf protocol needs to follow a specific structure. This data/payload are defined as msgs in .proto files

Example:

```python
syntax = "proto3";

package protoblog;

message TodoList {
   // Elements of the todo list will be defined here
   ...
}

```

Those files are then used to generate integration classes or stubs for the language of your choice using code generators within the protoc compiler. The current version, Proto3, already supports all the major programming languages. The community supports many more in third-party open-source implementations.

Generated classes are the core elements of Protocol Buffers. They allow the creation of elements by instantiating new messages, based on the .proto files, which are then used for serialization.

The binary data can then be stored, sent over the network, and used any other way human-readable data like JSON or XML is. After transmission or storage, the byte-stream can be deserialized and restored using any language-specific, compiled protobuf class we generate from the .proto
file.

## How to generate protoc from .proto

```bash
protoc -I=. --python_out=. ./todolist.proto
```

## How to serialize & deserialize the data

```python

with open("./serializedFile", "wb") as fd:
    fd.write(my_list.SerializeToString())


my_list = TodoList.TodoList()
with open("./serializedFile", "rb") as fd:
    my_list.ParseFromString(fd.read())


```

## Why don't we use it everywhere

Debugging is difficult, cant debug just looking at network logs or the communication channel

Iteration times in engineering also tend to increase since updates in the data require updating the proto files before usage.

client-server needs to broker a proto file, an unknown client cant communicate with an unknown server


## Where would I suggest to use it ?

- Transferring large number of heavy documents/data between the services
- pub/sub model
- When data is drilled and passed through multiple services, proto structure helps to ensure data format integrity. 

## Where have I used it ?
 In JPMorgan, where huge data was passed through multiple systems on a data bus and ensuring the specific format was of paramount importance. In totality the decision was prompted by:
 
 1. The need to transfer large data
 2. Format restriction and enforcement


 ## Recent interesting implementation in public domain

 Linkedin switched to using proto to transfer data between its services instead of json which lead to 60% decrease in the latency.

