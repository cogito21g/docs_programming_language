#### 1.2. Vulkan

Vulkan은 고성능 그래픽과 컴퓨팅을 위한 최신 API입니다. OpenGL에 비해 더 낮은 수준의 접근을 허용하여 그래픽 하드웨어의 성능을 극대화할 수 있습니다.

##### Vulkan 설치 및 설정

1. **Windows**:
   - Vulkan SDK를 다운로드하고 설치합니다: [LunarG Vulkan SDK](https://vulkan.lunarg.com/sdk/home)
   - Visual Studio에서 프로젝트를 생성하고 include 및 library 디렉토리를 설정합니다.

2. **macOS**:
   - Vulkan을 직접 지원하지 않지만, MoltenVK를 사용하여 Vulkan을 Metal API로 변환할 수 있습니다.
   - MoltenVK 설치 및 설정: [MoltenVK](https://github.com/KhronosGroup/MoltenVK)

3. **Linux**:
   - 패키지 관리자를 사용하여 Vulkan을 설치합니다:
     ```bash
     sudo apt-get install vulkan-sdk
     ```

##### 기본 사용법

Vulkan을 사용하여 기본 윈도우와 렌더링 설정:

```cpp
#include <vulkan/vulkan.h>
#include <GLFW/glfw3.h>
#include <iostream>

const int WIDTH = 800;
const int HEIGHT = 600;

GLFWwindow* window;
VkInstance instance;

void initWindow() {
    glfwInit();
    glfwWindowHint(GLFW_CLIENT_API, GLFW_NO_API);
    window = glfwCreateWindow(WIDTH, HEIGHT, "Vulkan Window", nullptr, nullptr);
}

void initVulkan() {
    VkApplicationInfo appInfo{};
    appInfo.sType = VK_STRUCTURE_TYPE_APPLICATION_INFO;
    appInfo.pApplicationName = "Hello Vulkan";
    appInfo.applicationVersion = VK_MAKE_VERSION(1, 0, 0);
    appInfo.pEngineName = "No Engine";
    appInfo.engineVersion = VK_MAKE_VERSION(1, 0, 0);
    appInfo.apiVersion = VK_API_VERSION_1_0;

    VkInstanceCreateInfo createInfo{};
    createInfo.sType = VK_STRUCTURE_TYPE_INSTANCE_CREATE_INFO;
    createInfo.pApplicationInfo = &appInfo;

    if (vkCreateInstance(&createInfo, nullptr, &instance) != VK_SUCCESS) {
        throw std::runtime_error("failed to create instance!");
    }
}

void mainLoop() {
    while (!glfwWindowShouldClose(window)) {
        glfwPollEvents();
    }
}

void cleanup() {
    vkDestroyInstance(instance, nullptr);
    glfwDestroyWindow(window);
    glfwTerminate();
}

int main() {
    try {
        initWindow();
        initVulkan();
        mainLoop();
        cleanup();
    } catch (const std::exception& e) {
        std::cerr << e.what() << std::endl;
        return EXIT_FAILURE;
    }
    return EXIT_SUCCESS;
}
```

##### 주요 기능

1. **셋업**:
   - Vulkan은 더 많은 초기 설정을 필요로 합니다. 인스턴스 생성, 물리적 장치 선택, 논리적 장치 생성 등의 작업이 포함됩니다.
2. **쉐이더 모듈**:
   - 쉐이더 모듈은 SPIR-V 형식의 바이트코드로 제공되며, Vulkan은 이를 사용하여 그래픽 파이프라인을 구성합니다.
3. **커맨드 버퍼**:
   - 모든 드로우 호출과 상태 변경은 커맨드 버퍼에 기록되어 제출됩니다.
4. **동기화**:
   - Vulkan은 더 많은 동기화 객체를 사용하여 CPU와 GPU 간의 동기화를 관리합니다. (예: 세마포어, 펜스)
5. **메모리 관리**:
   - Vulkan은 메모리 할당 및 관리에 더 많은 제어권을 부여합니다.

##### 예제 코드

간단한 삼각형을 렌더링하는 Vulkan 코드입니다:

```cpp
#include <vulkan/vulkan.h>
#include <GLFW/glfw3.h>
#include <iostream>

const int WIDTH = 800;
const int HEIGHT = 600;

GLFWwindow* window;
VkInstance instance;

void initWindow() {
    glfwInit();
    glfwWindowHint(GLFW_CLIENT_API, GLFW_NO_API);
    window = glfwCreateWindow(WIDTH, HEIGHT, "Vulkan Window", nullptr, nullptr);
}

void initVulkan() {
    VkApplicationInfo appInfo{};
    appInfo.sType = VK_STRUCTURE_TYPE_APPLICATION_INFO;
    appInfo.pApplicationName = "Hello Vulkan";
    appInfo.applicationVersion = VK_MAKE_VERSION(1, 0, 0);
    appInfo.pEngineName = "No Engine";
    appInfo.engineVersion = VK_MAKE_VERSION(1, 0, 0);
    appInfo.apiVersion = VK_API_VERSION_1_0;

    VkInstanceCreateInfo createInfo{};
    createInfo.sType = VK_STRUCTURE_TYPE_INSTANCE_CREATE_INFO;
    createInfo.pApplicationInfo = &appInfo;

    if (vkCreateInstance(&createInfo, nullptr, &instance) != VK_SUCCESS) {
        throw std::runtime_error("failed to create instance!");
    }
}

void mainLoop() {
    while (!glfwWindowShouldClose(window)) {
        glfwPollEvents();
    }
}

void cleanup() {
    vkDestroyInstance(instance, nullptr);
    glfwDestroyWindow(window);
    glfwTerminate();
}

int main() {
    try {
        initWindow();
        initVulkan();
        mainLoop();
        cleanup();
    } catch (const std::exception& e) {
        std::cerr << e.what() << std::endl;
        return EXIT_FAILURE;
    }
    return EXIT_SUCCESS;
}
```

이 코드는 Vulkan을 사용하여 윈도우를 생성하고, 기본적인 인스턴스를 설정하는 예제입니다.
