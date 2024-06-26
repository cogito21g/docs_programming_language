#### 7.2. gRPC 기본 사용법

##### gRPC 서버 구현

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

이 예제에서는 `ExampleServiceImpl` 클래스를 정의하여 `ExampleService` 서비스를 구현합니다. `SayHello` 메서드는 `HelloRequest`를 받아 `HelloReply` 메시지를 반환합니다.

##### gRPC 클라이언트 구현

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

이 예제에서는 `ExampleClient` 클래스를 정의하여 `ExampleService` 서비스에 접근합니다. `SayHello` 메서드는 `HelloRequest`를 서버에 보내고, 서버의 응답을 받아 출력합니다.

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
