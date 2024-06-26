#### 8.2. Thrift 기본 사용법

##### Thrift 서버 구현

**server.cpp**:
```cpp
#include <iostream>
#include <memory>
#include <string>
#include <thrift/Thrift.h>
#include <thrift/transport/TServerSocket.h>
#include <thrift/transport/TBufferTransports.h>
#include <thrift/protocol/TBinaryProtocol.h>
#include <thrift/server/TSimpleServer.h>
#include "example_constants.h"
#include "example_types.h"
#include "ExampleService.h"

using namespace ::apache::thrift;
using namespace ::apache::thrift::protocol;
using namespace ::apache::thrift::transport;
using namespace ::apache::thrift::server;
using namespace example;

class ExampleServiceHandler : public ExampleServiceIf {
public:
    ExampleServiceHandler() = default;

    void sayHello(std::string& _return, const std::string& name) override {
        _return = "Hello, " + name + "!";
    }
};

int main() {
    int port = 9090;
    std::shared_ptr<ExampleServiceHandler> handler(new ExampleServiceHandler());
    std::shared_ptr<TProcessor> processor(new ExampleServiceProcessor(handler));
    std::shared_ptr<TServerTransport> serverTransport(new TServerSocket(port));
    std::shared_ptr<TTransportFactory> transportFactory(new TBufferedTransportFactory());
    std::shared_ptr<TProtocolFactory> protocolFactory(new TBinaryProtocolFactory());

    TSimpleServer server(processor, serverTransport, transportFactory, protocolFactory);
    std::cout << "Starting the server..." << std::endl;
    server.serve();
    std::cout << "Server stopped." << std::endl;
    return 0;
}
```

이 예제에서는 `ExampleServiceHandler` 클래스를 정의하여 `ExampleService` 서비스를 구현합니다. `sayHello` 메서드는 `name`을 받아 인사말을 반환합니다.

##### Thrift 클라이언트 구현

**client.cpp**:
```cpp
#include <iostream>
#include <memory>
#include <string>
#include <thrift/Thrift.h>
#include <thrift/transport/TSocket.h>
#include <thrift/transport/TBufferTransports.h>
#include <thrift/protocol/TBinaryProtocol.h>
#include "example_constants.h"
#include "example_types.h"
#include "ExampleService.h"

using namespace ::apache::thrift;
using namespace ::apache::thrift::protocol;
using namespace ::apache::thrift::transport;
using namespace example;

int main() {
    std::shared_ptr<TTransport> socket(new TSocket("localhost", 9090));
    std::shared_ptr<TTransport> transport(new TBufferedTransport(socket));
    std::shared_ptr<TProtocol> protocol(new TBinaryProtocol(transport));
    ExampleServiceClient client(protocol);

    try {
        transport->open();

        std::string response;
        client.sayHello(response, "world");
        std::cout << "Client received: " << response << std::endl;

        transport->close();
    } catch (TException& tx) {
        std::cout << "ERROR: " << tx.what() << std::endl;
    }

    return 0;
}
```

이 예제에서는 `ExampleServiceClient` 클래스를 사용하여 `ExampleService` 서비스에 접근합니다. `sayHello` 메서드는 `name`을 서버에 보내고, 서버의 응답을 받아 출력합니다.

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
