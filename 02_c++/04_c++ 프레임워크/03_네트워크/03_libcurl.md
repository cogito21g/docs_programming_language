#### 3.3. libcurl

libcurl은 데이터 전송을 위한 URL 구문 분석 및 HTTP, FTP, SMTP 등의 프로토콜을 지원하는 라이브러리입니다. 크로스 플랫폼 라이브러리로 다양한 네트워크 작업을 쉽게 수행할 수 있습니다.

##### libcurl 설치 및 설정

1. **Windows**:
   - [libcurl 다운로드 페이지](https://curl.se/download.html)에서 Windows용 libcurl을 다운로드합니다.
   - 압축을 풀고 Visual Studio에서 프로젝트를 생성한 후, libcurl의 include 디렉토리와 lib 디렉토리를 설정합니다.
   - 필요한 라이브러리를 링크합니다: `libcurl.lib`.

2. **macOS**:
   - Homebrew를 사용하여 libcurl을 설치합니다:
     ```bash
     brew install curl
     ```

3. **Linux**:
   - 패키지 관리자를 사용하여 libcurl을 설치합니다:
     ```bash
     sudo apt-get install libcurl4-openssl-dev
     ```

##### 기본 사용법

libcurl을 사용하여 간단한 HTTP GET 요청을 수행:

```cpp
#include <curl/curl.h>
#include <iostream>

size_t WriteCallback(void* contents, size_t size, size_t nmemb, void* userp) {
    ((std::string*)userp)->append((char*)contents, size * nmemb);
    return size * nmemb;
}

int main() {
    CURL* curl;
    CURLcode res;
    std::string readBuffer;

    curl_global_init(CURL_GLOBAL_DEFAULT);
    curl = curl_easy_init();
    if (curl) {
        curl_easy_setopt(curl, CURLOPT_URL, "https://www.example.com/");
        curl_easy_setopt(curl, CURLOPT_WRITEFUNCTION, WriteCallback);
        curl_easy_setopt(curl, CURLOPT_WRITEDATA, &readBuffer);
        res = curl_easy_perform(curl);
        if (res != CURLE_OK) {
            std::cerr << "curl_easy_perform() failed: " << curl_easy_strerror(res) << std::endl;
        }
        curl_easy_cleanup(curl);
    }
    curl_global_cleanup();

    std::cout << readBuffer << std::endl;

    return 0;
}
```

##### 주요 기능

1. **HTTP/HTTPS**:
   - HTTP 및 HTTPS 요청을 통해 웹 서버와 통신할 수 있습니다.
2. **FTP/FTPS**:
   - FTP 및 FTPS를 사용하여 파일을 업로드하고 다운로드할 수 있습니다.
3. **SMTP**:
   - SMTP를 사용하여 이메일을 보낼 수 있습니다.
4. **프로토콜 지원**:
   - 다양한 프로토콜(SFTP, SCP, LDAP 등)을 지원합니다.
5. **쿠키 및 세션 관리**:
   - 쿠키를 저장하고 세션을 관리할 수 있습니다.
6. **폼 데이터 전송**:
   - HTML 폼 데이터를 전송할 수 있습니다.
7. **멀티 스레드**:
   - 멀티 스레드 환경에서도 안전하게 사용할 수 있습니다.

##### 예제 코드

간단한 HTTP POST 요청을 수행하는 libcurl 코드입니다:

```cpp
#include <curl/curl.h>
#include <iostream>
#include <string>

size_t WriteCallback(void* contents, size_t size, size_t nmemb, void* userp) {
    ((std::string*)userp)->append((char*)contents, size * nmemb);
    return size * nmemb;
}

int main() {
    CURL* curl;
    CURLcode res;
    std::string readBuffer;

    curl_global_init(CURL_GLOBAL_DEFAULT);
    curl = curl_easy_init();
    if (curl) {
        curl_easy_setopt(curl, CURLOPT_URL, "https://httpbin.org/post");
        curl_easy_setopt(curl, CURLOPT_POST, 1L);

        const char* postFields = "name=JohnDoe&project=curl";
        curl_easy_setopt(curl, CURLOPT_POSTFIELDS, postFields);

        curl_easy_setopt(curl, CURLOPT_WRITEFUNCTION, WriteCallback);
        curl_easy_setopt(curl, CURLOPT_WRITEDATA, &readBuffer);

        res = curl_easy_perform(curl);
        if (res != CURLE_OK) {
            std::cerr << "curl_easy_perform() failed: " << curl_easy_strerror(res) << std::endl;
        }
        curl_easy_cleanup(curl);
    }
    curl_global_cleanup();

    std::cout << readBuffer << std::endl;

    return 0;
}
```

이 코드는 libcurl을 사용하여 HTTP POST 요청을 보내고 응답을 출력하는 예제입니다.
