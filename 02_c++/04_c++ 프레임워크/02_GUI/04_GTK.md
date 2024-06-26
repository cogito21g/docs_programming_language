#### 2.4. GTK+

GTK+는 크로스 플랫폼 GUI 툴킷으로, GNOME 데스크탑 환경에서 주로 사용되며 다양한 위젯을 제공하여 복잡한 GUI 애플리케이션을 개발할 수 있습니다.

##### GTK+ 설치 및 설정

1. **Windows**:
   - [MSYS2](https://www.msys2.org/)를 설치하고 MSYS2 쉘을 실행합니다.
   - 패키지 관리자를 업데이트하고 필요한 패키지를 설치합니다:
     ```bash
     pacman -Syu
     pacman -S mingw-w64-x86_64-gtk3
     ```
   - Visual Studio에서 프로젝트를 생성한 후, GTK+의 include 디렉토리와 lib 디렉토리를 설정합니다.
   - 필요한 라이브러리를 링크합니다: `gtk-3.lib`, `gobject-2.0.lib`, `glib-2.0.lib` 등.

2. **macOS**:
   - Homebrew를 사용하여 GTK+를 설치합니다:
     ```bash
     brew install gtk+3
     ```

3. **Linux**:
   - 패키지 관리자를 사용하여 GTK+를 설치합니다:
     ```bash
     sudo apt-get install libgtk-3-dev
     ```

##### 기본 사용법

GTK+를 사용하여 기본 윈도우와 버튼을 설정:

```cpp
#include <gtk/gtk.h>

static void on_button_clicked(GtkWidget* widget, gpointer data) {
    g_print("Button clicked!\n");
}

int main(int argc, char* argv[]) {
    gtk_init(&argc, &argv);

    GtkWidget* window = gtk_window_new(GTK_WINDOW_TOPLEVEL);
    gtk_window_set_title(GTK_WINDOW(window), "GTK+ Window");
    gtk_window_set_default_size(GTK_WINDOW(window), 800, 600);

    GtkWidget* button = gtk_button_new_with_label("Click me");
    g_signal_connect(button, "clicked", G_CALLBACK(on_button_clicked), NULL);

    gtk_container_add(GTK_CONTAINER(window), button);

    g_signal_connect(window, "destroy", G_CALLBACK(gtk_main_quit), NULL);

    gtk_widget_show_all(window);

    gtk_main();

    return 0;
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

간단한 버튼을 클릭하면 메시지 박스를 표시하는 GTK+ 코드입니다:

```cpp
#include <gtk/gtk.h>

static void on_button_clicked(GtkWidget* widget, gpointer data) {
    g_print("Button clicked!\n");
}

int main(int argc, char* argv[]) {
    gtk_init(&argc, &argv);

    GtkWidget* window = gtk_window_new(GTK_WINDOW_TOPLEVEL);
    gtk_window_set_title(GTK_WINDOW(window), "GTK+ Window");
    gtk_window_set_default_size(GTK_WINDOW(window), 800, 600);

    GtkWidget* button = gtk_button_new_with_label("Click me");
    g_signal_connect(button, "clicked", G_CALLBACK(on_button_clicked), NULL);

    gtk_container_add(GTK_CONTAINER(window), button);

    g_signal_connect(window, "destroy", G_CALLBACK(gtk_main_quit), NULL);

    gtk_widget_show_all(window);

    gtk_main();

    return 0;
}
```

이 코드는 GTK+를 사용하여 버튼을 생성하고, 버튼 클릭 시 메시지 박스를 표시하는 예제입니다.
