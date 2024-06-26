### 5. POCO C++ Libraries

#### 5.1. POCO 개요 및 설치

POCO(C++ Portable Components)는 네트워킹, 파일 시스템 접근, 스레드 관리 등 다양한 기능을 제공하는 크로스 플랫폼 C++ 라이브러리입니다. POCO는 네트워크 프로그래밍, 파일 및 디렉터리 조작, 동기화 및 스레드 관리 등을 포함한 여러 기능을 제공합니다.

##### POCO 설치

**Windows**:
1. [POCO GitHub 페이지](https://github.com/pocoproject/poco)에서 소스를 다운로드하거나 Git을 사용하여 클론합니다:
   ```bash
   git clone -b master https://github.com/pocoproject/poco.git
   ```
2. Visual Studio를 사용하여 빌드합니다:
   - `poco` 디렉터리로 이동합니다.
   - `buildwin` 디렉터리로 이동하여 솔루션 파일을 엽니다.
   - Visual Studio에서 솔루션을 빌드합니다.

**macOS 및 Linux**:
1. [POCO GitHub 페이지](https://github.com/pocoproject/poco)에서 소스를 다운로드하거나 Git을 사용하여 클론합니다:
   ```bash
   git clone -b master https://github.com/pocoproject/poco.git
   ```
2. CMake를 사용하여 빌드합니다:
   ```bash
   cd poco
   mkdir cmake-build
   cd cmake-build
   cmake ..
   make -j4
   sudo make install
   ```

##### 예제: 간단한 POCO 프로젝트 설정

다음은 POCO 라이브러리를 사용하여 간단한 프로젝트를 설정하는 예제입니다.

**프로젝트 구조**:
```
PocoExample/
├── CMakeLists.txt
└── main.cpp
```

**CMakeLists.txt**:
```cmake
cmake_minimum_required(VERSION 3.10)

# 프로젝트 이름 설정
project(PocoExample)

# C++ 표준 설정
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# POCO 라이브러리 찾기
find_package(Poco REQUIRED Foundation)

# 실행 파일 추가
add_executable(PocoExample main.cpp)

# POCO 라이브러리 링크
target_link_libraries(PocoExample Poco::Foundation)
```

**main.cpp**:
```cpp
#include <iostream>
#include <Poco/DateTime.h>
#include <Poco/DateTimeFormatter.h>
#include <Poco/DateTimeFormat.h>

int main() {
    Poco::DateTime now;
    std::string formattedDate = Poco::DateTimeFormatter::format(now, Poco::DateTimeFormat::ISO8601_FORMAT);
    std::cout << "Current date and time: " << formattedDate << std::endl;
    return 0;
}
```

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

3. 생성된 빌드 시스템을 사용하여 프로젝트를 빌드합니다:
   ```bash
   make
   ```

4. 빌드된 실행 파일을 실행합니다:
   ```bash
   ./PocoExample
   ```

이 예제에서는 POCO의 `Foundation` 라이브러리를 사용하여 현재 날짜와 시간을 ISO 8601 형식으로 출력합니다. `Poco::DateTime`, `Poco::DateTimeFormatter`, `Poco::DateTimeFormat` 클래스를 사용하여 날짜와 시간을 조작하고 형식화하는 방법을 보여줍니다.
