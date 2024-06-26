#### 8.2. Intel Threading Building Blocks (TBB)

Intel Threading Building Blocks (TBB)는 멀티코어 프로세서에서 병렬 처리를 쉽게 구현할 수 있도록 도와주는 C++ 라이브러리입니다. TBB는 작업 스케줄링, 병렬 반복, 동시 컨테이너 등을 지원합니다.

##### TBB 설치 및 설정

1. **Windows, macOS, Linux**:
   - [TBB 다운로드 페이지](https://github.com/oneapi-src/oneTBB/releases)에서 TBB를 다운로드합니다.
   - CMake를 사용하여 TBB를 빌드하고 설치합니다:
     ```bash
     git clone https://github.com/oneapi-src/oneTBB.git
     cd oneTBB
     mkdir build
     cd build
     cmake ..
     make
     sudo make install
     ```

##### 기본 사용법

TBB를 사용하여 간단한 병렬 반복 구현:

1. **코드 작성**

```cpp
#include <tbb/tbb.h>
#include <iostream>
#include <vector>

void parallel_sum(const std::vector<int>& input, std::vector<int>& output) {
    tbb::parallel_for(
        tbb::blocked_range<size_t>(0, input.size()),
        [&input, &output](const tbb::blocked_range<size_t>& r) {
            for (size_t i = r.begin(); i < r.end(); ++i) {
                output[i] = input[i] * 2;
            }
        }
    );
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
project(TBBExample)

set(CMAKE_CXX_STANDARD 11)

# TBB 라이브러리 설정
find_package(TBB REQUIRED)
include_directories(${TBB_INCLUDE_DIRS})

# 코드 추가
add_executable(runExample main.cpp)
target_link_libraries(runExample ${TBB_LIBRARIES})
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

1. **작업 스케줄링**:
   - TBB는 작업을 스케줄링하고 효율적으로 분배하여 병렬 처리를 지원합니다.
2. **병렬 반복**:
   - `parallel_for`, `parallel_reduce` 등의 함수를 사용하여 병렬 반복을 쉽게 구현할 수 있습니다.
3. **동시 컨테이너**:
   - 동시 접근이 가능한 스레드 안전한 컨테이너를 제공합니다.
4. **파이프라인**:
   - 병렬 파이프라인을 구성하여 데이터 흐름을 관리할 수 있습니다.
5. **동기화 도구**:
   - 뮤텍스, 스핀락, 조건 변수 등의 동기화 도구를 제공합니다.

##### 예제 코드

간단한 병렬 반복을 수행하는 TBB 코드입니다:

1. **코드 작성**

```cpp
#include <tbb/tbb.h>
#include <iostream>
#include <vector>

void parallel_sum(const std::vector<int>& input, std::vector<int>& output) {
    tbb::parallel_for(
        tbb::blocked_range<size_t>(0, input.size()),
        [&input, &output](const tbb::blocked_range<size_t>& r) {
            for (size_t i = r.begin(); i < r.end(); ++i) {
                output[i] = input[i] * 2;
            }
        }
    );
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

이 예제에서는 TBB를 사용하여 입력 벡터의 각 요소를 2배로 만드는 병렬 반복 작업을 수행합니다. 결과는 출력 벡터에 저장되고, 첫 10개의 결과가 출력됩니다.
