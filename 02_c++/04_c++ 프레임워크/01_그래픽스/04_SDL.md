#### 1.4. SDL (Simple DirectMedia Layer)

SDL은 멀티미디어 애플리케이션과 게임 개발을 위한 크로스 플랫폼 라이브러리입니다. 그래픽, 오디오, 입력 장치 등의 다양한 기능을 제공합니다.

##### SDL 설치 및 설정

1. **Windows**:
   - [SDL 다운로드 페이지](https://www.libsdl.org/download-2.0.php)에서 Windows용 SDL2 개발 라이브러리를 다운로드합니다.
   - 압축을 풀고 Visual Studio에서 프로젝트를 생성한 후, SDL의 include 디렉토리와 lib 디렉토리를 설정합니다.
   - 필요한 라이브러리를 링크합니다: `SDL2.lib`, `SDL2main.lib`
   - 프로젝트 속성에서 링커 -> 시스템 -> 서브시스템을 `콘솔`로 설정합니다.

2. **macOS**:
   - Homebrew를 사용하여 SDL을 설치합니다:
     ```bash
     brew install sdl2
     ```

3. **Linux**:
   - 패키지 관리자를 사용하여 SDL을 설치합니다:
     ```bash
     sudo apt-get install libsdl2-dev
     ```

##### 기본 사용법

SDL을 사용하여 기본 윈도우와 그래픽 설정:

```cpp
#include <SDL.h>
#include <iostream>

int main(int argc, char* argv[]) {
    if (SDL_Init(SDL_INIT_VIDEO) < 0) {
        std::cerr << "Failed to initialize SDL: " << SDL_GetError() << std::endl;
        return -1;
    }

    SDL_Window* window = SDL_CreateWindow("SDL Window",
                                          SDL_WINDOWPOS_CENTERED,
                                          SDL_WINDOWPOS_CENTERED,
                                          800, 600,
                                          SDL_WINDOW_SHOWN);

    if (!window) {
        std::cerr << "Failed to create window: " << SDL_GetError() << std::endl;
        SDL_Quit();
        return -1;
    }

    SDL_Renderer* renderer = SDL_CreateRenderer(window, -1, SDL_RENDERER_ACCELERATED);

    if (!renderer) {
        std::cerr << "Failed to create renderer: " << SDL_GetError() << std::endl;
        SDL_DestroyWindow(window);
        SDL_Quit();
        return -1;
    }

    bool running = true;
    SDL_Event event;

    while (running) {
        while (SDL_PollEvent(&event)) {
            if (event.type == SDL_QUIT) {
                running = false;
            }
        }

        SDL_SetRenderDrawColor(renderer, 0, 0, 0, 255);
        SDL_RenderClear(renderer);

        // Drawing code here

        SDL_RenderPresent(renderer);
    }

    SDL_DestroyRenderer(renderer);
    SDL_DestroyWindow(window);
    SDL_Quit();

    return 0;
}
```

##### 주요 기능

1. **그래픽**:
   - 2D 그래픽을 그릴 수 있으며, 텍스처, 스프라이트 등을 사용할 수 있습니다.
2. **오디오**:
   - 다양한 오디오 형식을 재생하고, 사운드와 음악을 관리할 수 있습니다.
3. **입력 처리**:
   - 키보드, 마우스, 조이스틱 등의 입력을 처리할 수 있습니다.
4. **타이머 및 이벤트**:
   - 타이머를 설정하고, 다양한 이벤트를 처리할 수 있습니다.
5. **텍스트 렌더링**:
   - TTF (TrueType Font)를 사용하여 텍스트를 렌더링할 수 있습니다.

##### 예제 코드

간단한 사각형을 렌더링하는 SDL 코드입니다:

```cpp
#include <SDL.h>
#include <iostream>

int main(int argc, char* argv[]) {
    if (SDL_Init(SDL_INIT_VIDEO) < 0) {
        std::cerr << "Failed to initialize SDL: " << SDL_GetError() << std::endl;
        return -1;
    }

    SDL_Window* window = SDL_CreateWindow("SDL Window",
                                          SDL_WINDOWPOS_CENTERED,
                                          SDL_WINDOWPOS_CENTERED,
                                          800, 600,
                                          SDL_WINDOW_SHOWN);

    if (!window) {
        std::cerr << "Failed to create window: " << SDL_GetError() << std::endl;
        SDL_Quit();
        return -1;
    }

    SDL_Renderer* renderer = SDL_CreateRenderer(window, -1, SDL_RENDERER_ACCELERATED);

    if (!renderer) {
        std::cerr << "Failed to create renderer: " << SDL_GetError() << std::endl;
        SDL_DestroyWindow(window);
        SDL_Quit();
        return -1;
    }

    bool running = true;
    SDL_Event event;

    while (running) {
        while (SDL_PollEvent(&event)) {
            if (event.type == SDL_QUIT) {
                running = false;
            }
        }

        SDL_SetRenderDrawColor(renderer, 0, 0, 0, 255);
        SDL_RenderClear(renderer);

        SDL_Rect rect = {350, 250, 100, 100};
        SDL_SetRenderDrawColor(renderer, 0, 255, 0, 255);
        SDL_RenderFillRect(renderer, &rect);

        SDL_RenderPresent(renderer);
    }

    SDL_DestroyRenderer(renderer);
    SDL_DestroyWindow(window);
    SDL_Quit();

    return 0;
}
```

이 코드는 SDL을 사용하여 윈도우를 생성하고, 간단한 녹색 사각형을 렌더링하는 예제입니다.
