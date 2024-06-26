#### 5.4. POCO Util

POCO Util 라이브러리는 애플리케이션 구성, 명령줄 파싱, 설정 파일 처리 등을 포함한 유틸리티 기능을 제공합니다. 이 라이브러리는 특히 서버 애플리케이션 개발에 유용합니다.

##### 예제 1: 애플리케이션 구성

POCO Util 라이브러리를 사용하여 애플리케이션의 구성을 쉽게 관리할 수 있습니다. 다음 예제에서는 POCO Util을 사용하여 애플리케이션을 구성하는 방법을 설명합니다.

**프로젝트 구조**:
```
ConfigExample/
├── CMakeLists.txt
├── config.properties
└── main.cpp
```

**CMakeLists.txt**:
```cmake
cmake_minimum_required(VERSION 3.10)

# 프로젝트 이름 설정
project(ConfigExample)

# C++ 표준 설정
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# POCO 라이브러리 찾기
find_package(Poco REQUIRED Util Foundation)

# 실행 파일 추가
add_executable(ConfigExample main.cpp)

# POCO 라이브러리 링크
target_link_libraries(ConfigExample Poco::Util Poco::Foundation)
```

**config.properties**:
```
app.name = POCO Util Example
app.version = 1.0
```

**main.cpp**:
```cpp
#include <Poco/Util/ServerApplication.h>
#include <Poco/Util/Option.h>
#include <Poco/Util/OptionSet.h>
#include <Poco/Util/HelpFormatter.h>
#include <Poco/Util/IniFileConfiguration.h>
#include <Poco/AutoPtr.h>
#include <iostream>

class MyApp : public Poco::Util::ServerApplication {
protected:
    void initialize(Application& self) override {
        loadConfiguration("config.properties"); // 설정 파일 로드
        ServerApplication::initialize(self);
    }

    void uninitialize() override {
        ServerApplication::uninitialize();
    }

    int main(const std::vector<std::string>& args) override {
        std::string appName = config().getString("app.name", "DefaultApp");
        std::string appVersion = config().getString("app.version", "1.0");

        std::cout << "Application Name: " << appName << std::endl;
        std::cout << "Application Version: " << appVersion << std::endl;

        waitForTerminationRequest();
        return Application::EXIT_OK;
    }
};

int main(int argc, char** argv) {
    MyApp app;
    return app.run(argc, argv);
}
```

이 예제에서는 `Poco::Util::ServerApplication` 클래스를 사용하여 애플리케이션을 구성하고, `config.properties` 파일을 통해 설정을 로드합니다. 설정 파일에는 애플리케이션의 이름과 버전이 포함되어 있습니다.

##### 예제 2: 명령줄 파싱

POCO Util 라이브러리를 사용하여 명령줄 옵션을 파싱하는 방법을 설명합니다.

**프로젝트 구조**:
```
CmdLineExample/
├── CMakeLists.txt
└── main.cpp
```

**CMakeLists.txt**:
```cmake
cmake_minimum_required(VERSION 3.10)

# 프로젝트 이름 설정
project(CmdLineExample)

# C++ 표준 설정
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# POCO 라이브러리 찾기
find_package(Poco REQUIRED Util Foundation)

# 실행 파일 추가
add_executable(CmdLineExample main.cpp)

# POCO 라이브러리 링크
target_link_libraries(CmdLineExample Poco::Util Poco::Foundation)
```

**main.cpp**:
```cpp
#include <Poco/Util/ServerApplication.h>
#include <Poco/Util/Option.h>
#include <Poco/Util/OptionSet.h>
#include <Poco/Util/HelpFormatter.h>
#include <iostream>

class MyApp : public Poco::Util::ServerApplication {
protected:
    void defineOptions(Poco::Util::OptionSet& options) override {
        ServerApplication::defineOptions(options);
        options.addOption(
            Poco::Util::Option("name", "n", "specify the application name")
                .required(false)
                .repeatable(false)
                .argument("name")
                .callback(Poco::Util::OptionCallback<MyApp>(this, &MyApp::handleName))
        );
    }

    void handleName(const std::string& name, const std::string& value) {
        _appName = value;
    }

    int main(const std::vector<std::string>& args) override {
        std::cout << "Application Name: " << _appName << std::endl;
        waitForTerminationRequest();
        return Application::EXIT_OK;
    }

private:
    std::string _appName = "DefaultApp";
};

int main(int argc, char** argv) {
    MyApp app;
    return app.run(argc, argv);
}
```

이 예제에서는 `Poco::Util::ServerApplication` 클래스를 사용하여 명령줄 옵션을 정의하고, `defineOptions` 메서드를 사용하여 옵션을 추가합니다. `handleName` 메서드는 명령줄 옵션이 제공된 경우 호출되어 애플리케이션 이름을 설정합니다.

##### 예제 3: 설정 파일 처리

POCO Util 라이브러리를 사용하여 다양한 형식의 설정 파일을 처리할 수 있습니다. 다음 예제에서는 INI 파일을 처리하는 방법을 설명합니다.

**프로젝트 구조**:
```
IniFileExample/
├── CMakeLists.txt
├── config.ini
└── main.cpp
```

**CMakeLists.txt**:
```cmake
cmake_minimum_required(VERSION 3.10)

# 프로젝트 이름 설정
project(IniFileExample)

# C++ 표준 설정
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# POCO 라이브러리 찾기
find_package(Poco REQUIRED Util Foundation)

# 실행 파일 추가
add_executable(IniFileExample main.cpp)

# POCO 라이브러리 링크
target_link_libraries(IniFileExample Poco::Util Poco::Foundation)
```

**config.ini**:
```
[application]
name = POCO IniFile Example
version = 1.0
```

**main.cpp**:
```cpp
#include <Poco/Util/ServerApplication.h>
#include <Poco/Util/IniFileConfiguration.h>
#include <Poco/AutoPtr.h>
#include <iostream>

class MyApp : public Poco::Util::ServerApplication {
protected:
    void initialize(Application& self) override {
        Poco::AutoPtr<Poco::Util::IniFileConfiguration> pConf(new Poco::Util::IniFileConfiguration("config.ini"));
        config().add(pConf, PRIO_APPLICATION);
        ServerApplication::initialize(self);
    }

    void uninitialize() override {
        ServerApplication::uninitialize();
    }

    int main(const std::vector<std::string>& args) override {
        std::string appName = config().getString("application.name", "DefaultApp");
        std::string appVersion = config().getString("application.version", "1.0");

        std::cout << "Application Name: " << appName << std::endl;
        std::cout << "Application Version: " << appVersion << std::endl;

        waitForTerminationRequest();
        return Application::EXIT_OK;
    }
};

int main(int argc, char** argv) {
    MyApp app;
    return app.run(argc, argv);
}
```

이 예제에서는 `Poco::Util::IniFileConfiguration` 클래스를 사용하여 INI 파일을 로드하고, 설정을 애플리케이션에 추가합니다. 설정 파일에는 애플리케이션의 이름과 버전이 포함되어 있습니다.
