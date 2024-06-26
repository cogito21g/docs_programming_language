### 8. 멀티스레딩 및 동시성 라이브러리 (Multithreading and Concurrency Libraries)

#### 8.1. Boost.Thread

Boost.Thread는 C++ 표준 라이브러리의 스레드 기능을 확장한 고성능 스레딩 라이브러리입니다. Boost.Thread는 스레드 생성, 관리, 동기화 등의 기능을 제공합니다.

##### Boost.Thread 설치 및 설정

1. **Windows, macOS, Linux**:
   - Boost 라이브러리를 다운로드하고 설치합니다. Boost.Thread는 Boost 라이브러리의 일부이므로 별도의 설치가 필요 없습니다.
   - [Boost 다운로드 페이지](https://www.boost.org/users/download/)에서 Boost 라이브러리를 다운로드합니다.
   - 압축을 풀고, `bootstrap.bat` (Windows) 또는 `bootstrap.sh` (macOS, Linux)를 실행하여 Boost 라이브러리를 빌드합니다:
     ```bash
     ./bootstrap.sh
     ./b2
     ```

##### 기본 사용법

Boost.Thread를 사용하여 간단한 스레드 생성 및 동기화:

1. **코드 작성**

```cpp
#include <boost/thread.hpp>
#include <iostream>

void print_hello() {
    std::cout << "Hello from thread!" << std::endl;
}

int main() {
    // 스레드 생성
    boost::thread th(print_hello);

    // 메인 스레드에서 실행
    std::cout << "Hello from main!" << std::endl;

    // 스레드 종료 대기
    th.join();

    return 0;
}
```

2. **CMake 설정**

```cmake
cmake_minimum_required(VERSION 3.10)
project(BoostThreadExample)

set(CMAKE_CXX_STANDARD 11)

# Boost 라이브러리 설정
find_package(Boost REQUIRED COMPONENTS thread)
include_directories(${Boost_INCLUDE_DIRS})

# 코드 추가
add_executable(runExample main.cpp)
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

1. **스레드 생성 및 관리**:
   - 새로운 스레드를 생성하고 관리할 수 있습니다.
2. **동기화 도구**:
   - 뮤텍스, 조건 변수 등의 동기화 도구를 제공하여 스레드 간의 동기화를 지원합니다.
3. **락 관리**:
   - 다양한 락 관리 도구를 제공하여 뮤텍스를 안전하게 사용할 수 있습니다.
4. **스레드 그룹**:
   - 스레드 그룹을 사용하여 여러 스레드를 관리할 수 있습니다.
5. **스레드 로컬 저장소**:
   - 각 스레드마다 독립적인 저장소를 제공하여 스레드 로컬 데이터를 관리할 수 있습니다.

##### 예제 코드

간단한 스레드 생성 및 동기화를 수행하는 Boost.Thread 코드입니다:

1. **코드 작성**

```cpp
#include <boost/thread.hpp>
#include <iostream>

void print_hello() {
    std::cout << "Hello from thread!" << std::endl;
}

int main() {
    // 스레드 생성
    boost::thread th(print_hello);

    // 메인 스레드에서 실행
    std::cout << "Hello from main!" << std::endl;

    // 스레드 종료 대기
    th.join();

    return 0;
}
```

이 예제에서는 Boost.Thread를 사용하여 새로운 스레드를 생성하고, 스레드에서 "Hello from thread!" 메시지를 출력한 후, 메인 스레드에서 "Hello from main!" 메시지를 출력합니다. 마지막으로 스레드가 종료될 때까지 대기합니다.
