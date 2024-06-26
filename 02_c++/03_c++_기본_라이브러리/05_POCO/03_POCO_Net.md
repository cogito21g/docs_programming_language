#### 5.3. POCO Net

POCO Net 라이브러리는 네트워크 프로그래밍을 위한 다양한 기능을 제공합니다. 이 라이브러리에는 소켓, HTTP 클라이언트 및 서버, FTP 클라이언트, SMTP 클라이언트 등이 포함되어 있습니다.

##### 예제 1: TCP 소켓 클라이언트

POCO Net 라이브러리를 사용하여 TCP 소켓 클라이언트를 구현할 수 있습니다.

**프로젝트 구조**:
```
TcpClientExample/
├── CMakeLists.txt
└── main.cpp
```

**CMakeLists.txt**:
```cmake
cmake_minimum_required(VERSION 3.10)

# 프로젝트 이름 설정
project(TcpClientExample)

# C++ 표준 설정
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# POCO 라이브러리 찾기
find_package(Poco REQUIRED Net Foundation)

# 실행 파일 추가
add_executable(TcpClientExample main.cpp)

# POCO 라이브러리 링크
target_link_libraries(TcpClientExample Poco::Net Poco::Foundation)
```

**main.cpp**:
```cpp
#include <iostream>
#include <Poco/Net/StreamSocket.h>
#include <Poco/Net/SocketStream.h>
#include <Poco/Net/SocketAddress.h>

int main() {
    try {
        Poco::Net::SocketAddress sa("localhost", 8080);
        Poco::Net::StreamSocket socket(sa);
        Poco::Net::SocketStream str(socket);

        str << "Hello from POCO client!" << std::endl;
        str.flush();

        std::string response;
        std::getline(str, response);
        std::cout << "Response from server: " << response << std::endl;
    } catch (Poco::Exception& ex) {
        std::cerr << "Error: " << ex.displayText() << std::endl;
    }

    return 0;
}
```

이 예제에서는 `Poco::Net::StreamSocket`과 `Poco::Net::SocketStream` 클래스를 사용하여 TCP 소켓 클라이언트를 구현합니다. 클라이언트는 서버에 연결하고, 메시지를 전송한 후 서버로부터 응답을 받습니다.

##### 예제 2: HTTP 클라이언트

POCO Net 라이브러리를 사용하여 HTTP 클라이언트를 구현할 수 있습니다.

**프로젝트 구조**:
```
HttpClientExample/
├── CMakeLists.txt
└── main.cpp
```

**CMakeLists.txt**:
```cmake
cmake_minimum_required(VERSION 3.10)

# 프로젝트 이름 설정
project(HttpClientExample)

# C++ 표준 설정
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# POCO 라이브러리 찾기
find_package(Poco REQUIRED Net Foundation)

# 실행 파일 추가
add_executable(HttpClientExample main.cpp)

# POCO 라이브러리 링크
target_link_libraries(HttpClientExample Poco::Net Poco::Foundation)
```

**main.cpp**:
```cpp
#include <iostream>
#include <Poco/Net/HTTPClientSession.h>
#include <Poco/Net/HTTPRequest.h>
#include <Poco/Net/HTTPResponse.h>
#include <Poco/StreamCopier.h>
#include <Poco/Path.h>
#include <Poco/URI.h>

int main() {
    try {
        Poco::URI uri("http://www.example.com");
        Poco::Net::HTTPClientSession session(uri.getHost(), uri.getPort());
        Poco::Net::HTTPRequest req(Poco::Net::HTTPRequest::HTTP_GET, uri.getPathAndQuery(), Poco::Net::HTTPRequest::HTTP_1_1);
        session.sendRequest(req);

        Poco::Net::HTTPResponse res;
        std::istream& rs = session.receiveResponse(res);

        std::cout << "Response status: " << res.getStatus() << " " << res.getReason() << std::endl;

        std::ostringstream oss;
        Poco::StreamCopier::copyStream(rs, oss);
        std::cout << "Response body: " << oss.str() << std::endl;
    } catch (Poco::Exception& ex) {
        std::cerr << "Error: " << ex.displayText() << std::endl;
    }

    return 0;
}
```

