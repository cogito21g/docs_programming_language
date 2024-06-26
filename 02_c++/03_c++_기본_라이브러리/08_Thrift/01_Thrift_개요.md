#### 8. Thrift

Apache Thrift는 다양한 프로그래밍 언어 간의 원활한 통신을 위해 설계된 RPC 프레임워크입니다. Thrift는 인터페이스 정의 언어(IDL)를 사용하여 데이터 타입과 서비스 인터페이스를 정의하고, 이를 기반으로 여러 언어에서 사용할 수 있는 코드를 자동으로 생성합니다.

#### 8.1. Thrift 개요 및 설치

##### Thrift 개요

Thrift는 다음과 같은 특징을 가지고 있습니다:
- 다양한 언어 지원: C++, Java, Python, Ruby, PHP, JavaScript 등
- 효율적인 바이너리 프로토콜을 사용하여 고성능을 제공
- 다양한 전송 계층 및 프로토콜 지원

##### Thrift 설치

**Linux 및 macOS**:

1. Thrift를 설치하기 위해 필요한 도구를 설치합니다:
   ```bash
   sudo apt-get install automake bison flex g++ libboost-all-dev libevent-dev libssl-dev pkg-config
   ```

2. Thrift 소스 코드를 다운로드하고 빌드합니다:
   ```bash
   git clone https://github.com/apache/thrift.git
   cd thrift
   ./bootstrap.sh
   ./configure
   make
   sudo make install
   ```

**Windows**:
- Windows에서 Thrift를 설치하려면 [Thrift Windows 설치 가이드](https://thrift.apache.org/docs/Windows.html)를 참고하십시오.

##### Thrift 프로젝트 설정

프로젝트에 Thrift를 설정하려면 `CMakeLists.txt` 파일에 다음과 같은 내용을 추가합니다.

**프로젝트 구조**:
```
ThriftExample/
├── CMakeLists.txt
├── server.cpp
├── client.cpp
├── example.thrift
```

**CMakeLists.txt**:
```cmake
cmake_minimum_required(VERSION 3.10)

project(ThriftExample)

# C++ 표준 설정
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# Thrift 경로 설정
find_package(Thrift REQUIRED)

# Thrift 컴파일
set(THRIFT_OUTPUT_DIR ${CMAKE_CURRENT_BINARY_DIR})
add_custom_command(
    OUTPUT ${THRIFT_OUTPUT_DIR}/example_constants.h ${THRIFT_OUTPUT_DIR}/example_constants.cpp
           ${THRIFT_OUTPUT_DIR}/example_types.h ${THRIFT_OUTPUT_DIR}/example_types.cpp
           ${THRIFT_OUTPUT_DIR}/ExampleService.h ${THRIFT_OUTPUT_DIR}/ExampleService.cpp
    COMMAND thrift --gen cpp -out ${THRIFT_OUTPUT_DIR} ${CMAKE_CURRENT_SOURCE_DIR}/example.thrift
    DEPENDS ${CMAKE_CURRENT_SOURCE_DIR}/example.thrift)

# 실행 파일 추가
add_executable(server server.cpp ${THRIFT_OUTPUT_DIR}/example_constants.cpp ${THRIFT_OUTPUT_DIR}/example_types.cpp ${THRIFT_OUTPUT_DIR}/ExampleService.cpp)
add_executable(client client.cpp ${THRIFT_OUTPUT_DIR}/example_constants.cpp ${THRIFT_OUTPUT_DIR}/example_types.cpp ${THRIFT_OUTPUT_DIR}/ExampleService.cpp)

# Thrift 라이브러리 링크
target_link_libraries(server Thrift::thrift)
target_link_libraries(client Thrift::thrift)
```

**example.thrift**:
```thrift
namespace cpp example

service ExampleService {
    string sayHello(1:string name)
}
```

이 예제에서는 `ExampleService`라는 서비스를 정의하며, `sayHello` 메서드를 가집니다. `sayHello` 메서드는 이름을 받아 인사말을 반환합니다.

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
