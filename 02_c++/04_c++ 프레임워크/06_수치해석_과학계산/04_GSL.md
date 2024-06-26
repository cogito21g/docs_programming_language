#### 6.4. GSL (GNU Scientific Library)

GNU Scientific Library (GSL)은 다양한 수치 해석 알고리즘을 제공하는 C 및 C++ 라이브러리입니다. 선형 대수, 통계, 수치 적분, 미분 방정식 풀이 등을 포함한 다양한 기능을 제공합니다.

##### GSL 설치 및 설정

1. **Windows**:
   - Windows용 GSL 바이너리를 [여기](https://sourceforge.net/projects/gnuwin32/files/gsl/1.8/)에서 다운로드할 수 있습니다.
   - Visual Studio 프로젝트에서 GSL의 include 디렉토리와 lib 디렉토리를 설정합니다.
   - 필요한 라이브러리를 링크합니다: `gsl.lib`, `gslcblas.lib`.

2. **macOS**:
   - Homebrew를 사용하여 GSL을 설치합니다:
     ```bash
     brew install gsl
     ```

3. **Linux**:
   - 패키지 관리자를 사용하여 GSL을 설치합니다:
     ```bash
     sudo apt-get install libgsl-dev
     ```

##### 기본 사용법

GSL을 사용하여 간단한 벡터 연산:

1. **코드 작성**

```cpp
#include <gsl/gsl_vector.h>
#include <gsl/gsl_blas.h>
#include <iostream>

int main() {
    // 벡터 초기화
    gsl_vector* v = gsl_vector_alloc(3);
    gsl_vector_set(v, 0, 1.0);
    gsl_vector_set(v, 1, 2.0);
    gsl_vector_set(v, 2, 3.0);

    // 벡터 출력
    for (size_t i = 0; i < v->size; ++i) {
        std::cout << "v[" << i << "] = " << gsl_vector_get(v, i) << std::endl;
    }

    // 벡터 연산
    gsl_vector* w = gsl_vector_alloc(3);
    gsl_vector_memcpy(w, v);
    gsl_blas_dscal(2.0, w);

    // 벡터 출력
    std::cout << "After scaling by 2:" << std::endl;
    for (size_t i = 0; i < w->size; ++i) {
        std::cout << "w[" << i << "] = " << gsl_vector_get(w, i) << std::endl;
    }

    gsl_vector_free(v);
    gsl_vector_free(w);

    return 0;
}
```

2. **CMake 설정**

```cmake
cmake_minimum_required(VERSION 3.10)
project(GSLExample)

set(CMAKE_CXX_STANDARD 11)

# GSL 라이브러리 설정
find_package(GSL REQUIRED)
include_directories(${GSL_INCLUDE_DIRS})

# 코드 추가
add_executable(runExample main.cpp)
target_link_libraries(runExample ${GSL_LIBRARIES})
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

1. **선형 대수**:
   - 벡터, 행렬 연산 및 선형 대수 연산(역행렬, 고유값 분해 등)을 지원합니다.
2. **수치 해석**:
   - 적분, 미분, 미분 방정식 풀이 등의 수치 해석 알고리즘을 제공합니다.
3. **통계**:
   - 다양한 통계 함수와 확률 분포 함수(평균, 분산, 정규 분포, 베르누이 분포 등)를 제공합니다.
4. **최적화**:
   - 비선형 방정식 풀이, 함수 최적화 등의 알고리즘을 제공합니다.
5. **랜덤 수 생성**:
   - 다양한 랜덤 수 생성기와 관련 함수들을 제공합니다.

##### 예제 코드

간단한 벡터 연산을 수행하는 GSL 코드입니다:

1. **코드 작성**

```cpp
#include <gsl/gsl_vector.h>
#include <gsl/gsl_blas.h>
#include <iostream>

int main() {
    // 벡터 초기화
    gsl_vector* v = gsl_vector_alloc(3);
    gsl_vector_set(v, 0, 1.0);
    gsl_vector_set(v, 1, 2.0);
    gsl_vector_set(v, 2, 3.0);

    // 벡터 출력
    for (size_t i = 0; i < v->size; ++i) {
        std::cout << "v[" << i << "] = " << gsl_vector_get(v, i) << std::endl;
    }

    // 벡터 연산
    gsl_vector* w = gsl_vector_alloc(3);
    gsl_vector_memcpy(w, v);
    gsl_blas_dscal(2.0, w);

    // 벡터 출력
    std::cout << "After scaling by 2:" << std::endl;
    for (size_t i = 0; i < w->size; ++i) {
        std::cout << "w[" << i << "] = " << gsl_vector_get(w, i) << std::endl;
    }

    gsl_vector_free(v);
    gsl_vector_free(w);

    return 0;
}
```

이 예제에서는 GSL을 사용하여 벡터를 초기화하고, 벡터를 두 배로 스케일링하는 간단한 프로그램을 구현합니다.
