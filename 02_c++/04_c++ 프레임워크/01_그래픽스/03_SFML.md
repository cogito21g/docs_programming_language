#### 1.3. SFML (Simple and Fast Multimedia Library)

SFML은 멀티미디어 애플리케이션을 개발하기 위한 간단하고 빠른 라이브러리입니다. 그래픽, 오디오, 네트워크, 윈도우 및 이벤트 처리를 지원합니다. SFML은 2D 게임과 같은 애플리케이션을 개발하는 데 자주 사용됩니다.

##### SFML 설치 및 설정

1. **Windows**:
   - [SFML 다운로드 페이지](https://www.sfml-dev.org/download.php)에서 Windows용 SFML을 다운로드합니다.
   - 압축을 풀고 Visual Studio에서 프로젝트를 생성한 후, SFML의 include 디렉토리와 lib 디렉토리를 설정합니다.
   - 필요한 라이브러리를 링크합니다: `sfml-graphics.lib`, `sfml-window.lib`, `sfml-system.lib`

2. **macOS**:
   - Homebrew를 사용하여 SFML을 설치합니다:
     ```bash
     brew install sfml
     ```

3. **Linux**:
   - 패키지 관리자를 사용하여 SFML을 설치합니다:
     ```bash
     sudo apt-get install libsfml-dev
     ```

##### 기본 사용법

SFML을 사용하여 기본 윈도우와 그래픽 설정:

```cpp
#include <SFML/Graphics.hpp>
#include <iostream>

int main() {
    sf::RenderWindow window(sf::VideoMode(800, 600), "SFML Window");

    while (window.isOpen()) {
        sf::Event event;
        while (window.pollEvent(event)) {
            if (event.type == sf::Event::Closed) {
                window.close();
            }
        }

        window.clear(sf::Color::Black);

        // Drawing code here

        window.display();
    }

    return 0;
}
```

##### 주요 기능

1. **그래픽**:
   - 다양한 도형(사각형, 원, 선 등)을 그릴 수 있으며, 텍스처, 스프라이트, 셰이프 등을 사용할 수 있습니다.
2. **오디오**:
   - 오디오 파일을 재생하고, 사운드 버퍼와 스트림을 관리할 수 있습니다.
3. **네트워크**:
   - TCP와 UDP 소켓을 사용하여 네트워크 통신을 할 수 있습니다.
4. **입출력**:
   - 키보드, 마우스, 조이스틱 등의 입력을 처리할 수 있습니다.
5. **이벤트 처리**:
   - 윈도우 이벤트(닫기, 크기 조절 등)를 처리할 수 있습니다.

##### 예제 코드

간단한 사각형을 렌더링하는 SFML 코드입니다:

```cpp
#include <SFML/Graphics.hpp>

int main() {
    sf::RenderWindow window(sf::VideoMode(800, 600), "SFML Window");

    sf::RectangleShape rectangle(sf::Vector2f(100.f, 100.f));
    rectangle.setFillColor(sf::Color::Green);
    rectangle.setPosition(350.f, 250.f);

    while (window.isOpen()) {
        sf::Event event;
        while (window.pollEvent(event)) {
            if (event.type == sf::Event::Closed) {
                window.close();
            }
        }

        window.clear(sf::Color::Black);
        window.draw(rectangle);
        window.display();
    }

    return 0;
}
```

이 코드는 SFML을 사용하여 윈도우를 생성하고, 간단한 녹색 사각형을 렌더링하는 예제입니다.
