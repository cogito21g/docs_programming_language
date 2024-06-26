#### 2.3. FLTK (Fast Light Toolkit)

FLTK는 가볍고 빠른 크로스 플랫폼 GUI 툴킷으로, 작은 메모리 풋프린트와 높은 성능을 제공합니다. FLTK는 다양한 위젯을 제공하며, 간단한 API로 쉽게 GUI 애플리케이션을 개발할 수 있습니다.

##### FLTK 설치 및 설정

1. **Windows**:
   - [FLTK 다운로드 페이지](https://www.fltk.org/software.php)에서 FLTK를 다운로드합니다.
   - 압축을 풀고 Visual Studio에서 프로젝트를 생성한 후, FLTK의 include 디렉토리와 lib 디렉토리를 설정합니다.
   - 필요한 라이브러리를 링크합니다: `fltk.lib`, `fltk_images.lib`, `fltk_forms.lib` 등.

2. **macOS**:
   - Homebrew를 사용하여 FLTK를 설치합니다:
     ```bash
     brew install fltk
     ```

3. **Linux**:
   - 패키지 관리자를 사용하여 FLTK를 설치합니다:
     ```bash
     sudo apt-get install libfltk1.3-dev
     ```

##### 기본 사용법

FLTK를 사용하여 기본 윈도우와 버튼을 설정:

```cpp
#include <FL/Fl.H>
#include <FL/Fl_Window.H>
#include <FL/Fl_Button.H>
#include <FL/fl_ask.H>

void button_callback(Fl_Widget* widget, void*) {
    fl_message("Button clicked!");
}

int main(int argc, char** argv) {
    Fl_Window* window = new Fl_Window(800, 600, "FLTK Window");

    Fl_Button* button = new Fl_Button(350, 250, 100, 50, "Click me");
    button->callback(button_callback);

    window->end();
    window->show(argc, argv);
    return Fl::run();
}
```

##### 주요 기능

1. **GUI 개발**:
   - 다양한 위젯(버튼, 레이블, 텍스트 입력 등)을 제공하며, 사용자 정의 위젯을 생성할 수 있습니다.
2. **이벤트 처리**:
   - 이벤트 기반 프로그래밍을 지원하며, 다양한 이벤트를 처리할 수 있습니다.
3. **네이티브 룩 앤드 필**:
   - 각 플랫폼의 네이티브 룩 앤드 필을 제공하여 일관된 사용자 경험을 제공합니다.
4. **파일 입출력**:
   - 다양한 파일 포맷을 읽고 쓸 수 있으며, 파일 시스템을 탐색할 수 있습니다.
5. **타이머 및 이벤트**:
   - 타이머를 설정하고, 다양한 이벤트를 처리할 수 있습니다.
6. **텍스트 렌더링**:
   - 텍스트를 렌더링하고, 폰트를 설정할 수 있습니다.

##### 예제 코드

간단한 버튼을 클릭하면 메시지 박스를 표시하는 FLTK 코드입니다:

```cpp
#include <FL/Fl.H>
#include <FL/Fl_Window.H>
#include <FL/Fl_Button.H>
#include <FL/fl_ask.H>

void button_callback(Fl_Widget* widget, void*) {
    fl_message("Button clicked!");
}

int main(int argc, char** argv) {
    Fl_Window* window = new Fl_Window(800, 600, "FLTK Window");

    Fl_Button* button = new Fl_Button(350, 250, 100, 50, "Click me");
    button->callback(button_callback);

    window->end();
    window->show(argc, argv);
    return Fl::run();
}
```

이 코드는 FLTK를 사용하여 버튼을 생성하고, 버튼 클릭 시 메시지 박스를 표시하는 예제입니다.
