### 2. GUI 프레임워크 (GUI Frameworks)

#### 2.2. wxWidgets

wxWidgets는 크로스 플랫폼 GUI 라이브러리로, 다양한 플랫폼에서 네이티브 룩 앤드 필을 제공하는 C++ 프레임워크입니다.

##### wxWidgets 설치 및 설정

1. **Windows**:
   - [wxWidgets 다운로드 페이지](https://www.wxwidgets.org/downloads/)에서 wxWidgets를 다운로드합니다.
   - 압축을 풀고 Visual Studio에서 프로젝트를 생성한 후, wxWidgets의 include 디렉토리와 lib 디렉토리를 설정합니다.
   - 필요한 라이브러리를 링크합니다: `wxbase31u.lib`, `wxmsw31u_core.lib`, `wxmsw31u_adv.lib` 등.

2. **macOS**:
   - Homebrew를 사용하여 wxWidgets를 설치합니다:
     ```bash
     brew install wxwidgets
     ```

3. **Linux**:
   - 패키지 관리자를 사용하여 wxWidgets를 설치합니다:
     ```bash
     sudo apt-get install libwxgtk3.0-dev
     ```

##### 기본 사용법

wxWidgets를 사용하여 기본 윈도우와 버튼을 설정:

```cpp
#include <wx/wx.h>

class MyApp : public wxApp {
public:
    virtual bool OnInit();
};

class MyFrame : public wxFrame {
public:
    MyFrame(const wxString& title);

    void OnButtonClicked(wxCommandEvent& event);
};

wxIMPLEMENT_APP(MyApp);

bool MyApp::OnInit() {
    MyFrame* frame = new MyFrame("wxWidgets Window");
    frame->Show(true);
    return true;
}

MyFrame::MyFrame(const wxString& title) : wxFrame(nullptr, wxID_ANY, title, wxDefaultPosition, wxSize(800, 600)) {
    wxPanel* panel = new wxPanel(this, wxID_ANY);
    wxButton* button = new wxButton(panel, wxID_ANY, "Click me", wxPoint(350, 250), wxSize(100, 50));
    button->Bind(wxEVT_BUTTON, &MyFrame::OnButtonClicked, this);
}

void MyFrame::OnButtonClicked(wxCommandEvent& event) {
    wxMessageBox("Button clicked!", "Info", wxOK | wxICON_INFORMATION);
}
```

##### 주요 기능

1. **GUI 개발**:
   - 다양한 위젯(버튼, 레이블, 텍스트 입력 등)을 제공하며, 사용자 정의 위젯을 생성할 수 있습니다.
2. **이벤트 처리**:
   - 이벤트 기반 프로그래밍을 지원하며, 다양한 이벤트를 처리할 수 있습니다.
3. **네트워킹**:
   - HTTP, FTP, TCP, UDP 등 다양한 네트워크 프로토콜을 지원합니다.
4. **파일 입출력**:
   - 다양한 파일 포맷을 읽고 쓸 수 있으며, 파일 시스템을 탐색할 수 있습니다.
5. **멀티스레딩**:
   - wxThread를 사용하여 스레드를 관리하고, 비동기 작업을 처리할 수 있습니다.
6. **데이터베이스**:
   - SQLite, MySQL, PostgreSQL 등 다양한 데이터베이스와 연동할 수 있습니다.

##### 예제 코드

간단한 버튼을 클릭하면 메시지 박스를 표시하는 wxWidgets 코드입니다:

```cpp
#include <wx/wx.h>

class MyApp : public wxApp {
public:
    virtual bool OnInit();
};

class MyFrame : public wxFrame {
public:
    MyFrame(const wxString& title);

    void OnButtonClicked(wxCommandEvent& event);
};

wxIMPLEMENT_APP(MyApp);

bool MyApp::OnInit() {
    MyFrame* frame = new MyFrame("wxWidgets Window");
    frame->Show(true);
    return true;
}

MyFrame::MyFrame(const wxString& title) : wxFrame(nullptr, wxID_ANY, title, wxDefaultPosition, wxSize(800, 600)) {
    wxPanel* panel = new wxPanel(this, wxID_ANY);
    wxButton* button = new wxButton(panel, wxID_ANY, "Click me", wxPoint(350, 250), wxSize(100, 50));
    button->Bind(wxEVT_BUTTON, &MyFrame::OnButtonClicked, this);
}

void MyFrame::OnButtonClicked(wxCommandEvent& event) {
    wxMessageBox("Button clicked!", "Info", wxOK | wxICON_INFORMATION);
}
```

이 코드는 wxWidgets를 사용하여 버튼을 생성하고, 버튼 클릭 시 메시지 박스를 표시하는 예제입니다.
