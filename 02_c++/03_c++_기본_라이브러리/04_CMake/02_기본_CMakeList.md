#### 4.2. 기본 CMakeLists.txt 작성

CMakeLists.txt 파일은 CMake 빌드 시스템의 중심 파일로, 프로젝트의 빌드 설정과 관련된 정보를 포함합니다. 이 섹션에서는 기본적인 CMakeLists.txt 파일을 작성하는 방법을 알아보겠습니다.

##### 예제 1: 간단한 CMake 프로젝트

다음은 간단한 C++ 프로젝트를 위한 CMakeLists.txt 파일의 예제입니다.

**프로젝트 구조**:
```
MySimpleProject/
├── CMakeLists.txt
└── main.cpp
```

**CMakeLists.txt**:
```cmake
cmake_minimum_required(VERSION 3.10)

# 프로젝트 이름 설정
project(MySimpleProject)

# C++ 표준 설정
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# 실행 파일 추가
add_executable(MySimpleProject main.cpp)
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
   ./MySimpleProject
   ```

##### 예제 2: 라이브러리 포함

다음은 외부 라이브러리를 포함하여 사용하는 CMakeLists.txt 파일의 예제입니다.

**프로젝트 구조**:
```
MyLibraryProject/
├── CMakeLists.txt
├── main.cpp
└── math
    ├── add.cpp
    └── add.h
```

**CMakeLists.txt**:
```cmake
cmake_minimum_required(VERSION 3.10)

# 프로젝트 이름 설정
project(MyLibraryProject)

# C++ 표준 설정
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# 서브 디렉토리 추가
add_subdirectory(math)

# 실행 파일 추가
add_executable(MyLibraryProject main.cpp)

# 타겟 링크 라이브러리 설정
target_link_libraries(MyLibraryProject PRIVATE MathLibrary)
```

**math/CMakeLists.txt**:
```cmake
cmake_minimum_required(VERSION 3.10)

# 라이브러리 추가
add_library(MathLibrary STATIC add.cpp)
```

**main.cpp**:
```cpp
#include <iostream>
#include "add.h"

int main() {
    std::cout << "3 + 4 = " << add(3, 4) << std::endl;
    return 0;
}
```

**math/add.h**:
```cpp
#ifndef ADD_H
#define ADD_H

int add(int a, int b);

#endif // ADD_H
```

**math/add.cpp**:
```cpp
#include "add.h"

int add(int a, int b) {
    return a + b;
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
   ./MyLibraryProject
   ```

이 예제에서는 `add` 함수를 정의한 라이브러리를 포함하여 사용하는 방법을 설명합니다. `add_subdirectory` 명령을 사용하여 서브 디렉터리의 CMakeLists.txt 파일을 포함하고, `target_link_libraries` 명령을 사용하여 라이브러리를 링크합니다.
