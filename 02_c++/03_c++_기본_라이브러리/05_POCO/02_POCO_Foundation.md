#### 5.2. POCO Foundation

POCO Foundation 라이브러리는 다양한 유틸리티와 기본 기능을 제공합니다. 이 라이브러리에는 날짜 및 시간 조작, 파일 시스템 접근, 문자열 처리, 로깅, 스레드 관리 등이 포함됩니다.

##### 날짜 및 시간 조작

POCO Foundation 라이브러리에서는 `Poco::DateTime`, `Poco::Timestamp` 등의 클래스를 사용하여 날짜와 시간을 조작할 수 있습니다.

**예제**: 날짜 및 시간 조작

```cpp
#include <iostream>
#include <Poco/DateTime.h>
#include <Poco/Timespan.h>
#include <Poco/DateTimeFormatter.h>
#include <Poco/DateTimeFormat.h>

int main() {
    // 현재 시간 가져오기
    Poco::DateTime now;
    std::cout << "Current DateTime: " << Poco::DateTimeFormatter::format(now, Poco::DateTimeFormat::ISO8601_FORMAT) << std::endl;

    // 특정 시간 생성
    Poco::DateTime dt(2023, 6, 24, 12, 30, 0);
    std::cout << "Specific DateTime: " << Poco::DateTimeFormatter::format(dt, Poco::DateTimeFormat::ISO8601_FORMAT) << std::endl;

    // 시간 연산
    dt += Poco::Timespan(1, 0, 0, 0, 0); // 1일 추가
    std::cout << "DateTime + 1 day: " << Poco::DateTimeFormatter::format(dt, Poco::DateTimeFormat::ISO8601_FORMAT) << std::endl;

    return 0;
}
```

##### 파일 시스템 접근

POCO Foundation 라이브러리에서는 `Poco::File`, `Poco::Path` 등의 클래스를 사용하여 파일 시스템에 접근할 수 있습니다.

**예제**: 파일 시스템 접근

```cpp
#include <iostream>
#include <Poco/File.h>
#include <Poco/Path.h>

int main() {
    Poco::Path path("example.txt");
    Poco::File file(path);

    if (file.exists()) {
        std::cout << "File exists: " << file.path() << std::endl;
        std::cout << "File size: " << file.getSize() << " bytes" << std::endl;
    } else {
        std::cout << "File does not exist: " << file.path() << std::endl;
    }

    return 0;
}
```

##### 문자열 처리

POCO Foundation 라이브러리에서는 `Poco::StringTokenizer`, `Poco::NumberParser`, `Poco::NumberFormatter` 등의 클래스를 사용하여 문자열을 처리할 수 있습니다.

**예제**: 문자열 처리

```cpp
#include <iostream>
#include <Poco/StringTokenizer.h>
#include <Poco/NumberParser.h>
#include <Poco/NumberFormatter.h>

int main() {
    // 문자열 토큰화
    std::string str = "Hello,POCO,Library";
    Poco::StringTokenizer tokenizer(str, ",");
    for (auto it = tokenizer.begin(); it != tokenizer.end(); ++it) {
        std::cout << "Token: " << *it << std::endl;
    }

    // 숫자 파싱
    std::string numStr = "12345";
    int num = Poco::NumberParser::parse(numStr);
    std::cout << "Parsed number: " << num << std::endl;

    // 숫자 형식화
    std::string formattedNum = Poco::NumberFormatter::format(num);
    std::cout << "Formatted number: " << formattedNum << std::endl;

    return 0;
}
```

##### 로깅

POCO Foundation 라이브러리에서는 `Poco::Logger` 클래스를 사용하여 로깅을 수행할 수 있습니다.

**예제**: 로깅

```cpp
#include <Poco/Logger.h>
#include <Poco/ConsoleChannel.h>
#include <Poco/AutoPtr.h>

int main() {
    Poco::AutoPtr<Poco::ConsoleChannel> pCons(new Poco::ConsoleChannel);
    Poco::Logger& logger = Poco::Logger::get("TestLogger");
    logger.setChannel(pCons);

    logger.information("Information message");
    logger.warning("Warning message");
    logger.error("Error message");

    return 0;
}
```

##### 스레드 관리

POCO Foundation 라이브러리에서는 `Poco::Thread`, `Poco::Runnable` 등의 클래스를 사용하여 스레드를 관리할 수 있습니다.

**예제**: 스레드 관리

```cpp
#include <iostream>
#include <Poco/Thread.h>
#include <Poco/Runnable.h>

class MyRunnable : public Poco::Runnable {
public:
    void run() override {
        std::cout << "Thread is running" << std::endl;
    }
};

int main() {
    MyRunnable runnable;
    Poco::Thread thread;

    thread.start(runnable);
    thread.join();

    return 0;
}
```

이 예제에서는 `MyRunnable` 클래스를 정의하고, `Poco::Thread`를 사용하여 스레드를 시작하고 종료하는 방법을 보여줍니다.