이 예제에서는 `Poco::Net::HTTPClientSession`, `Poco::Net::HTTPRequest`, `Poco::Net::HTTPResponse` 클래스를 사용하여 HTTP 클라이언트를 구현합니다. 클라이언트는 지정된 URL로 GET 요청을 보내고, 서버로부터 응답을 받아 출력합니다.

##### 예제 3: HTTP 서버

POCO Net 라이브러리를 사용하여 HTTP 서버를 구현할 수 있습니다.

**프로젝트 구조**:
```
HttpServerExample/
├── CMakeLists.txt
└── main.cpp
```

**CMakeLists.txt**:
```cmake
cmake_minimum_required(VERSION 3.10)

# 프로젝트 이름 설정
project(HttpServerExample)

# C++ 표준 설정
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# POCO 라이브러리 찾기
find_package(Poco REQUIRED Net Foundation)

# 실행 파일 추가
add_executable(HttpServerExample main.cpp)

# POCO 라이브러리 링크
target_link_libraries(HttpServerExample Poco::Net Poco::Foundation)
```

**main.cpp**:
```cpp
#include <iostream>
#include <Poco/Net/HTTPServer.h>
#include <Poco/Net/HTTPServerRequest.h>
#include <Poco/Net/HTTPServerResponse.h>
#include <Poco/Net/HTTPRequestHandler.h>
#include <Poco/Net/HTTPRequestHandlerFactory.h>
#include <Poco/Net/ServerSocket.h>
#include <Poco/Net/HTTPServerParams.h>
#include <Poco/Util/ServerApplication.h>
#include <Poco/Util/Option.h>
#include <Poco/Util/OptionSet.h>
#include <Poco/Util/HelpFormatter.h>

class MyRequestHandler : public Poco::Net::HTTPRequestHandler {
public:
    void handleRequest(Poco::Net::HTTPServerRequest& request, Poco::Net::HTTPServerResponse& response) override {
        response.setContentType("text/html");
        std::ostream& out = response.send();
        out << "<html><head><title>HTTP Server</title></head>";
        out << "<body><h1>Hello from POCO HTTP Server!</h1></body></html>";
    }
};

class MyRequestHandlerFactory : public Poco::Net::HTTPRequestHandlerFactory {
public:
    Poco::Net::HTTPRequestHandler* createRequestHandler(const Poco::Net::HTTPServerRequest&) override {
        return new MyRequestHandler;
    }
};

class MyHTTPServerApp : public Poco::Util::ServerApplication {
protected:
    void initialize(Application& self) override {
        loadConfiguration();
        ServerApplication::initialize(self);
    }

    void uninitialize() override {
        ServerApplication::uninitialize();
    }

    int main(const std::vector<std::string>& args) override {
        Poco::UInt16 port = (unsigned short) config().getInt("HTTPServer.port", 8080);
        Poco::Net::ServerSocket svs(port);
        Poco::Net::HTTPServer srv(new MyRequestHandlerFactory, svs, new Poco::Net::HTTPServerParams);

        srv.start();
        std::cout << "HTTP Server started on port " << port << std::endl;
        waitForTerminationRequest();
        srv.stop();
        return Application::EXIT_OK;
    }
};

int main(int argc, char** argv) {
    MyHTTPServerApp app;
    return app.run(argc, argv);
}
```

이 예제에서는 `Poco::Net::HTTPServer`, `Poco::Net::HTTPRequestHandler`, `Poco::Net::HTTPRequestHandlerFactory`, `Poco::Util::ServerApplication` 클래스를 사용하여 HTTP 서버를 구현합니다. 서버는 포트 8080에서 클라이언트의 요청을 받고, 간단한 HTML 응답을 반환합니다.
