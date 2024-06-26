#### 11. OpenSSL

OpenSSL은 보안 통신을 위한 강력한 툴킷입니다. 이 라이브러리는 SSL/TLS 프로토콜을 구현하고 암호화 및 인증서를 관리하는 기능을 제공합니다.

#### 11.1. OpenSSL 개요 및 설치

##### OpenSSL 개요

OpenSSL은 다음과 같은 기능을 제공합니다:
- SSL/TLS 프로토콜 구현
- 암호화/복호화
- 디지털 인증서 생성 및 관리
- 다양한 암호 알고리즘 지원

##### OpenSSL 설치

**Linux**:

1. OpenSSL 라이브러리 설치:
   ```bash
   sudo apt-get install libssl-dev
   ```

2. OpenSSL 툴킷 설치:
   ```bash
   sudo apt-get install openssl
   ```

**macOS**:

1. Homebrew를 사용하여 OpenSSL 설치:
   ```bash
   brew install openssl
   ```

**Windows**:

1. [OpenSSL 다운로드 페이지](https://slproweb.com/products/Win32OpenSSL.html)에서 Windows용 설치 파일을 다운로드하고 설치합니다.

##### 프로젝트 설정

프로젝트에서 OpenSSL을 사용하려면 CMake 파일에 OpenSSL 라이브러리를 링크해야 합니다.

**프로젝트 구조**:
```
OpenSSLExample/
├── CMakeLists.txt
├── main.cpp
```

**CMakeLists.txt**:
```cmake
cmake_minimum_required(VERSION 3.10)

project(OpenSSLExample)

# C++ 표준 설정
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# OpenSSL 라이브러리 찾기
find_package(OpenSSL REQUIRED)
include_directories(${OPENSSL_INCLUDE_DIR})

# 실행 파일 추가
add_executable(main main.cpp)
target_link_libraries(main OpenSSL::SSL OpenSSL::Crypto)
```

##### 기본 OpenSSL 예제

**main.cpp**:
```cpp
#include <openssl/ssl.h>
#include <openssl/err.h>
#include <openssl/rand.h>
#include <iostream>

void InitializeOpenSSL() {
    SSL_load_error_strings();
    OpenSSL_add_ssl_algorithms();
}

void CleanupOpenSSL() {
    EVP_cleanup();
}

int main() {
    InitializeOpenSSL();

    const SSL_METHOD* method = SSLv23_client_method();
    SSL_CTX* ctx = SSL_CTX_new(method);

    if (!ctx) {
        std::cerr << "Unable to create SSL context" << std::endl;
        ERR_print_errors_fp(stderr);
        return EXIT_FAILURE;
    }

    SSL* ssl = SSL_new(ctx);
    if (!ssl) {
        std::cerr << "Unable to create SSL structure" << std::endl;
        ERR_print_errors_fp(stderr);
        SSL_CTX_free(ctx);
        return EXIT_FAILURE;
    }

    SSL_CTX_free(ctx);
    CleanupOpenSSL();

    std::cout << "OpenSSL initialized and cleaned up successfully." << std::endl;

    return EXIT_SUCCESS;
}
```

이 예제에서는 OpenSSL 라이브러리를 초기화하고, SSL 컨텍스트와 SSL 구조를 생성한 후, 정리하는 기본적인 과정을 보여줍니다.

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
