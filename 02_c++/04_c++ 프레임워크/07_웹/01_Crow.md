### 7. 웹 프레임워크 (Web Frameworks)

#### 7.1. Crow

Crow는 C++을 사용하여 빠르고 간편하게 웹 서버를 구축할 수 있는 경량 웹 프레임워크입니다. RESTful API를 작성하고, JSON 데이터를 처리하는 데 유용합니다.

##### Crow 설치 및 설정

1. **Windows, macOS, Linux**:
   - [Crow GitHub 페이지](https://github.com/CrowCpp/Crow)에서 Crow를 다운로드합니다.
   - Crow는 단일 헤더 파일 라이브러리로, `crow_all.h` 파일을 프로젝트에 포함하면 됩니다.

##### 기본 사용법

Crow를 사용하여 간단한 웹 서버 구현:

1. **코드 작성**

```cpp
#define CROW_MAIN
#include "crow_all.h"

int main() {
    crow::SimpleApp app;

    // 라우팅 설정
    CROW_ROUTE(app, "/")([]() {
        return "Hello, Crow!";
    });

    // JSON 데이터 처리
    CROW_ROUTE(app, "/json")
    ([] {
        crow::json::wvalue x;
        x["message"] = "Hello, Crow!";
        return x;
    });

    app.port(18080).multithreaded().run();
}
```

2. **CMake 설정**

```cmake
cmake_minimum_required(VERSION 3.10)
project(CrowExample)

set(CMAKE_CXX_STANDARD 11)

# Crow include 디렉토리 설정
include_directories(/path/to/crow)

# 코드 추가
add_executable(runExample main.cpp)

# Boost 라이브러리 링크
find_package(Boost REQUIRED COMPONENTS system)
target_link_libraries(runExample ${Boost_LIBRARIES})
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

1. **RESTful API 작성**:
   - 간단한 라우팅 설정으로 RESTful API를 쉽게 작성할 수 있습니다.
2. **JSON 데이터 처리**:
   - JSON 데이터를 손쉽게 생성하고 파싱할 수 있는 기능을 제공합니다.
3. **멀티스레드 지원**:
   - 멀티스레드 모드를 지원하여 고성능 웹 서버를 구축할 수 있습니다.
4. **미들웨어 지원**:
   - 요청 전후에 처리할 미들웨어를 설정할 수 있습니다.
5. **HTTP/HTTPS 지원**:
   - HTTP 및 HTTPS 프로토콜을 모두 지원합니다.

##### 예제 코드

간단한 웹 서버를 구축하고 JSON 데이터를 처리하는 Crow 코드입니다:

1. **코드 작성**

```cpp
#define CROW_MAIN
#include "crow_all.h"

int main() {
    crow::SimpleApp app;

    // 라우팅 설정
    CROW_ROUTE(app, "/")([]() {
        return "Hello, Crow!";
    });

    // JSON 데이터 처리
    CROW_ROUTE(app, "/json")
    ([] {
        crow::json::wvalue x;
        x["message"] = "Hello, Crow!";
        return x;
    });

    app.port(18080).multithreaded().run();
}
```

이 예제에서는 Crow를 사용하여 간단한 웹 서버를 구축하고, `/` 엔드포인트에서 문자열을 반환하고, `/json` 엔드포인트에서 JSON 데이터를 반환하는 프로그램을 구현합니다.
