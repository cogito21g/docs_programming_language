#### 5.5. POCO XML

POCO XML 라이브러리는 XML 문서를 처리하고 조작하기 위한 기능을 제공합니다. 이 라이브러리는 XML 파싱, DOM(Document Object Model) 조작, XML 생성 등을 포함합니다.

##### 예제 1: XML 파싱

POCO XML 라이브러리를 사용하여 XML 문서를 파싱하는 예제입니다.

**프로젝트 구조**:
```
XmlParseExample/
├── CMakeLists.txt
├── example.xml
└── main.cpp
```

**CMakeLists.txt**:
```cmake
cmake_minimum_required(VERSION 3.10)

# 프로젝트 이름 설정
project(XmlParseExample)

# C++ 표준 설정
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# POCO 라이브러리 찾기
find_package(Poco REQUIRED XML Foundation)

# 실행 파일 추가
add_executable(XmlParseExample main.cpp)

# POCO 라이브러리 링크
target_link_libraries(XmlParseExample Poco::XML Poco::Foundation)
```

**example.xml**:
```xml
<root>
    <message>Hello, POCO XML!</message>
</root>
```

**main.cpp**:
```cpp
#include <iostream>
#include <Poco/DOM/DOMParser.h>
#include <Poco/DOM/Document.h>
#include <Poco/DOM/NodeList.h>
#include <Poco/DOM/Element.h>
#include <Poco/XML/XMLString.h>
#include <Poco/SAX/InputSource.h>
#include <Poco/Path.h>
#include <Poco/FileStream.h>

int main() {
    try {
        Poco::FileInputStream xmlFile("example.xml");
        Poco::XML::InputSource src(xmlFile);
        Poco::XML::DOMParser parser;
        Poco::AutoPtr<Poco::XML::Document> pDoc = parser.parse(&src);

        Poco::AutoPtr<Poco::XML::NodeList> messages = pDoc->getElementsByTagName("message");
        for (unsigned long i = 0; i < messages->length(); ++i) {
            Poco::XML::Element* pElement = static_cast<Poco::XML::Element*>(messages->item(i));
            std::cout << "Message: " << pElement->innerText() << std::endl;
        }
    } catch (Poco::Exception& ex) {
        std::cerr << "Error: " << ex.displayText() << std::endl;
    }

    return 0;
}
```

이 예제에서는 `Poco::XML::DOMParser` 클래스를 사용하여 XML 문서를 파싱하고, `getElementsByTagName` 메서드를 사용하여 특정 태그를 검색합니다. `example.xml` 파일에서 `message` 태그의 내용을 출력합니다.

##### 예제 2: XML 생성

POCO XML 라이브러리를 사용하여 XML 문서를 생성하는 예제입니다.

**프로젝트 구조**:
```
XmlCreateExample/
├── CMakeLists.txt
└── main.cpp
```

**CMakeLists.txt**:
```cmake
cmake_minimum_required(VERSION 3.10)

# 프로젝트 이름 설정
project(XmlCreateExample)

# C++ 표준 설정
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# POCO 라이브러리 찾기
find_package(Poco REQUIRED XML Foundation)

# 실행 파일 추가
add_executable(XmlCreateExample main.cpp)

# POCO 라이브러리 링크
target_link_libraries(XmlCreateExample Poco::XML Poco::Foundation)
```

**main.cpp**:
```cpp
#include <iostream>
#include <Poco/DOM/Document.h>
#include <Poco/DOM/Element.h>
#include <Poco/DOM/DOMWriter.h>
#include <Poco/XML/XMLWriter.h>
#include <Poco/FileStream.h>

int main() {
    try {
        Poco::AutoPtr<Poco::XML::Document> pDoc = new Poco::XML::Document;
        Poco::AutoPtr<Poco::XML::Element> pRoot = pDoc->createElement("root");
        pDoc->appendChild(pRoot);

        Poco::AutoPtr<Poco::XML::Element> pMessage = pDoc->createElement("message");
        pMessage->appendChild(pDoc->createTextNode("Hello, POCO XML!"));
        pRoot->appendChild(pMessage);

        Poco::FileOutputStream xmlFile("output.xml");
        Poco::XML::DOMWriter writer;
        writer.setOptions(Poco::XML::XMLWriter::PRETTY_PRINT);
        writer.writeNode(xmlFile, pDoc);
    } catch (Poco::Exception& ex) {
        std::cerr << "Error: " << ex.displayText() << std::endl;
    }

    return 0;
}
```

이 예제에서는 `Poco::XML::Document`와 `Poco::XML::Element` 클래스를 사용하여 XML 문서를 생성하고, `Poco::XML::DOMWriter` 클래스를 사용하여 XML 문서를 파일에 저장합니다. 생성된 XML 문서는 `output.xml` 파일에 저장됩니다.

##### 예제 3: XML 조작

POCO XML 라이브러리를 사용하여 XML 문서를 조작하는 예제입니다.

**프로젝트 구조**:
```
XmlManipulateExample/
├── CMakeLists.txt
├── example.xml
└── main.cpp
```

**CMakeLists.txt**:
```cmake
cmake_minimum_required(VERSION 3.10)

# 프로젝트 이름 설정
project(XmlManipulateExample)

# C++ 표준 설정
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# POCO 라이브러리 찾기
find_package(Poco REQUIRED XML Foundation)

# 실행 파일 추가
add_executable(XmlManipulateExample main.cpp)

# POCO 라이브러리 링크
target_link_libraries(XmlManipulateExample Poco::XML Poco::Foundation)
```

**example.xml**:
```xml
<root>
    <message>Hello, POCO XML!</message>
</root>
```

**main.cpp**:
```cpp
#include <iostream>
#include <Poco/DOM/DOMParser.h>
#include <Poco/DOM/Document.h>
#include <Poco/DOM/NodeList.h>
#include <Poco/DOM/Element.h>
#include <Poco/DOM/DOMWriter.h>
#include <Poco/XML/XMLString.h>
#include <Poco/SAX/InputSource.h>
#include <Poco/FileStream.h>

int main() {
    try {
        Poco::FileInputStream xmlFile("example.xml");
        Poco::XML::InputSource src(xmlFile);
        Poco::XML::DOMParser parser;
        Poco::AutoPtr<Poco::XML::Document> pDoc = parser.parse(&src);

        Poco::AutoPtr<Poco::XML::Element> pRoot = pDoc->documentElement();
        Poco::AutoPtr<Poco::XML::Element> pNewMessage = pDoc->createElement("message");
        pNewMessage->appendChild(pDoc->createTextNode("New message added"));
        pRoot->appendChild(pNewMessage);

        Poco::FileOutputStream outFile("modified.xml");
        Poco::XML::DOMWriter writer;
        writer.setOptions(Poco::XML::XMLWriter::PRETTY_PRINT);
        writer.writeNode(outFile, pDoc);
    } catch (Poco::Exception& ex) {
        std::cerr << "Error: " << ex.displayText() << std::endl;
    }

    return 0;
}
```

이 예제에서는 `example.xml` 파일을 파싱하고, 새로운 `message` 엘리먼트를 추가한 후, 수정된 XML 문서를 `modified.xml` 파일에 저장합니다.
