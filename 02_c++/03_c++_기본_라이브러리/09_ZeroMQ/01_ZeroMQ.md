### 9. ZeroMQ

ZeroMQ는 고성능 비동기 메시징 라이브러리로, 네트워크 프로토콜을 추상화하여 간단한 소켓 API로 제공됩니다. ZeroMQ는 다양한 메시징 패턴과 고성능 비동기 I/O를 지원하며, 네트워크 프로그래밍을 쉽게 만들어줍니다.

#### 9.1. ZeroMQ 개요 및 설치

##### ZeroMQ 개요

ZeroMQ는 다음과 같은 특징을 가지고 있습니다:
- 고성능 비동기 메시징 라이브러리
- 다양한 메시징 패턴 지원 (Request-Reply, Publish-Subscribe, Push-Pull 등)
- 네트워크 프로그래밍을 위한 간단한 API 제공
- 다양한 언어 바인딩 지원

##### ZeroMQ 설치

**Linux**:
1. ZeroMQ 라이브러리 설치:
   ```bash
   sudo apt-get install libzmq3-dev
   ```

2. cppzmq (C++ 바인딩) 설치:
   ```bash
   sudo apt-get install libzmqpp-dev
   ```

**macOS**:
1. Homebrew를 사용하여 ZeroMQ와 cppzmq를 설치합니다:
   ```bash
   brew install zeromq
   brew install cppzmq
   ```

**Windows**:
1. [ZeroMQ 다운로드 페이지](https://zeromq.org/download/)에서 Windows용 설치 파일을 다운로드하고 설치합니다.

