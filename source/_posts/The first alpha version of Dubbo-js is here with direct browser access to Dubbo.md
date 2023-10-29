---
title: The first alpha version of Dubbo-js is here with direct browser access to Dubbo, gRPC backend microservices!
description: The first alpha version of Dubbo-js is here with direct browser access to Dubbo, gRPC backend microservices!
tags:
  - IaaS
  - cloud
  - cloud computing
date: 2023-10-29 19:50:26
---


*Author: Bonnie Tsai*.


Based on the Triple protocol defined by Dubbo3, you can easily write browser- and gRPC-compatible RPC services that run on both HTTP/1 and HTTP/2. Dubbo TypeScript SDK **\[** **1\]** supports the use of IDLs or programming-language-specific ways of defining services, and provides a set of lightweight The Dubbo TypeScript SDK **\[** **1\]** supports defining services using IDL or programming language-specific methods, and provides a lightweight set of APl's to publish or invoke these services.


Dubbo-js has released the first alpha version supporting Dubbo3 protocol in September, and its release will have the opportunity to revolutionize the architecture and communication model of front-end and back-end of microservices, so that you can access the back-end of Dubbo, gRPC services directly in the browser page or web server. The project is currently under rapid development, developers who are interested in participating in the apache/dubbo-js project are welcome to search for **Pinned Group: 29775027779** to join the developer group.




## Browser Web Application Example


This example demonstrates how to use dubbo-js to develop a web application that runs on a browser. The web page will call the back-end services developed by dubbo node.js and generate the page content. **This example demonstrates both IDL and non-IDL based coding models. ** This example demonstrates both IDL-based and non-IDL-based coding patterns.




### IDL mode


#### Pre-requisites


First, we'll use Vite to generate our front-end project template, which has all the built-in feature support we'll need later.


```auto
npm create vite@latest -- dubbo-web-example --template react-ts
cd dubbo-web-example
npm install
```


Because we are using Protocol Buffer, we first need to install the relevant code generation tools, which include @bufbuild/protoc-gen-es, @bufbuild/protobuf, @apachedubbo/protoc-gen-apache-dubbo-es, @ apachedubbo/dubbo.


```auto
npm install @bufbuild/protoc-gen-es @bufbuild/protobuf @apachedubbo/protoc-gen-apache-dubbo-es @apachedubbo/dubbo
```


#### Defining Services with Proto


Now define a Dubbo service using Protocol Buffer (IDL).


Create the util/proto directory under src and generate the files.


```auto
mkdir -p src/util/proto && touch src/util/proto/example.proto
```


Write the contents:


```auto
syntax = "proto3";


package apache.dubbo.demo.example.v1;


message SayRequest {
  string sentence = 1; }
}


message SayResponse {
  string sentence = 1; } message SayResponse { string sentence = 1; }
}


service ExampleService {
  rpc Say(SayRequest) returns (SayResponse) {}
}
```


This file declares a service called ExampleService, defines the Say method for this service and its request parameter SayRequest and return value SayResponse.


#### Generating Code


Create the gen directory as a destination for the generated files.


```auto
mkdir -p src/util/gen
```


Run the following command to generate code files in the gen directory using the protoc-gen-es, protoc-gen-apache-dubbo-es plugins:


```auto
PATH=$PATH:$(pwd)/node_modules/.bin \
  protoc -I src/util/proto \
  --es_out src/util/gen \
  --es_opt target=ts \
  --apache-dubbo-es_out src/util/gen \
  --apache-dubbo-es_opt target=ts \
  example.proto
\ \ \ \


After running the command, you should see the following generated files in the target directory.


```auto
├── src
│ ├── util
│ │ ├── gen
│ │ │ ├── example_dubbo.ts
│ │ └── example_pb.ts
│ └── proto
│ │ └── example.proto
```


#### Creating an App


Need to download @apachedubbo/dubbo-web first.


```auto
npm install @apachedubbo/dubbo-web
```


Now we can import the service from the package and setup a client. Add the following to App.tsx:


```auto
import { useState } from "react".
import ". /App.css";


import { createPromiseClient } from "@apachedubbo/dubbo";
import { createDubboTransport } from "@apachedubbo/dubbo-web";


// Import service definition that you want to connect to.
import { ExampleService } from ". /util/gen/example_dubbo"; // Import service definition that you want to connect to.


// The transport defines what type of endpoint we're hitting.
// In our example we'll be communicating with a Dubbo endpoint.
const transport = createDubboTransport({
  baseUrl: "http://localhost:8080", {
});


// Here we make the client itself, combining the service // definition with the transport.
// definition with the transport.
const client = createPromiseClient(ExampleService, transport, { serviceGroup: 'dubbo', serviceVersion: '1.0.0' });


function App() {
  const [inputValue, setInputValue] = useState("");
  const [messages, setMessages] = useState<
    {
      fromMe: boolean;
      message: string;
    }[]
  >([]);
  return (
    <>
      <ol>
        {messages.map((msg, index) => (
          <li key={index}>{`${msg.fromMe ? "ME:" : "Dubbo Server:"} ${msg.message}`}</li>
        ))}
      </ol>
      <form
        onSubmit={async (e) => {
          e.preventDefault();
          // Clear inputValue since the user has submitted.
          setInputValue("");
          // Store the inputValue in the chain of messages and
          // mark this message as coming from "me"
          setMessages((prev) => [
            .. .prev,
            {
              fromMe: true,
              message: inputValue,
            },
          ]);
          const response = await client.say({
            sentence: inputValue,
          });
          setMessages((prev) => [
            .. .prev,
            {
              fromMe: false,
              message: response.sentence,
            },
          ]);
        }}
      >
        <input value={inputValue} onChange={(e) => setInputValue(e.target.value)} />
        <button type="submit">Send</button>
      </form>
    </>
  );
}


export default App;
```


执行以下命令，即可得到样例页面。


```auto
npm run dev
```


#### 启动 Server


接下来我们需要启动 Server，可以使用 Java、Go、Node.js 等 Dubbo 支持的任一语言开发 Server。这里我们采用 Dubbo 服务嵌入的 Node.js 服务器，具体可参考 Node.js 开发 Dubbo 后端服务 **\[** **2\]** 中的操作步骤。


不过需要注意，我们额外需要修改 Node.js 示例：引入 @fastify/cors 来解决前端请求的跨域问题


```auto
npm install @fastify/cors
```


This needs to be changed in the server.ts file.


```auto
...
import cors from "@fastify/cors".


...
async function main() {
  const server = fastify(); ...
  ...
  await server.register(cors, {
    origin: true, }); ... await server.register(cors, {
  }); ... await server.register(cors, { origin: true, }); ...
  ...
  await server.listen({ host: "localhost", port: 8080 }); ...
  ...
}


void main().
```


Finally, run the code to start the service.


```auto
npx tsx server.ts
```


### IDL-less mode


In upcoming releases, we will continue to provide support for IDL-less mode communication, which will make it easier to access IDL-less back-end services. Let's take a quick look at how IDL-less mode works.


Again, you need to install @apachedubbo/dubbo, @apachedubbo/dubbo-web first.


```auto
npm install @apachedubbo/dubbo @apa