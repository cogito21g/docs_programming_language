#### 10.4. Protobuf와 gRPC 연동

Protobuf와 gRPC는 함께 사용하여 강력하고 효율적인 원격 프로시저 호출(RPC) 시스템을 구축할 수 있습니다. Protobuf는 데이터 직렬화를 담당하고, gRPC는 네트워크 통신을 처리합니다. 이 섹션에서는 Protobuf와 gRPC를 연동하여 서버와 클라이언트를 구현하는 방법을 설명합니다.

##### Protobuf 및 gRPC 파일 정의

**example.proto**:
```proto
syntax = "proto3";

package example;

service ExampleService {
    rpc SayHello (HelloRequest) returns (HelloReply);
}

message HelloRequest {
    string name = 1;
}

message HelloReply {
    string message = 1;
}
```

이 프로토콜 파일은 `ExampleService`라는 서비스를 정의하며, `SayHello` 메서드를 가집니다. `SayHello` 메서드는 `HelloRequest` 메시지를 받아 `HelloReply` 메시지를 반환합니다.

##### CMake 설정

**CMakeLists.txt**:
```cmake
cmake_minimum_required(VERSION 3.10)

project(ProtobufGrpcExample)

# C++ 표준 설정
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# Protobuf 및 gRPC 라이브러리 찾기
find_package(Protobuf REQUIRED)
find_package(gRPC REQUIRED)
include_directories(${Protobuf_INCLUDE_DIRS} ${gRPC_INCLUDE_DIRS})

# Protobuf 및 gRPC 컴파일
set(PROTO_SRC_DIR ${CMAKE_CURRENT_SOURCE_DIR})
set(PROTO_OUTPUT_DIR ${CMAKE_CURRENT_BINARY_DIR})

add_custom_command(
    OUTPUT ${PROTO_OUTPUT_DIR}/example.pb.cc ${PROTO_OUTPUT_DIR}/example.pb.h
           ${PROTO_OUTPUT_DIR}/example.grpc.pb.cc ${PROTO_OUTPUT_DIR}/example.grpc.pb.h
    COMMAND ${Protobuf_PROTOC_EXECUTABLE}
    ARGS --grpc_out=${PROTO_OUTPUT_DIR} --cpp_out=${PROTO_OUTPUT_DIR}
    --plugin=protoc-gen-grpc=${gRPC_CPP_PLUGIN_EXECUTABLE}
    ${PROTO_SRC_DIR}/example.proto
    DEPENDS ${PROTO_SRC_DIR}/example.proto)

# 실행 파일 추가
add_executable(server server.cpp ${PROTO_OUTPUT_DIR}/example.pb.cc ${PROTO_OUTPUT_DIR}/example.grpc.pb.cc)
add_executable(client client.cpp ${PROTO_OUTPUT_DIR}/example.pb.cc ${PROTO_OUTPUT_DIR}/example.grpc.pb.cc)

# Protobuf 및 gRPC 라이브러리 링크
target_link_libraries(server ${Protobuf_LIBRARIES} ${gRPC_LIBRARIES})
target_link_libraries(client ${Protobuf_LIBRARIES} ${gRPC_LIBRARIES})
```

##### 서버 구현

**server.cpp**:
```cpp
#include <iostream>
#include <memory>
#include <string>
#include <grpcpp/grpcpp.h>
#include "example.grpc.pb.h"

using grpc::Server;
using grpc::ServerBuilder;
using grpc::ServerContext;
using grpc::Status;
using example::ExampleService;
using example::HelloRequest;
using example::HelloReply;

class ExampleServiceImpl final : public ExampleService::Service {
    Status SayHello(ServerContext* context, const HelloRequest* request, HelloReply* reply) override {
        std::string prefix("Hello ");
        reply->set_message(prefix + request->name());
        return Status::OK;
    }
};

void RunServer() {
    std::string server_address("0.0.0.0:50051");
    ExampleServiceImpl service;

    ServerBuilder builder;
    builder.AddListeningPort(server_address, grpc::InsecureServerCredentials());
    builder.RegisterService(&service);
    std::unique_ptr<Server> server(builder.BuildAndStart());
    std::cout << "Server listening on " << server_address << std::endl;
    server->Wait();
}

int main(int argc, char** argv) {
    RunServer();
    return 0;
}
```

##### 클라이언트 구현

**client.cpp**:
```cpp
#include <iostream>
#include <memory>
#include <string>
#include <grpcpp/grpcpp.h>
#include "example.grpc.pb.h"

using grpc::Channel;
using grpc::ClientContext;
using grpc::Status;
using example::ExampleService;
using example::HelloRequest;
using example::HelloReply;

class ExampleClient {
public:
    ExampleClient(std::shared_ptr<Channel> channel)
        : stub_(ExampleService::NewStub(channel)) {}

    std::string SayHello(const std::string& name) {
        HelloRequest request;
        request.set_name(name);

        HelloReply reply;

        ClientContext context;
        Status status = stub_->SayHello(&context, request, &reply);

        if (status.ok()) {
            return reply.message();
        } else {
            return "RPC failed";
        }
    }

private:
    std::unique_ptr<ExampleService::Stub> stub_;
};

int main(int argc, char** argv) {
    ExampleClient client(grpc::CreateChannel("localhost:50051", grpc::InsecureChannelCredentials()));
    std::string user("world");
    std::string reply = client.SayHello(user);
    std::cout << "Client received: " << reply << std::endl;
    return 0;
}
```

##### 프로젝트 빌드 및 실행

1. 빌드 디렉터리를 생성하고 이동합니다:
   ```bash
   mkdir build
   cd build
   ```

2. CMake를 실행하여 빌드 시스템을 생성합니다:
   ```bash
   cmake ..
   ```

3. 서버와 클라이언트를 빌드합니다:
   ```bash
   make
   ```

4. 서버를 실행합니다:
   ```bash
   ./server
   ```

5. 다른 터미널에서 클라이언트를 실행합니다:
   ```bash
   ./client
   ```
