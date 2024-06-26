### 7. gRPC

gRPC는 원격 프로시저 호출(RPC) 시스템으로, 분산 시스템 간의 통신을 용이하게 합니다. gRPC는 HTTP/2를 기반으로 하며, 프로토콜 버퍼(Protocol Buffers)를 사용하여 인터페이스와 메시지를 정의합니다.

#### 7.1. gRPC 개요 및 설치

##### gRPC 개요

gRPC는 클라이언트와 서버 간의 통신을 지원하며, 주로 다음과 같은 특징을 가집니다:

- HTTP/2를 사용하여 효율적인 통신을 제공
- 프로토콜 버퍼를 사용하여 인터페이스와 메시지를 정의
- 다양한 언어와 플랫폼을 지원
- 스트리밍 및 양방향 통신을 지원

##### gRPC 설치

**Linux 및 macOS**:

1. gRPC 및 프로토콜 버퍼 컴파일러 설치:
   ```bash
   sudo apt-get install build-essential autoconf libtool pkg-config
   sudo apt-get install cmake
   git clone --recurse-submodules -b v1.39.0 https://github.com/grpc/grpc
   cd grpc
   mkdir -p cmake/build
   pushd cmake/build
   cmake ../..
   make
   sudo make install
   popd
   ```

2. 프로토콜 버퍼 컴파일러 설치:
   ```bash
   sudo apt-get install protobuf-compiler
   ```

**Windows**:

1. Visual Studio를 설치합니다.
2. [gRPC 설치 가이드](https://grpc.io/docs/languages/cpp/quickstart/)를 참고하여 설치합니다.

##### gRPC 프로젝트 설정

프로젝트에 gRPC를 설정하려면 `CMakeLists.txt` 파일에 다음과 같은 내용을 추가합니다.

**프로젝트 구조**:
```
gRPCExample/
├── CMakeLists.txt
├── server.cpp
├── client.cpp
├── example.proto
```

**CMakeLists.txt**:
```cmake
cmake_minimum_required(VERSION 3.10)

project(gRPCExample)

# C++ 표준 설정
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# gRPC 및 Protobuf 경로 설정
set(PROTOBUF_IMPORT_DIRS "/usr/local/include")
find_package(PkgConfig REQUIRED)
pkg_check_modules(GRPC REQUIRED grpc++ grpc)

include_directories(${PROTOBUF_IMPORT_DIRS})
include_directories(${GRPC_INCLUDE_DIRS})

# 프로토콜 버퍼 컴파일
find_program(PROTOC_EXECUTABLE protoc)
find_program(GRPC_CPP_PLUGIN_EXECUTABLE grpc_cpp_plugin)

set(PROTO_SRC_DIR ${CMAKE_CURRENT_SOURCE_DIR})
set(PROTO_OUTPUT_DIR ${CMAKE_CURRENT_BINARY_DIR})

add_custom_command(
    OUTPUT ${PROTO_OUTPUT_DIR}/example.pb.cc ${PROTO_OUTPUT_DIR}/example.pb.h ${PROTO_OUTPUT_DIR}/example.grpc.pb.cc ${PROTO_OUTPUT_DIR}/example.grpc.pb.h
    COMMAND ${PROTOC_EXECUTABLE}
    ARGS --grpc_out=${PROTO_OUTPUT_DIR} --cpp_out=${PROTO_OUTPUT_DIR}
    --plugin=protoc-gen-grpc=${GRPC_CPP_PLUGIN_EXECUTABLE}
    ${PROTO_SRC_DIR}/example.proto
    DEPENDS ${PROTO_SRC_DIR}/example.proto)

# 서버와 클라이언트 컴파일 설정
add_executable(server server.cpp ${PROTO_OUTPUT_DIR}/example.pb.cc ${PROTO_OUTPUT_DIR}/example.grpc.pb.cc)
target_link_libraries(server ${GRPC_LIBRARIES} ${PROTOBUF_LIBRARIES})

add_executable(client client.cpp ${PROTO_OUTPUT_DIR}/example.pb.cc ${PROTO_OUTPUT_DIR}/example.grpc.pb.cc)
target_link_libraries(client ${GRPC_LIBRARIES} ${PROTOBUF_LIBRARIES})
```

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

