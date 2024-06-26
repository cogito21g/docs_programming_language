#### 3.2. POCO C++ Libraries

POCO (Portable Components) C++ Libraries는 네트워크 프로그래밍, 파일 시스템, 동기화, 데이터베이스 접근, XML 및 JSON 파싱 등 다양한 기능을 제공하는 크로스 플랫폼 라이브러리입니다.

##### POCO 설치 및 설정

1. **Windows**:
   - [POCO 다운로드 페이지](https://pocoproject.org/download/index.html)에서 POCO를 다운로드합니다.
   - 압축을 풀고 Visual Studio에서 프로젝트를 생성한 후, POCO의 include 디렉토리와 lib 디렉토리를 설정합니다.
   - 필요한 라이브러리를 링크합니다: `PocoFoundation.lib`, `PocoNet.lib`, `PocoUtil.lib` 등.

2. **macOS**:
   - Homebrew를 사용하여 POCO를 설치합니다:
     ```bash
     brew install poco
     ```

3. **Linux**:
   - 패키지 관리자를 사용하여 POCO를 설치합니다:
     ```bash
     sudo apt-get install libpoco-dev
     ```

##### 기본 사용법

POCO를 사용하여 기본적인 TCP 서버와 클라이언트를 설정:

###### TCP 서버

```cpp
#include <Poco/Net/ServerSocket.h>
#include <Poco/Net/SocketStream.h>
#include <Poco/Net/StreamSocket.h>
#include <Poco/ThreadPool.h>
#include <iostream>

class TCPServerConnection : public Poco::Net::TCPServerConnection {
public:
    TCPServerConnection(const Poco::Net::StreamSocket& s) : Poco::Net::TCPServerConnection(s) {}

    void run() override {
        Poco::Net::SocketStream str(socket());
        str << "Hello from POCO TCP Server" << std::endl;
        str.flush();
    }
};

int main() {
    Poco::Net::ServerSocket svs(12345);
    Poco::ThreadPool::defaultPool().addCapacity(16);
    Poco::Net::TCPServer server(new Poco::Net::TCPServerConnectionFactoryImpl<TCPServerConnection>(), svs);
    server.start();

    std::cout << "TCP server is running..." << std::endl;
    std::cin.get();

    server.stop();
    return 0;
}
```

###### TCP 클라이언트

```cpp
#include <Poco/Net/StreamSocket.h>
#include <Poco/Net/SocketStream.h>
#include <iostream>

int main() {
    Poco::Net::SocketAddress sa("127.0.0.1", 12345);
    Poco::Net::StreamSocket socket(sa);
    Poco::Net::SocketStream str(socket);

    std::string line;
    std::getline(str, line);
    std::cout << "Received: " << line << std::endl;

    return 0;
}
```

##### 주요 기능

1. **네트워크 통신**:
   - TCP, UDP, HTTP/HTTPS, FTP 등 다양한 네트워크 프로토콜을 지원합니다.
2. **파일 시스템**:
   - 파일 및 디렉토리 조작, 파일 시스템 감시 등을 지원합니다.
3. **동기화**:
   - 스레드, 뮤텍스, 이벤트 등을 사용하여 동기화 작업을 수행할 수 있습니다.
4. **데이터베이스 접근**:
   - 다양한 데이터베이스(DB2, MySQL, PostgreSQL, SQLite)와의 통합을 지원합니다.
5. **XML 및 JSON 파싱**:
   - XML 및 JSON 데이터를 읽고 쓸 수 있습니다.

##### 예제 코드

간단한 TCP 서버와 클라이언트를 작성하여 네트워크 통신을 구현하는 예제입니다.

###### TCP 서버

```cpp
#include <Poco/Net/ServerSocket.h>
#include <Poco/Net/SocketStream.h>
#include <Poco/Net/StreamSocket.h>
#include <Poco/ThreadPool.h>
#include <iostream>

class TCPServerConnection : public Poco::Net::TCPServerConnection {
public:
    TCPServerConnection(const Poco::Net::StreamSocket& s) : Poco::Net::TCPServerConnection(s) {}

    void run() override {
        Poco::Net::SocketStream str(socket());
        str << "Hello from POCO TCP Server" << std::endl;
        str.flush();
    }
};

int main() {
    Poco::Net::ServerSocket svs(12345);
    Poco::ThreadPool::defaultPool().addCapacity(16);
    Poco::Net::TCPServer server(new Poco::Net::TCPServerConnectionFactoryImpl<TCPServerConnection>(), svs);
    server.start();

    std::cout << "TCP server is running..." << std::endl;
    std::cin.get();

    server.stop();
    return 0;
}
```

###### TCP 클라이언트

```cpp
#include <Poco/Net/StreamSocket.h>
#include <Poco/Net/SocketStream.h>
#include <iostream>

int main() {
    Poco::Net::SocketAddress sa("127.0.0.1", 12345);
    Poco::Net::StreamSocket socket(sa);
    Poco::Net::SocketStream str(socket);

    std::string line;
    std::getline(str, line);
    std::cout << "Received: " << line << std::endl;

    return 0;
}
```

이 예제에서는 POCO를 사용하여 간단한 TCP 에코 서버와 클라이언트를 구현합니다. 서버는 클라이언트로부터 메시지를 받아 다시 클라이언트에게 반환합니다.
