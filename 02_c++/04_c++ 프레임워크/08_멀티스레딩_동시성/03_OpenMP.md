#### 8.3. OpenMP

OpenMP는 C, C++, Fortran 언어에서 병렬 프로그래밍을 지원하기 위한 API로, 멀티코어 프로세서에서의 병렬 처리를 쉽게 구현할 수 있도록 도와줍니다. OpenMP는 컴파일러 디렉티브, 라이브러리 루틴, 환경 변수 등을 통해 병렬 처리를 제어합니다.

##### OpenMP 설치 및 설정

1. **Windows**:
   - Visual Studio에서 OpenMP 지원을 활성화합니다. 프로젝트 속성 -> C/C++ -> 모든 컴파일러 옵션 -> OpenMP 지원을 활성화합니다.

2. **macOS**:
   - Apple Clang 컴파일러는 기본적으로 OpenMP를 지원하지 않습니다. 대신, Homebrew를 사용하여 GCC를 설치합니다:
     ```bash
     brew install gcc
     ```

3. **Linux**:
   - 대부분의 GCC 컴파일러는 기본적으로 OpenMP를 지원합니다. GCC가 설치되어 있는지 확인합니다:
     ```bash
     gcc --version
     ```

##### 기본 사용법

OpenMP를 사용하여 간단한 병렬 반복 구현:

1. **코드 작성**

```cpp
#include <omp.h>
#include <iostream>
#include <vector>

void parallel_sum(const std::vector<int>& input, std::vector<int>& output) {
    #pragma omp parallel for
    for (size_t i = 0; i < input.size(); ++i) {
        output[i] = input[i] * 2;
    }
}

int main() {
    std::vector<int> input(1000, 1);
    std::vector<int> output(1000, 0);

    parallel_sum(input, output);

    for (size_t i = 0; i < 10; ++i) {
        std::cout << "output[" << i << "] = " << output[i] << std::endl;
    }

    return 0;
}
```

2. **CMake 설정**

```cmake
cmake_minimum_required(VERSION 3.10)
project(OpenMPExample)

set(CMAKE_CXX_STANDARD 11)

# OpenMP 라이브러리 설정
find_package(OpenMP REQUIRED)
if (OpenMP_CXX_FOUND)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}")
endif()

# 코드 추가
add_executable(runExample main.cpp)
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

1. **병렬 반복**:
   - `#pragma omp parallel for` 디렉티브를 사용하여 반복 작업을 병렬로 수행할 수 있습니다.
2. **작업 공유**:
   - OpenMP는 작업을 스레드 간에 자동으로 분할하여 병렬 처리를 지원합니다.
3. **동기화 도구**:
   - 뮤텍스, 조건 변수 등의 동기화 도구를 제공합니다.
4. **리덕션 연산**:
   - 병렬 반복 내에서 누적 계산을 수행할 수 있는 리덕션 연산을 지원합니다.
5. **동적 조정**:
   - 스레드 수를 동적으로 조정하여 최적의 성능을 도출합니다.

##### 예제 코드

간단한 병렬 반복을 수행하는 OpenMP 코드입니다:

1. **코드 작성**

```cpp
#include <omp.h>
#include <iostream>
#include <vector>

void parallel_sum(const std::vector<int>& input, std::vector<int>& output) {
    #pragma omp parallel for
    for (size_t i = 0; i < input.size(); ++i) {
        output[i] = input[i] * 2;
    }
}

int main() {
    std::vector<int> input(1000, 1);
    std::vector<int> output(1000, 0);

    parallel_sum(input, output);

    for (size_t i = 0; i < 10; ++i) {
        std::cout << "output[" << i << "] = " << output[i] << std::endl;
    }

    return 0;
}
```

이 예제에서는 OpenMP를 사용하여 입력 벡터의 각 요소를 2배로 만드는 병렬 반복 작업을 수행합니다. 결과는 출력 벡터에 저장되고, 첫 10개의 결과가 출력됩니다.
