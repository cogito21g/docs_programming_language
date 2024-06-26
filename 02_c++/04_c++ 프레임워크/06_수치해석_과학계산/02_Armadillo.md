#### 6.2. Armadillo

Armadillo는 C++에서 선형 대수와 수치 해석을 위한 고성능 라이브러리입니다. Matlab과 유사한 문법을 사용하여 코드 작성이 용이하며, 내부적으로 최적화된 블라스(BLAS)와 LAPACK 라이브러리를 활용하여 성능을 극대화합니다.

##### Armadillo 설치 및 설정

1. **Windows**:
   - [Armadillo 다운로드 페이지](http://arma.sourceforge.net/download.html)에서 Armadillo를 다운로드합니다.
   - Visual Studio 프로젝트에서 Armadillo의 include 디렉토리와 lib 디렉토리를 설정합니다.
   - 필요한 라이브러리를 링크합니다: `libarmadillo.lib`.

2. **macOS**:
   - Homebrew를 사용하여 Armadillo를 설치합니다:
     ```bash
     brew install armadillo
     ```

3. **Linux**:
   - 패키지 관리자를 사용하여 Armadillo를 설치합니다:
     ```bash
     sudo apt-get install libarmadillo-dev
     ```

##### 기본 사용법

Armadillo를 사용하여 간단한 행렬 연산:

1. **코드 작성**

```cpp
#include <armadillo>
#include <iostream>

int main() {
    // 2x2 행렬 초기화
    arma::mat A(2, 2);
    A(0, 0) = 3.0; A(0, 1) = 1.5;
    A(1, 0) = 1.0; A(1, 1) = 2.0;

    // 행렬 출력
    std::cout << "Matrix A:\n" << A << std::endl;

    // 행렬 연산
    arma::mat B = A + A;
    std::cout << "Matrix B (A + A):\n" << B << std::endl;

    return 0;
}
```

2. **CMake 설정**

```cmake
cmake_minimum_required(VERSION 3.10)
project(ArmadilloExample)

set(CMAKE_CXX_STANDARD 11)

# Armadillo include 디렉토리 설정
find_package(Armadillo REQUIRED)
include_directories(${ARMADILLO_INCLUDE_DIRS})

# 코드 추가
add_executable(runExample main.cpp)
target_link_libraries(runExample ${ARMADILLO_LIBRARIES})
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

1. **행렬 및 벡터 연산**:
   - 다양한 크기의 행렬과 벡터를 정의하고 연산할 수 있습니다.
2. **선형 대수**:
   - 역행렬, 고유값 분해, LU 분해, QR 분해 등 선형 대수 연산을 지원합니다.
3. **수치 해석**:
   - 선형 방정식 풀이, 최적화, 미분 방정식 등 다양한 수치 해석 기능을 제공합니다.
4. **고성능 최적화**:
   - BLAS와 LAPACK 라이브러리를 활용하여 최적화된 연산을 수행합니다.
5. **Matlab 유사 문법**:
   - Matlab과 유사한 문법을 사용하여 코드 작성이 용이합니다.

##### 예제 코드

간단한 행렬 연산을 수행하는 Armadillo 코드입니다:

1. **코드 작성**

```cpp
#include <armadillo>
#include <iostream>

int main() {
    // 2x2 행렬 초기화
    arma::mat A(2, 2);
    A(0, 0) = 3.0; A(0, 1) = 1.5;
    A(1, 0) = 1.0; A(1, 1) = 2.0;

    // 행렬 출력
    std::cout << "Matrix A:\n" << A << std::endl;

    // 행렬 연산
    arma::mat B = A + A;
    std::cout << "Matrix B (A + A):\n" << B << std::endl;

    return 0;
}
```

이 예제에서는 Armadillo를 사용하여 행렬을 초기화하고, 행렬의 합을 계산하는 간단한 프로그램을 구현합니다.
