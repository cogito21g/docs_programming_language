### 10. Protobuf

Protocol Buffers (Protobuf)는 Google에서 개발한 언어 중립적이고 플랫폼 중립적인 직렬화 라이브러리입니다. Protobuf는 데이터를 구조화하여 저장하거나 네트워크를 통해 전송할 때 사용하는 효율적이고 확장 가능한 방법을 제공합니다.

#### 10.1. Protobuf 개요 및 설치

##### Protobuf 개요

Protobuf는 다음과 같은 특징을 가지고 있습니다:
- 데이터 직렬화/역직렬화 기능 제공
- 다양한 언어 지원 (C++, Java, Python, etc.)
- 작은 크기와 높은 성능
- 강력한 타입 시스템

##### Protobuf 설치

**Linux 및 macOS**:

1. Protobuf 컴파일러와 라이브러리를 설치합니다:
   ```bash
   sudo apt-get install -y protobuf-compiler libprotobuf-dev
   ```

2. Protobuf C++ 라이브러리를 빌드하고 설치합니다:
   ```bash
   git clone https://github.com/protocolbuffers/protobuf.git
   cd protobuf
   git submodule update --init --recursive
   ./autogen.sh
   ./configure
   make
   sudo make install
   ```

**Windows**:

1. [Protobuf GitHub 페이지](https://github.com/protocolbuffers/protobuf/releases)에서 Windows용 precompiled binaries를 다운로드하고 설치합니다.

#### 10.2. 기본 사용법

Protobuf의 기본 사용법을 설명하기 위해 데이터를 정의하고 이를 직렬화/역직렬화하는 예제를 살펴보겠습니다.

**프로젝트 구조**:
```
ProtobufExample/
├── CMakeLists.txt
├── main.cpp
└── addressbook.proto
```

**addressbook.proto**:
```proto
syntax = "proto3";

package tutorial;

message Person {
  int32 id = 1;
  string name = 2;
  string email = 3;

  message PhoneNumber {
    string number = 1;
    enum PhoneType {
      MOBILE = 0;
      HOME = 1;
      WORK = 2;
    }
    PhoneType type = 2;
  }

  repeated PhoneNumber phones = 4;
}

message AddressBook {
  repeated Person people = 1;
}
```

**CMakeLists.txt**:
```cmake
cmake_minimum_required(VERSION 3.10)

project(ProtobufExample)

# C++ 표준 설정
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# Protobuf 라이브러리 찾기
find_package(Protobuf REQUIRED)
include_directories(${Protobuf_INCLUDE_DIRS})

# Protobuf 컴파일
protobuf_generate_cpp(PROTO_SRCS PROTO_HDRS addressbook.proto)

# 실행 파일 추가
add_executable(main main.cpp ${PROTO_SRCS} ${PROTO_HDRS})
target_link_libraries(main ${Protobuf_LIBRARIES})
```

##### main.cpp

**main.cpp**:
```cpp
#include <iostream>
#include <fstream>
#include "addressbook.pb.h"

void SaveAddressBook(const tutorial::AddressBook& address_book, const std::string& filename) {
    std::ofstream output(filename, std::ios::binary);
    if (!address_book.SerializeToOstream(&output)) {
        std::cerr << "Failed to write address book." << std::endl;
    }
}

void LoadAddressBook(tutorial::AddressBook& address_book, const std::string& filename) {
    std::ifstream input(filename, std::ios::binary);
    if (!address_book.ParseFromIstream(&input)) {
        std::cerr << "Failed to read address book." << std::endl;
    }
}

int main(int argc, char* argv[]) {
    GOOGLE_PROTOBUF_VERIFY_VERSION;

    tutorial::AddressBook address_book;

    tutorial::Person* person = address_book.add_people();
    person->set_id(1);
    person->set_name("John Doe");
    person->set_email("john.doe@example.com");

    tutorial::Person::PhoneNumber* phone = person->add_phones();
    phone->set_number("123-456-7890");
    phone->set_type(tutorial::Person::HOME);

    SaveAddressBook(address_book, "addressbook.bin");

    tutorial::AddressBook loaded_address_book;
    LoadAddressBook(loaded_address_book, "addressbook.bin");

    for (const auto& person : loaded_address_book.people()) {
        std::cout << "Person ID: " << person.id() << std::endl;
        std::cout << "Name: " << person.name() << std::endl;
        std::cout << "Email: " << person.email() << std::endl;
        for (const auto& phone : person.phones()) {
            std::cout << "Phone: " << phone.number() << " (" << phone.type() << ")" << std::endl;
        }
    }

    google::protobuf::ShutdownProtobufLibrary();

    return 0;
}
```

이 예제에서는 `addressbook.proto` 파일을 정의하여 `Person`과 `AddressBook` 메시지를 생성합니다. `main.cpp` 파일에서는 `AddressBook` 객체를 생성하고, 데이터를 추가한 후 이를 파일에 직렬화하여 저장합니다. 그런 다음, 파일에서 데이터를 역직렬화하여 로드하고, 데이터를 출력합니다.

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
