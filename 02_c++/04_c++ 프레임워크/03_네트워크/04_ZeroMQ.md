#### 3.4. ZeroMQ

ZeroMQ는 고성능 비동기 메시지 라이브러리로, 메시징 패턴(Request-Reply, Publish-Subscribe, Push-Pull 등)을 사용하여 네트워크 통신을 간단하게 구현할 수 있습니다. ZeroMQ는 크로스 플랫폼을 지원하며, 다양한 언어 바인딩을 제공합니다.

##### ZeroMQ 설치 및 설정

1. **Windows**:
   - [ZeroMQ 다운로드 페이지](https://zeromq.org/download/)에서 Windows용 ZeroMQ를 다운로드합니다.
   - 압축을 풀고 Visual Studio에서 프로젝트를 생성한 후, ZeroMQ의 include 디렉토리와 lib 디렉토리를 설정합니다.
   - 필요한 라이브러리를 링크합니다: `libzmq.lib`.

2. **macOS**:
   - Homebrew를 사용하여 ZeroMQ를 설치합니다:
     ```bash
     brew install zeromq
     ```

3. **Linux**:
   - 패키지 관리자를 사용하여 ZeroMQ를 설치합니다:
     ```bash
     sudo apt-get install libzmq3-dev
     ```

##### 기본 사용법

ZeroMQ를 사용하여 간단한 Request-Reply 패턴을 구현:

###### 서버 (Reply)

```cpp
#include <zmq.hpp>
#include <iostream>
#include <string>
#include <thread>
#include <chrono>

int main() {
    zmq::context_t context(1);
    zmq::socket_t socket(context, zmq::socket_type::rep);
    socket.bind("tcp://*:5555");

    while (true) {
        zmq::message_t request;
        socket.recv(request, zmq::recv_flags::none);
        std::cout << "Received: " << request.to_string() << std::endl;

        std::this_thread::sleep_for(std::chrono::seconds(1));

        zmq::message_t reply(5);
        memcpy(reply.data(), "World", 5);
        socket.send(reply, zmq::send_flags::none);
    }
    return 0;
}
```

###### 클라이언트 (Request)

```cpp
#include <zmq.hpp>
#include <iostream>
#include <string>

int main() {
    zmq::context_t context(1);
    zmq::socket_t socket(context, zmq::socket_type::req);
    socket.connect("tcp://localhost:5555");

    for (int i = 0; i < 10; ++i) {
        zmq::message_t request(5);
        memcpy(request.data(), "Hello", 5);
        socket.send(request, zmq::send_flags::none);

        zmq::message_t reply;
        socket.recv(reply, zmq::recv_flags::none);
        std::cout << "Received: " << reply.to_string() << std::endl;
    }
    return 0;
}
```

##### 주요 기능

1. **메시징 패턴**:
   - 다양한 메시징 패턴(Request-Reply, Publish-Subscribe, Push-Pull 등)을 지원합니다.
2. **비동기 I/O**:
   - 비동기 통신을 지원하여 높은 성능과 확장성을 제공합니다.
3. **멀티프로토콜 지원**:
   - TCP, IPC, PGM, EPGM, INPROC 등 다양한 프로토콜을 지원합니다.
4. **멀티플랫폼 지원**:
   - Windows, macOS, Linux 등 다양한 플랫폼을 지원합니다.
5. **다양한 언어 바인딩**:
   - C++, Python, Java, Go 등 다양한 언어 바인딩을 제공합니다.

##### 예제 코드

간단한 Publish-Subscribe 패턴을 구현하는 ZeroMQ 코드입니다.

###### 퍼블리셔 (Publisher)

```cpp
#include <zmq.hpp>
#include <iostream>
#include <thread>
#include <chrono>

int main() {
    zmq::context_t context(1);
    zmq::socket_t publisher(context, zmq::socket_type::pub);
    publisher.bind("tcp://*:5556");

    while (true) {
        zmq::message_t message(6);
        memcpy(message.data(), "Hello", 5);
        publisher.send(message, zmq::send_flags::none);
        std::this_thread::sleep_for(std::chrono::seconds(1));
    }
    return 0;
}
```

###### 구독자 (Subscriber)

```cpp
#include <zmq.hpp>
#include <iostream>
#include <string>

int main() {
    zmq::context_t context(1);
    zmq::socket_t subscriber(context, zmq::socket_type::sub);
    subscriber.connect("tcp://localhost:5556");
    subscriber.set(zmq::sockopt::subscribe, "");

    while (true) {
        zmq::message_t message;
        subscriber.recv(message, zmq::recv_flags::none);
        std::cout << "Received: " << message.to_string() << std::endl;
    }
    return 0;
}
```

이 예제에서는 ZeroMQ를 사용하여 퍼블리셔가 "Hello" 메시지를 보내고, 구독자가 이를 받아 출력하는 간단한 Publish-Subscribe 패턴을 구현합니다.
