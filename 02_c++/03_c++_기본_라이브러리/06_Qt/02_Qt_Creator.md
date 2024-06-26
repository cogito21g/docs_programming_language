#### 6.2. Qt Creator 사용법

Qt Creator는 Qt 애플리케이션 개발을 위한 통합 개발 환경(IDE)입니다. 이 IDE는 프로젝트 생성, 코드 편집, 디버깅, UI 디자인 등을 포함한 다양한 기능을 제공합니다.

##### Qt Creator 설치 및 시작

**Windows, macOS, Linux**:
1. [Qt 다운로드 페이지](https://www.qt.io/download-qt-installer)에서 Qt Online Installer를 다운로드하고 설치합니다. 설치 과정에서 Qt Creator를 선택합니다.
2. 설치가 완료되면 Qt Creator를 실행합니다.

##### 새로운 프로젝트 생성

1. Qt Creator를 실행합니다.
2. 시작 페이지에서 "New Project"를 클릭합니다.
3. "Application" 아래에서 "Qt Widgets Application"을 선택하고 "Choose..."를 클릭합니다.
4. 프로젝트 이름과 위치를 입력하고 "Next"를 클릭합니다.
5. 사용 가능한 Qt 버전을 선택하고 "Next"를 클릭합니다.
6. 클래스 정보 (예: `MainWindow`)를 입력하고 "Next"를 클릭합니다.
7. 프로젝트를 생성할 때 사용되는 빌드 시스템 (예: CMake)을 선택하고 "Next"를 클릭합니다.
8. 모든 설정을 확인하고 "Finish"를 클릭하여 프로젝트를 생성합니다.

##### 코드 편집

프로젝트가 생성되면, Qt Creator의 편집기에서 소스 파일과 헤더 파일을 열어 코드를 편집할 수 있습니다. 예를 들어, `main.cpp` 파일을 열어 다음과 같은 기본 코드를 확인할 수 있습니다:

**main.cpp**:
```cpp
#include "mainwindow.h"
#include <QApplication>

int main(int argc, char *argv[]) {
    QApplication a(argc, argv);
    MainWindow w;
    w.show();
    return a.exec();
}
```

**mainwindow.h**:
```cpp
#ifndef MAINWINDOW_H
#define MAINWINDOW_H

#include <QMainWindow>

namespace Ui {
class MainWindow;
}

class MainWindow : public QMainWindow {
    Q_OBJECT

public:
    explicit MainWindow(QWidget *parent = nullptr);
    ~MainWindow();

private:
    Ui::MainWindow *ui;
};

#endif // MAINWINDOW_H
```

**mainwindow.cpp**:
```cpp
#include "mainwindow.h"
#include "ui_mainwindow.h"

MainWindow::MainWindow(QWidget *parent) :
    QMainWindow(parent),
    ui(new Ui::MainWindow) {
    ui->setupUi(this);
}

MainWindow::~MainWindow() {
    delete ui;
}
```

##### UI 디자인

Qt Creator에는 UI 디자인을 위한 그래픽 편집기인 Qt Designer가 내장되어 있습니다. UI 파일 (`.ui`)을 열어 위젯을 추가하고 속성을 설정할 수 있습니다.

1. 프로젝트 뷰에서 `mainwindow.ui` 파일을 더블 클릭합니다.
2. Qt Designer가 열리면, 위젯을 드래그 앤 드롭하여 UI를 디자인합니다.
3. 위젯의 속성을 설정하고, 필요한 경우 시그널과 슬롯을 연결합니다.

##### 디버깅

Qt Creator는 강력한 디버깅 기능을 제공합니다. 디버깅을 시작하려면:

1. 상단 메뉴에서 "Debug" -> "Start Debugging" -> "Start Debugging (F5)"를 선택합니다.
2. 중단점을 설정하려면 소스 코드 편집기에서 중단점을 설정하고자 하는 줄 번호를 클릭합니다.
3. 프로그램이 중단점에서 일시 중지되면 변수 값을 검사하고, 스택 트레이스를 확인하며, 단계별로 코드를 실행할 수 있습니다.

##### 빌드 및 실행

프로젝트를 빌드하고 실행하려면:

1. 상단 메뉴에서 "Build" -> "Build Project"를 선택하여 프로젝트를 빌드합니다.
2. 빌드가 완료되면 "Run" -> "Run (Ctrl+R)"를 선택하여 애플리케이션을 실행합니다.
