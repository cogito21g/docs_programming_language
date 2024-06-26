#### 4.5. 고급 CMake 기능 (변수, 조건문, 반복문 등)

CMake는 고급 빌드 설정을 지원하기 위해 변수, 조건문, 반복문 등을 제공합니다. 이를 통해 복잡한 빌드 설정을 구성하고 관리할 수 있습니다.

##### 변수 사용

CMake에서는 변수를 사용하여 값을 저장하고, 이를 다양한 빌드 설정에 사용할 수 있습니다. 변수를 정의하고 사용하는 방법은 다음과 같습니다:

```cmake
# 변수 정의
set(MY_VARIABLE "Hello, CMake!")

# 변수 사용
message(STATUS "Variable value: ${MY_VARIABLE}")
```

##### 조건문 사용

CMake에서는 `if` 명령을 사용하여 조건문을 작성할 수 있습니다. 조건문을 사용하여 빌드 설정을 조건부로 적용할 수 있습니다.

```cmake
# 조건문 사용
if(MY_VARIABLE STREQUAL "Hello, CMake!")
    message(STATUS "Condition met!")
else()
    message(STATUS "Condition not met!")
endif()
```

##### 반복문 사용

CMake에서는 `foreach` 명령을 사용하여 반복문을 작성할 수 있습니다. 반복문을 사용하여 여러 항목에 대해 반복 작업을 수행할 수 있습니다.

```cmake
# 리스트 정의
set(MY_LIST "item1" "item2" "item3")

# 반복문 사용
foreach(ITEM IN LISTS MY_LIST)
    message(STATUS "List item: ${ITEM}")
endforeach()
```

##### 고급 예제: 조건부 컴파일 및 반복문을 사용한 빌드 설정

다음 예제에서는 조건부 컴파일 및 반복문을 사용하여 여러 파일을 컴파일하고 링크하는 방법을 보여줍니다.

**프로젝트 구조**:
```
AdvancedCMake/
├── CMakeLists.txt
├── main.cpp
└── src
    ├── file1.cpp
    ├── file2.cpp
    └── file3.cpp
```

**CMakeLists.txt**:
```cmake
cmake_minimum_required(VERSION 3.10)

# 프로젝트 이름 설정
project(AdvancedCMake)

# C++ 표준 설정
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# 소스 파일 리스트 정의
set(SOURCE_FILES
    src/file1.cpp
    src/file2.cpp
    src/file3.cpp
)

# 조건부 컴파일 옵션
option(USE_CUSTOM_OPTION "Use custom option" ON)

if(USE_CUSTOM_OPTION)
    add_definitions(-DCUSTOM_OPTION)
    message(STATUS "Custom option enabled")
endif()

# 실행 파일 추가
add_executable(AdvancedCMake main.cpp)

# 소스 파일 추가
foreach(SOURCE IN LISTS SOURCE_FILES)
    target_sources(AdvancedCMake PRIVATE ${SOURCE})
endforeach()
```

**main.cpp**:
```cpp
#include <iostream>

void file1();
void file2();
void file3();

int main() {
    std::cout << "Hello, Advanced CMake!" << std::endl;
    file1();
    file2();
    file3();
    return 0;
}
```

**src/file1.cpp**:
```cpp
#include <iostream>

void file1() {
#ifdef CUSTOM_OPTION
    std::cout << "Custom option is enabled in file1" << std::endl;
#else
    std::cout << "Custom option is not enabled in file1" << std::endl;
#endif
}
```

**src/file2.cpp**:
```cpp
#include <iostream>

void file2() {
#ifdef CUSTOM_OPTION
    std::cout << "Custom option is enabled in file2" << std::endl;
#else
    std::cout << "Custom option is not enabled in file2" << std::endl;
#endif
}
```

**src/file3.cpp**:
```cpp
#include <iostream>

void file3() {
#ifdef CUSTOM_OPTION
    std::cout << "Custom option is enabled in file3" << std::endl;
#else
    std::cout << "Custom option is not enabled in file3" << std::endl;
#endif
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
   ./AdvancedCMake
   ```

이 예제에서는 `USE_CUSTOM_OPTION`을 사용하여 조건부 컴파일을 설정하고, `foreach` 반복문을 사용하여 여러 소스 파일을 추가합니다. 조건부 컴파일 옵션이 설정되면 `CUSTOM_OPTION` 매크로가 정의되어 각 소스 파일에서 해당 조건에 따라 다른 출력을 생성합니다.
