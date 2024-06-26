### 13. 운영체제 및 시스템 프로그래밍

운영체제 및 시스템 프로그래밍은 운영체제의 기능을 활용하여 애플리케이션을 개발하는 것을 의미합니다. 여기에는 파일 시스템 접근, 프로세스 관리, 메모리 관리, 동기화 및 IPC, 시스템 호출 등의 주제가 포함됩니다.

#### 13.1. POSIX 표준

POSIX(Portable Operating System Interface)은 이식성이 높은 운영체제 인터페이스를 정의하는 표준입니다. 이 섹션에서는 POSIX 표준의 기본 개념과 시스템 프로그래밍에서의 활용 방법을 다룹니다.

##### POSIX 개요

POSIX 표준은 다음과 같은 기능을 정의합니다:
- 파일 및 디렉터리 조작
- 프로세스 관리
- 스레드 관리
- 동기화
- IPC(Inter-Process Communication)
- 신호 처리

POSIX는 주로 Unix 계열 운영체제에서 사용되며, Windows에서도 일부 POSIX 기능을 지원합니다.

##### POSIX 헤더 파일

POSIX 표준은 여러 헤더 파일에 의해 정의됩니다. 주요 헤더 파일은 다음과 같습니다:
- `<unistd.h>`: 기본 시스템 호출 및 기능
- `<fcntl.h>`: 파일 제어 옵션
- `<sys/types.h>`: 데이터 타입 정의
- `<sys/stat.h>`: 파일 상태 정보
- `<dirent.h>`: 디렉터리 조작
- `<pthread.h>`: 스레드 관리
- `<semaphore.h>`: 세마포어
- `<sys/ipc.h>`: IPC 키
- `<sys/msg.h>`: 메시지 큐
- `<sys/shm.h>`: 공유 메모리
- `<sys/sem.h>`: 세마포어
- `<signal.h>`: 신호 처리

##### 예제: 파일 읽기 및 쓰기

다음 예제는 POSIX 표준을 사용하여 파일을 읽고 쓰는 방법을 보여줍니다.

**프로젝트 구조**:
```
POSIXExample/
├── CMakeLists.txt
└── main.cpp
```

**CMakeLists.txt**:
```cmake
cmake_minimum_required(VERSION 3.10)

project(POSIXExample)

# C++ 표준 설정
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# 실행 파일 추가
add_executable(main main.cpp)
```

**main.cpp**:
```cpp
#include <iostream>
#include <unistd.h>
#include <fcntl.h>
#include <sys/types.h>
#include <sys/stat.h>

void WriteToFile(const char* filename, const char* data) {
    int fd = open(filename, O_WRONLY | O_CREAT | O_TRUNC, S_IRUSR | S_IWUSR);
    if (fd == -1) {
        perror("Failed to open file");
        return;
    }

    ssize_t bytes_written = write(fd, data, strlen(data));
    if (bytes_written == -1) {
        perror("Failed to write to file");
    }

    close(fd);
}

void ReadFromFile(const char* filename) {
    int fd = open(filename, O_RDONLY);
    if (fd == -1) {
        perror("Failed to open file");
        return;
    }

    char buffer[1024];
    ssize_t bytes_read = read(fd, buffer, sizeof(buffer) - 1);
    if (bytes_read == -1) {
        perror("Failed to read from file");
    } else {
        buffer[bytes_read] = '\0';
        std::cout << "File content: " << buffer << std::endl;
    }

    close(fd);
}

int main() {
    const char* filename = "example.txt";
    const char* data = "Hello, POSIX!";

    WriteToFile(filename, data);
    ReadFromFile(filename);

    return 0;
}
```

이 예제에서는 `open`, `write`, `read`, `close` 함수와 같은 POSIX 시스템 호출을 사용하여 파일을 읽고 씁니다. `open` 함수는 파일을 열고, `write` 함수는 데이터를 파일에 씁니다. `read` 함수는 파일에서 데이터를 읽고, `close` 함수는 파일을 닫습니다.

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

3. 프로젝트를 빌드합니다:
   ```bash
   make
   ```

4. 빌드된 실행 파일을 실행합니다:
   ```bash
   ./main
   ```
