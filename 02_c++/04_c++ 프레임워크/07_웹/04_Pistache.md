#### 7.4. Pistache

Pistache는 C++로 작성된 경량의 고성능 REST API 서버 프레임워크입니다. 간단한 API로 쉽게 웹 서버를 구축할 수 있으며, 비동기 I/O와 멀티스레딩을 지원합니다.

##### Pistache 설치 및 설정

1. **Windows**:
   - Windows에서 Pistache를 설치하려면 CMake를 사용하여 소스를 빌드해야 합니다.
   - [Pistache GitHub 페이지](https://github.com/pistacheio/pistache)에서 소스를 다운로드합니다.
   - CMake를 사용하여 빌드합니다.

2. **macOS**:
   - Homebrew를 사용하여 Pistache를 설치합니다:
     ```bash
     brew install pistache
     ```

3. **Linux**:
   - 패키지 관리자를 사용하여 Pistache를 설치합니다:
     ```bash
     sudo apt-get install libpistache-dev
     ```

##### 기본 사용법

Pistache를 사용하여 간단한 웹 서버 구현:

1. **코드 작성**

```cpp
#include <pistache/endpoint.h>

using namespace Pistache;

class HelloHandler : public Http::Handler {
public:
    HTTP_PROTOTYPE(HelloHandler)

    void onRequest(const Http::Request& request, Http::ResponseWriter response) override {
        if (request.resource() == "/json") {
            response.send(Http::Code::Ok, R"({"message": "Hello, Pistache!"})", MIME(Application, Json));
        } else {
            response.send(Http::Code::Ok, "Hello, Pistache!");
        }
    }
};

int main() {
    Http::Endpoint server(Address("localhost", Port(9080)));

    auto opts = Http::Endpoint::options()
                .threads(1)
                .flags(Tcp::Options::ReuseAddr);

    server.init(opts);
    server.setHandler(Http::make_handler<HelloHandler>());
    server.serve();

    server.shutdown();
}
```

2. **CMake 설정**

```cmake
cmake_minimum_required(VERSION 3.10)
project(PistacheExample)

set(CMAKE_CXX_STANDARD 11)

# Pistache 라이브러리 설정
find_package(Pistache REQUIRED)
include_directories(${PISTACHE_INCLUDE_DIR})

# 코드 추가
add_executable(runExample main.cpp)
target_link_libraries(runExample Pistache::pistache)
```

3. **빌드 및 실행**

```bash
mkdir build
cd build
cmake ..
make
./runExample
```

##### 주요 기능

1. **REST API 작성**:
   - 간단한 API로 RESTful 웹 서버를 쉽게 작성할 수 있습니다.
2. **비동기 I/O**:
   - 비동기 I/O를 지원하여 고성능 서버를 구축할 수 있습니다.
3. **멀티스레딩**:
   - 멀티스레드 모드를 지원하여 고성능 서버를 구축할 수 있습니다.
4. **간단한 API**:
   - 간단하고 직관적인 API로 빠르게 웹 서버를 구축할 수 있습니다.
5. **HTTP 및 HTTPS 지원**:
   - HTTP 및 HTTPS 프로토콜을 모두 지원합니다.

##### 예제 코드

간단한 웹 서버를 구축하고 JSON 데이터를 처리하는 Pistache 코드입니다:

1. **코드 작성**

```cpp
#include <pistache/endpoint.h>

using namespace Pistache;

class HelloHandler : public Http::Handler {
public:
    HTTP_PROTOTYPE(HelloHandler)

    void onRequest(const Http::Request& request, Http::ResponseWriter response) override {
        if (request.resource() == "/json") {
            response.send(Http::Code::Ok, R"({"message": "Hello, Pistache!"})", MIME(Application, Json));
        } else {
            response.send(Http::Code::Ok, "Hello, Pistache!");
        }
    }
};

int main() {
    Http::Endpoint server(Address("localhost", Port(9080)));

    auto opts = Http::Endpoint::options()
                .threads(1)
                .flags(Tcp::Options::ReuseAddr);

    server.init(opts);
    server.setHandler(Http::make_handler<HelloHandler>());
    server.serve();

    server.shutdown();
}
```

이 예제에서는 Pistache를 사용하여 간단한 웹 서버를 구축하고, `/` 엔드포인트에서 문자열을 반환하고, `/json` 엔드포인트에서 JSON 데이터를 반환하는 프로그램을 구현합니다.
