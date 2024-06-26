### 2. Boost 라이브러리 (Boost Libraries)

#### 2.1. Boost 설치 및 설정

Boost는 C++ 표준 라이브러리를 확장하는 다양한 라이브러리를 제공하는 프로젝트입니다. Boost 라이브러리는 대부분의 주요 C++ 프로젝트에서 사용되며, C++ 표준 라이브러리에 포함된 여러 기능의 원천이기도 합니다.

##### 설치

1. **Windows**:
   - [Boost 다운로드 페이지](https://www.boost.org/users/download/)에서 Boost 라이브러리를 다운로드합니다.
   - 압축을 풀고 Boost 디렉토리로 이동합니다.
   - `bootstrap.bat` 파일을 실행하여 Boost 빌드 시스템을 설정합니다.
   - `b2` 명령을 실행하여 필요한 라이브러리를 빌드합니다:
     ```bash
     .\b2 install
     ```

2. **macOS**:
   - Homebrew를 사용하여 Boost를 설치합니다:
     ```bash
     brew install boost
     ```

3. **Linux**:
   - 패키지 관리자를 사용하여 Boost를 설치합니다:
     ```bash
     sudo apt-get install libboost-all-dev
     ```

##### 설정

Boost 라이브러리를 프로젝트에서 사용하려면, 프로젝트의 빌드 시스템에 Boost 라이브러리를 포함시켜야 합니다. 다음은 CMake를 사용하는 예제입니다:

**CMakeLists.txt 예제**:

```cmake
cmake_minimum_required(VERSION 3.10)
project(BoostExample)

set(CMAKE_CXX_STANDARD 11)

# Boost 라이브러리 설정
find_package(Boost REQUIRED COMPONENTS filesystem system)

include_directories(${Boost_INCLUDE_DIRS})
link_directories(${Boost_LIBRARY_DIRS})

add_executable(BoostExample main.cpp)
target_link_libraries(BoostExample ${Boost_LIBRARIES})
```

##### 간단한 예제

Boost를 사용한 간단한 예제 프로그램을 작성해보겠습니다. 이 예제에서는 Boost.Filesystem을 사용하여 디렉토리의 파일 목록을 출력합니다.

**main.cpp**:

```cpp
#include <iostream>
#include <boost/filesystem.hpp>

namespace fs = boost::filesystem;

int main() {
    fs::path someDir(".");

    if (fs::exists(someDir) && fs::is_directory(someDir)) {
        for (const auto& entry : fs::directory_iterator(someDir)) {
            std::cout << entry.path().string() << std::endl;
        }
    } else {
        std::cerr << "Directory does not exist." << std::endl;
    }

    return 0;
}
```

**빌드 및 실행**:

```bash
mkdir build
cd build
cmake ..
make
./BoostExample
```
