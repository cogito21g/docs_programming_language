#### 13.5. 동기화 및 IPC (semaphores, shared memory, message queues)

동기화 및 IPC(Inter-Process Communication)는 멀티프로세스 환경에서 필수적인 요소입니다. 이 섹션에서는 세마포어, 공유 메모리, 메시지 큐를 사용하여 프로세스 간의 통신 및 동기화를 구현하는 방법을 설명합니다.

##### 세마포어 (Semaphores)

세마포어는 프로세스 간의 동기화를 위해 사용되는 기법입니다. 세마포어는 두 가지 주요 작업을 제공합니다: `wait` (또는 `P` 연산)와 `signal` (또는 `V` 연산).

**프로젝트 구조**:
```
SemaphoreExample/
├── CMakeLists.txt
└── main.cpp
```

**CMakeLists.txt**:
```cmake
cmake_minimum_required(VERSION 3.10)

project(SemaphoreExample)

# C++ 표준 설정
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# 실행 파일 추가
add_executable(main main.cpp)
target_link_libraries(main pthread)
```

**main.cpp**:
```cpp
#include <iostream>
#include <pthread.h>
#include <semaphore.h>
#include <unistd.h>

sem_t semaphore;

void* ThreadFunction(void* arg) {
    sem_wait(&semaphore); // 세마포어를 기다림
    std::cout << "Thread " << pthread_self() << " is in critical section." << std::endl;
    sleep(1); // 임의의 작업 수행
    std::cout << "Thread " << pthread_self() << " is leaving critical section." << std::endl;
    sem_post(&semaphore); // 세마포어를 신호함
    return nullptr;
}

int main() {
    pthread_t threads[3];

    // 세마포어 초기화
    sem_init(&semaphore, 0, 1);

    // 스레드 생성
    for (int i = 0; i < 3; ++i) {
        pthread_create(&threads[i], nullptr, ThreadFunction, nullptr);
    }

    // 스레드 조인
    for (int i = 0; i < 3; ++i) {
        pthread_join(threads[i], nullptr);
    }

    // 세마포어 제거
    sem_destroy(&semaphore);

    return 0;
}
```

##### 공유 메모리 (Shared Memory)

공유 메모리는 여러 프로세스가 공통의 메모리 영역을 공유하여 통신하는 방법입니다.

**프로젝트 구조**:
```
SharedMemoryExample/
├── CMakeLists.txt
├── writer.cpp
└── reader.cpp
```

**CMakeLists.txt**:
```cmake
cmake_minimum_required(VERSION 3.10)

project(SharedMemoryExample)

# C++ 표준 설정
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# 실행 파일 추가
add_executable(writer writer.cpp)
add_executable(reader reader.cpp)
```

**writer.cpp**:
```cpp
#include <iostream>
#include <sys/ipc.h>
#include <sys/shm.h>
#include <cstring>
#include <unistd.h>

int main() {
    // 키 생성
    key_t key = ftok("shmfile", 65);

    // 공유 메모리 식별자 생성
    int shmid = shmget(key, 1024, 0666 | IPC_CREAT);

    // 공유 메모리에 연결
    char* str = (char*)shmat(shmid, nullptr, 0);

    // 데이터 쓰기
    std::strcpy(str, "Hello, Shared Memory!");

    std::cout << "Data written in memory: " << str << std::endl;

    // 공유 메모리 분리
    shmdt(str);

    return 0;
}
```

**reader.cpp**:
```cpp
#include <iostream>
#include <sys/ipc.h>
#include <sys/shm.h>
#include <unistd.h>

int main() {
    // 키 생성
    key_t key = ftok("shmfile", 65);

    // 공유 메모리 식별자 생성
    int shmid = shmget(key, 1024, 0666 | IPC_CREAT);

    // 공유 메모리에 연결
    char* str = (char*)shmat(shmid, nullptr, 0);

    // 데이터 읽기
    std::cout << "Data read from memory: " << str << std::endl;

    // 공유 메모리 분리
    shmdt(str);

    // 공유 메모리 제거
    shmctl(shmid, IPC_RMID, nullptr);

    return 0;
}
```

##### 메시지 큐 (Message Queues)

메시지 큐는 프로세스 간에 메시지를 교환하기 위한 방법입니다.

**프로젝트 구조**:
```
MessageQueueExample/
├── CMakeLists.txt
├── sender.cpp
└── receiver.cpp
```

**CMakeLists.txt**:
```cmake
cmake_minimum_required(VERSION 3.10)

project(MessageQueueExample)

# C++ 표준 설정
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# 실행 파일 추가
add_executable(sender sender.cpp)
add_executable(receiver receiver.cpp)
```

**sender.cpp**:
```cpp
#include <iostream>
#include <sys/ipc.h>
#include <sys/msg.h>
#include <cstring>

struct message_buffer {
    long message_type;
    char message_text[100];
};

int main() {
    // 키 생성
    key_t key = ftok("msgfile", 65);

    // 메시지 큐 식별자 생성
    int msgid = msgget(key, 0666 | IPC_CREAT);
    message_buffer message;

    // 메시지 작성
    message.message_type = 1;
    std::strcpy(message.message_text, "Hello, Message Queue!");

    // 메시지 보내기
    msgsnd(msgid, &message, sizeof(message), 0);

    std::cout << "Data sent: " << message.message_text << std::endl;

    return 0;
}
```

**receiver.cpp**:
```cpp
#include <iostream>
#include <sys/ipc.h>
#include <sys/msg.h>

struct message_buffer {
    long message_type;
    char message_text[100];
};

int main() {
    // 키 생성
    key_t key = ftok("msgfile", 65);

    // 메시지 큐 식별자 생성
    int msgid = msgget(key, 0666 | IPC_CREAT);
    message_buffer message;

    // 메시지 받기
    msgrcv(msgid, &message, sizeof(message), 1, 0);

    // 데이터 출력
    std::cout << "Data received: " << message.message_text << std::endl;

    // 메시지 큐 제거
    msgctl(msgid, IPC_RMID, nullptr);

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

3. 세마포어 예제를 빌드합니다:
   ```bash
   make
   ```

4. 세마포어 예제를 실행합니다:
   ```bash
   ./main
   ```

5. 공유 메모리 예제를 빌드합니다:
   ```bash
   make writer reader
   ```

6. 공유 메모리 예제를 실행합니다:
   ```bash
   ./writer
   ./reader
   ```

7. 메시지 큐 예제를 빌드합니다:
   ```bash
   make sender receiver
   ```

8. 메시지 큐 예제를 실행합니다:
   ```bash
   ./sender
   ./receiver
   ```
