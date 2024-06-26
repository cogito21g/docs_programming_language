### 4. CMake

#### 4.1. CMake 개요 및 설치

CMake는 크로스 플랫폼 빌드 시스템으로, C++ 프로젝트를 구성하고 관리하는 데 사용됩니다. CMake는 프로젝트의 빌드 과정을 자동화하고, 플랫폼에 따라 적절한 빌드 설정을 생성하는 데 도움을 줍니다.

##### CMake 설치

**Windows**:
1. [CMake 공식 웹사이트](https://cmake.org/download/)에서 최신 버전을 다운로드합니다.
2. 설치 프로그램을 실행하고 지침에 따라 설치를 완료합니다.
3. 설치 후, 명령 프롬프트에서 `cmake --version` 명령을 실행하여 설치가 제대로 되었는지 확인합니다.

**macOS**:
1. Homebrew를 사용하여 CMake를 설치합니다:
   ```bash
   brew install cmake
   ```
2. 설치 후, 터미널에서 `cmake --version` 명령을 실행하여 설치가 제대로 되었는지 확인합니다.

**Linux**:
1. 패키지 관리자를 사용하여 CMake를 설치합니다:
   ```bash
   sudo apt-get install cmake
   ```
2. 설치 후, 터미널에서 `cmake --version` 명령을 실행하여 설치가 제대로 되었는지 확인합니다.

##### CMake 기본 개념

CMake는 프로젝트의 소스 코드와 빌드 설정을 설명하는 `CMakeLists.txt` 파일을 사용합니다. 이 파일에는 프로젝트 이름, 빌드 옵션, 의존성, 컴파일 옵션 등이 포함됩니다.

##### 예제: 간단한 CMake 프로젝트 설정

다음은 간단한 CMake 프로젝트를 설정하는 예제입니다.

**프로젝트 구조**:
```
MyProject/
├── CMakeLists.txt
└── main.cpp
```

**CMakeLists.txt**:
```cmake
cmake_minimum_required(VERSION 3.10)

# 프로젝트 이름 설정
project(MyProject)

# C++ 표준 설정
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# 실행 파일 추가
add_executable(MyProject main.cpp)
```

**main.cpp**:
```cpp
#include <iostream>

int main() {
    std::cout << "Hello, CMake!" << std::endl;
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
   ./MyProject
   ```

이 예제에서는 `CMakeLists.txt` 파일을 사용하여 간단한 C++ 프로젝트를 설정하고, CMake를 사용하여 프로젝트를 빌드하고 실행하는 과정을 설명합니다.
