#### 8.4. PThreads (POSIX Threads)

PThreads는 POSIX 표준에 정의된 스레드 프로그래밍 API로, 멀티플랫폼 환경에서 멀티스레딩을 지원합니다. PThreads는 스레드 생성, 관리, 동기화 등의 기능을 제공합니다.

##### PThreads 설치 및 설정

PThreads는 대부분의 Unix-like 시스템에 기본적으로 포함되어 있습니다. Windows에서는 PThreads를 사용할 수 없으며, 대신 Windows 스레드 API를 사용해야 합니다.

##### 기본 사용법

PThreads를 사용하여 간단한 스레드 생성 및 동기화:

1. **코드 작성**

```cpp
#include <pthread.h>
#include <iostream>

void* print_hello(void* threadid) {
    long tid;
    tid = (long)threadid;
    std::cout << "Hello from thread " << tid << std::endl;
    pthread_exit(NULL);
}

int main() {
    pthread_t threads[5];
    int rc;
    int i;
    for (i = 0; i < 5; ++i) {
        std::cout << "Creating thread " << i << std::endl;
        rc = pthread_create(&threads[i], NULL, print_hello, (void*)i);
        if (rc) {
            std::cout << "Error:unable to create thread," << rc << std::endl;
            exit(-1);
        }
    }

    for (i = 0; i < 5; ++i) {
        pthread_join(threads[i], NULL);
    }

    pthread_exit(NULL);
}
```

2. **CMake 설정**

```cmake
cmake_minimum_required(VERSION 3.10)
project(PThreadsExample)

set(CMAKE_CXX_STANDARD 11)

# PThreads 라이브러리 설정
find_package(Threads REQUIRED)

# 코드 추가
add_executable(runExample main.cpp)
target_link_libraries(runExample ${CMAKE_THREAD_LIBS_INIT})
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

1. **스레드 생성 및 관리**:
   - 새로운 스레드를 생성하고 관리할 수 있습니다.
2. **동기화 도구**:
   - 뮤텍스, 조건 변수 등의 동기화 도구를 제공하여 스레드 간의 동기화를 지원합니다.
3. **스레드 종료**:
   - 스레드가 종료될 때까지 대기할 수 있습니다.
4. **스레드 로컬 저장소**:
   - 각 스레드마다 독립적인 저장소를 제공하여 스레드 로컬 데이터를 관리할 수 있습니다.
5. **스레드 속성 설정**:
   - 스레드의 우선순위, 스케줄링 정책 등 다양한 속성을 설정할 수 있습니다.

##### 예제 코드

간단한 스레드 생성 및 동기화를 수행하는 PThreads 코드입니다:

1. **코드 작성**

```cpp
#include <pthread.h>
#include <iostream>

void* print_hello(void* threadid) {
    long tid;
    tid = (long)threadid;
    std::cout << "Hello from thread " << tid << std::endl;
    pthread_exit(NULL);
}

int main() {
    pthread_t threads[5];
    int rc;
    int i;
    for (i = 0; i < 5; ++i) {
        std::cout << "Creating thread " << i << std::endl;
        rc = pthread_create(&threads[i], NULL, print_hello, (void*)i);
        if (rc) {
            std::cout << "Error:unable to create thread," << rc << std::endl;
            exit(-1);
        }
    }

    for (i = 0; i < 5; ++i) {
        pthread_join(threads[i], NULL);
    }

    pthread_exit(NULL);
}
```

이 예제에서는 PThreads를 사용하여 5개의 스레드를 생성하고, 각 스레드에서 "Hello from thread" 메시지를 출력합니다. 모든 스레드가 종료될 때까지 대기합니다.
