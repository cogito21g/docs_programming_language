#### 5.6. POCO JSON

POCO JSON 라이브러리는 JSON 데이터를 파싱하고 생성하는 기능을 제공합니다. 이 라이브러리는 JSON 데이터를 쉽게 처리할 수 있는 다양한 기능을 포함하고 있습니다.

##### 예제 1: JSON 파싱

POCO JSON 라이브러리를 사용하여 JSON 데이터를 파싱하는 예제입니다.

**프로젝트 구조**:
```
JsonParseExample/
├── CMakeLists.txt
└── main.cpp
```

**CMakeLists.txt**:
```cmake
cmake_minimum_required(VERSION 3.10)

# 프로젝트 이름 설정
project(JsonParseExample)

# C++ 표준 설정
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# POCO 라이브러리 찾기
find_package(Poco REQUIRED JSON Foundation)

# 실행 파일 추가
add_executable(JsonParseExample main.cpp)

# POCO 라이브러리 링크
target_link_libraries(JsonParseExample Poco::JSON Poco::Foundation)
```

**main.cpp**:
```cpp
#include <iostream>
#include <Poco/JSON/Parser.h>
#include <Poco/Dynamic/Var.h>
#include <Poco/JSON/Object.h>
#include <Poco/JSON/Array.h>

int main() {
    std::string jsonString = R"({
        "name": "John Doe",
        "age": 30,
        "is_student": false,
        "skills": ["C++", "Python", "Java"]
    })";

    Poco::JSON::Parser parser;
    Poco::Dynamic::Var result = parser.parse(jsonString);
    Poco::JSON::Object::Ptr jsonObject = result.extract<Poco::JSON::Object::Ptr>();

    std::string name = jsonObject->getValue<std::string>("name");
    int age = jsonObject->getValue<int>("age");
    bool isStudent = jsonObject->getValue<bool>("is_student");
    Poco::JSON::Array::Ptr skills = jsonObject->getArray("skills");

    std::cout << "Name: " << name << std::endl;
    std::cout << "Age: " << age << std::endl;
    std::cout << "Is student: " << std::boolalpha << isStudent << std::endl;

    std::cout << "Skills: ";
    for (size_t i = 0; i < skills->size(); ++i) {
        std::cout << skills->getElement<std::string>(i) << (i < skills->size() - 1 ? ", " : "");
    }
    std::cout << std::endl;

    return 0;
}
```

이 예제에서는 `Poco::JSON::Parser` 클래스를 사용하여 JSON 문자열을 파싱하고, `Poco::JSON::Object`와 `Poco::JSON::Array` 클래스를 사용하여 JSON 데이터를 추출합니다. JSON 문자열에서 이름, 나이, 학생 여부, 기술 목록을 추출하여 출력합니다.

##### 예제 2: JSON 생성

POCO JSON 라이브러리를 사용하여 JSON 데이터를 생성하는 예제입니다.

**프로젝트 구조**:
```
JsonCreateExample/
├── CMakeLists.txt
└── main.cpp
```

**CMakeLists.txt**:
```cmake
cmake_minimum_required(VERSION 3.10)

# 프로젝트 이름 설정
project(JsonCreateExample)

# C++ 표준 설정
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# POCO 라이브러리 찾기
find_package(Poco REQUIRED JSON Foundation)

# 실행 파일 추가
add_executable(JsonCreateExample main.cpp)

# POCO 라이브러리 링크
target_link_libraries(JsonCreateExample Poco::JSON Poco::Foundation)
```

**main.cpp**:
```cpp
#include <iostream>
#include <Poco/JSON/Object.h>
#include <Poco/JSON/Array.h>
#include <Poco/JSON/Stringifier.h>

int main() {
    Poco::JSON::Object::Ptr jsonObject = new Poco::JSON::Object;

    jsonObject->set("name", "John Doe");
    jsonObject->set("age", 30);
    jsonObject->set("is_student", false);

    Poco::JSON::Array::Ptr skills = new Poco::JSON::Array;
    skills->add("C++");
    skills->add("Python");
    skills->add("Java");
    jsonObject->set("skills", skills);

    std::stringstream ss;
    jsonObject->stringify(ss, 2); // Pretty print with 2 spaces indentation
    std::cout << ss.str() << std::endl;

    return 0;
}
```

이 예제에서는 `Poco::JSON::Object`와 `Poco::JSON::Array` 클래스를 사용하여 JSON 데이터를 생성하고, `Poco::JSON::Stringifier` 클래스를 사용하여 JSON 문자열로 변환합니다. 생성된 JSON 데이터는 출력됩니다.

##### 예제 3: JSON 파일 읽기 및 쓰기

POCO JSON 라이브러리를 사용하여 JSON 파일을 읽고 쓰는 예제입니다.

**프로젝트 구조**:
```
JsonFileExample/
├── CMakeLists.txt
├── input.json
└── main.cpp
```

**CMakeLists.txt**:
```cmake
cmake_minimum_required(VERSION 3.10)

# 프로젝트 이름 설정
project(JsonFileExample)

# C++ 표준 설정
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# POCO 라이브러리 찾기
find_package(Poco REQUIRED JSON Foundation)

# 실행 파일 추가
add_executable(JsonFileExample main.cpp)

# POCO 라이브러리 링크
target_link_libraries(JsonFileExample Poco::JSON Poco::Foundation)
```

**input.json**:
```json
{
    "name": "John Doe",
    "age": 30,
    "is_student": false,
    "skills": ["C++", "Python", "Java"]
}
```

**main.cpp**:
```cpp
#include <iostream>
#include <fstream>
#include <Poco/JSON/Parser.h>
#include <Poco/JSON/Object.h>
#include <Poco/JSON/Array.h>
#include <Poco/JSON/Stringifier.h>
#include <Poco/Dynamic/Var.h>

int main() {
    // JSON 파일 읽기
    std::ifstream inputFile("input.json");
    std::stringstream buffer;
    buffer << inputFile.rdbuf();
    std::string jsonString = buffer.str();

    Poco::JSON::Parser parser;
    Poco::Dynamic::Var result = parser.parse(jsonString);
    Poco::JSON::Object::Ptr jsonObject = result.extract<Poco::JSON::Object::Ptr>();

    std::string name = jsonObject->getValue<std::string>("name");
    int age = jsonObject->getValue<int>("age");
    bool isStudent = jsonObject->getValue<bool>("is_student");
    Poco::JSON::Array::Ptr skills = jsonObject->getArray("skills");

    std::cout << "Name: " << name << std::endl;
    std::cout << "Age: " << age << std::endl;
    std::cout << "Is student: " << std::boolalpha << isStudent << std::endl;

    std::cout << "Skills: ";
    for (size_t i = 0; i < skills->size(); ++i) {
        std::cout << skills->getElement<std::string>(i) << (i < skills->size() - 1 ? ", " : "");
    }
    std::cout << std::endl;

    // JSON 파일 쓰기
    jsonObject->set("age", 31); // Modify age

    std::ofstream outputFile("output.json");
    Poco::JSON::Stringifier::stringify(jsonObject, outputFile, 2); // Pretty print with 2 spaces indentation

    return 0;
}
```

이 예제에서는 `input.json` 파일에서 JSON 데이터를 읽고, 데이터를 수정한 후 `output.json` 파일에 저장합니다. `Poco::JSON::Parser`를 사용하여 JSON 파일을 파싱하고, `Poco::JSON::Stringifier`를 사용하여 JSON 데이터를 파일에 씁니다.
