### 6. 수치 해석 및 과학 계산 라이브러리 (Numerical and Scientific Libraries)

#### 6.1. Eigen

Eigen은 C++에서 행렬 및 벡터 연산, 선형 대수, 수치 해석 등을 수행하기 위한 고성능 라이브러리입니다. Eigen은 템플릿 라이브러리로, 다양한 수치 연산을 최적화하여 제공합니다.

##### Eigen 설치 및 설정

1. **Windows, macOS, Linux**:
   - [Eigen 다운로드 페이지](https://eigen.tuxfamily.org/dox/GettingStarted.html)에서 Eigen을 다운로드합니다.
   - 압축을 풀고 프로젝트에서 Eigen의 `Eigen` 디렉토리를 include 경로에 추가합니다.

##### 기본 사용법

Eigen을 사용하여 간단한 행렬 연산:

1. **코드 작성**

```cpp
#include <Eigen/Dense>
#include <iostream>

int main() {
    // 2x2 행렬 초기화
    Eigen::Matrix2d mat;
    mat(0, 0) = 3;
    mat(1, 0) = 2.5;
    mat(0, 1) = -1;
    mat(1, 1) = mat(1, 0) + mat(0, 1);

    // 행렬 출력
    std::cout << "Here is the matrix mat:\n" << mat << std::endl;

    // 행렬 연산
    Eigen::Matrix2d mat2;
    mat2 << 1, 2,
            3, 4;
    Eigen::Matrix2d result = mat + mat2;
    std::cout << "Here is the sum of mat and mat2:\n" << result << std::endl;

    return 0;
}
```

2. **CMake 설정**

```cmake
cmake_minimum_required(VERSION 3.10)
project(EigenExample)

set(CMAKE_CXX_STANDARD 11)

# Eigen include 디렉토리 설정
include_directories(/path/to/eigen)

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

1. **행렬 및 벡터 연산**:
   - 다양한 크기의 행렬과 벡터를 정의하고 연산할 수 있습니다.
2. **선형 대수**:
   - 역행렬, 고유값 분해, LU 분해, QR 분해 등 선형 대수 연산을 지원합니다.
3. **수치 해석**:
   - 선형 방정식 풀이, 최적화, 미분 방정식 등 다양한 수치 해석 기능을 제공합니다.
4. **고성능 최적화**:
   - 템플릿 라이브러리로, 다양한 최적화 기법을 통해 성능을 극대화합니다.
5. **확장성**:
   - 다양한 수치 연산 라이브러리와 통합하여 사용할 수 있습니다.

##### 예제 코드

간단한 행렬 연산을 수행하는 Eigen 코드입니다:

1. **코드 작성**

```cpp
#include <Eigen/Dense>
#include <iostream>

int main() {
    // 2x2 행렬 초기화
    Eigen::Matrix2d mat;
    mat(0, 0) = 3;
    mat(1, 0) = 2.5;
    mat(0, 1) = -1;
    mat(1, 1) = mat(1, 0) + mat(0, 1);

    // 행렬 출력
    std::cout << "Here is the matrix mat:\n" << mat << std::endl;

    // 행렬 연산
    Eigen::Matrix2d mat2;
    mat2 << 1, 2,
            3, 4;
    Eigen::Matrix2d result = mat + mat2;
    std::cout << "Here is the sum of mat and mat2:\n" << result << std::endl;

    return 0;
}
```

이 예제에서는 Eigen을 사용하여 행렬을 초기화하고, 행렬의 합을 계산하는 간단한 프로그램을 구현합니다.
