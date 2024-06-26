#### 6.6. QML 및 Qt Quick

QML(Quick Modeling Language)은 Qt Quick을 사용하여 유저 인터페이스(UI)를 개발하기 위한 선언형 언어입니다. Qt Quick은 QML과 JavaScript를 사용하여 인터페이스를 빠르고 쉽게 개발할 수 있게 해줍니다. QML은 직관적이고 간단한 문법을 사용하여 UI 요소를 정의하고, Qt의 강력한 기능을 통해 유연한 애니메이션 및 트랜지션을 추가할 수 있습니다.

##### 기본 QML 예제

**프로젝트 구조**:
```
QMLExample/
├── CMakeLists.txt
├── main.cpp
└── main.qml
```

**CMakeLists.txt**:
```cmake
cmake_minimum_required(VERSION 3.10)

# 프로젝트 이름 설정
project(QMLExample)

# C++ 표준 설정
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# Qt5 라이브러리 찾기
find_package(Qt5 COMPONENTS Quick REQUIRED)

# 실행 파일 추가
add_executable(QMLExample main.cpp)

# Qt5 라이브러리 링크
target_link_libraries(QMLExample Qt5::Quick)
```

**main.cpp**:
```cpp
#include <QGuiApplication>
#include <QQmlApplicationEngine>

int main(int argc, char *argv[]) {
    QGuiApplication app(argc, argv);

    QQmlApplicationEngine engine;
    engine.load(QUrl(QStringLiteral("qrc:/main.qml")));

    if (engine.rootObjects().isEmpty())
        return -1;

    return app.exec();
}
```

**main.qml**:
```qml
import QtQuick 2.12
import QtQuick.Window 2.12

Window {
    visible: true
    width: 640
    height: 480
    title: qsTr("Hello, QML")

    Rectangle {
        width: 200
        height: 200
        color: "lightblue"
        anchors.centerIn: parent

        Text {
            text: qsTr("Hello, QML")
            anchors.centerIn: parent
            font.pixelSize: 24
        }
    }
}
```

이 예제에서는 `QQmlApplicationEngine`을 사용하여 `main.qml` 파일을 로드하고, 간단한 "Hello, QML" 메시지를 표시하는 기본 창을 생성합니다. QML 파일은 `Rectangle`과 `Text` 요소를 사용하여 UI를 구성합니다.

##### QML과 C++ 통합

QML과 C++을 통합하여 더 복잡한 애플리케이션을 만들 수 있습니다. QML에서 C++ 객체에 접근하고 메서드를 호출할 수 있습니다.

**프로젝트 구조**:
```
QMLCppIntegration/
├── CMakeLists.txt
├── main.cpp
├── MyObject.h
├── MyObject.cpp
└── main.qml
```

**CMakeLists.txt**:
```cmake
cmake_minimum_required(VERSION 3.10)

# 프로젝트 이름 설정
project(QMLCppIntegration)

# C++ 표준 설정
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# Qt5 라이브러리 찾기
find_package(Qt5 COMPONENTS Quick REQUIRED)

# 실행 파일 추가
add_executable(QMLCppIntegration main.cpp MyObject.cpp)

# Qt5 라이브러리 링크
target_link_libraries(QMLCppIntegration Qt5::Quick)
```

**main.cpp**:
```cpp
#include <QGuiApplication>
#include <QQmlApplicationEngine>
#include <QQmlContext>
#include "MyObject.h"

int main(int argc, char *argv[]) {
    QGuiApplication app(argc, argv);

    QQmlApplicationEngine engine;

    MyObject myObject;
    engine.rootContext()->setContextProperty("myObject", &myObject);

    engine.load(QUrl(QStringLiteral("qrc:/main.qml")));

    if (engine.rootObjects().isEmpty())
        return -1;

    return app.exec();
}
```

**MyObject.h**:
```cpp
#ifndef MYOBJECT_H
#define MYOBJECT_H

#include <QObject>

class MyObject : public QObject {
    Q_OBJECT
    Q_PROPERTY(QString message READ message WRITE setMessage NOTIFY messageChanged)

public:
    explicit MyObject(QObject *parent = nullptr);

    QString message() const;
    void setMessage(const QString &message);

signals:
    void messageChanged();

public slots:
    void changeMessage(const QString &newMessage);

private:
    QString m_message;
};

#endif // MYOBJECT_H
```

**MyObject.cpp**:
```cpp
#include "MyObject.h"

MyObject::MyObject(QObject *parent) : QObject(parent), m_message("Hello from C++") {}

QString MyObject::message() const {
    return m_message;
}

void MyObject::setMessage(const QString &message) {
    if (message != m_message) {
        m_message = message;
        emit messageChanged();
    }
}

void MyObject::changeMessage(const QString &newMessage) {
    setMessage(newMessage);
}
```

**main.qml**:
```qml
import QtQuick 2.12
import QtQuick.Window 2.12

Window {
    visible: true
    width: 640
    height: 480
    title: qsTr("QML and C++ Integration")

    Rectangle {
        width: 200
        height: 200
        color: "lightblue"
        anchors.centerIn: parent

        Text {
            text: myObject.message
            anchors.centerIn: parent
            font.pixelSize: 24
        }

        MouseArea {
            anchors.fill: parent
            onClicked: {
                myObject.changeMessage("Message changed from QML");
            }
        }
    }
}
```

이 예제에서는 C++ 클래스를 QML에 등록하고, QML에서 C++ 객체의 속성에 접근하며 메서드를 호출하는 방법을 보여줍니다. `MyObject` 클래스는 `message`라는 속성과 `changeMessage`라는 슬롯을 가지며, QML에서 이 객체를 사용하여 메시지를 변경할 수 있습니다.
