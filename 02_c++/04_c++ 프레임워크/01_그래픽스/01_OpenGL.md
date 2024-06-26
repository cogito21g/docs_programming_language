### 1. 그래픽 라이브러리 (Graphics Libraries)

#### 1.1. OpenGL

OpenGL은 크로스 플랫폼 그래픽 API로, 2D 및 3D 그래픽을 렌더링하는 데 사용됩니다. C++에서 OpenGL을 사용하면 강력한 그래픽 애플리케이션을 개발할 수 있습니다.

##### OpenGL 설치 및 설정

1. **Windows**:
   - Visual Studio를 사용하여 프로젝트를 생성합니다.
   - `glew`와 `glfw` 라이브러리를 다운로드하고 설치합니다.
   - 프로젝트 설정에서 include 디렉토리와 라이브러리 디렉토리를 추가합니다.
   - 필요한 라이브러리를 링크합니다: `opengl32.lib`, `glew32.lib`, `glfw3.lib`

2. **macOS**:
   - Homebrew를 사용하여 `glew`와 `glfw`를 설치합니다:
     ```bash
     brew install glew glfw
     ```
   - Xcode 프로젝트를 생성하고 include 및 library 경로를 설정합니다.

3. **Linux**:
   - 패키지 관리자를 사용하여 `glew`와 `glfw`를 설치합니다:
     ```bash
     sudo apt-get install libglew-dev libglfw3-dev
     ```

##### 기본 사용법

OpenGL을 사용한 기본 윈도우와 렌더링 설정:

```cpp
#include <GL/glew.h>
#include <GLFW/glfw3.h>
#include <iostream>

void framebuffer_size_callback(GLFWwindow* window, int width, int height) {
    glViewport(0, 0, width, height);
}

void processInput(GLFWwindow* window) {
    if (glfwGetKey(window, GLFW_KEY_ESCAPE) == GLFW_PRESS) {
        glfwSetWindowShouldClose(window, true);
    }
}

int main() {
    if (!glfwInit()) {
        std::cerr << "Failed to initialize GLFW" << std::endl;
        return -1;
    }

    GLFWwindow* window = glfwCreateWindow(800, 600, "OpenGL Window", nullptr, nullptr);
    if (!window) {
        std::cerr << "Failed to create GLFW window" << std::endl;
        glfwTerminate();
        return -1;
    }

    glfwMakeContextCurrent(window);
    glfwSetFramebufferSizeCallback(window, framebuffer_size_callback);

    if (glewInit() != GLEW_OK) {
        std::cerr << "Failed to initialize GLEW" << std::endl;
        return -1;
    }

    while (!glfwWindowShouldClose(window)) {
        processInput(window);

        glClearColor(0.2f, 0.3f, 0.3f, 1.0f);
        glClear(GL_COLOR_BUFFER_BIT);

        glfwSwapBuffers(window);
        glfwPollEvents();
    }

    glfwTerminate();
    return 0;
}
```

##### 주요 기능

1. **쉐이더 프로그래밍**:
   - 버텍스 쉐이더와 프래그먼트 쉐이더를 작성하여 GPU에서 실행될 그래픽 파이프라인을 정의합니다.
2. **VAO와 VBO**:
   - Vertex Array Object(VAO)와 Vertex Buffer Object(VBO)를 사용하여 버텍스 데이터를 효율적으로 관리합니다.
3. **텍스처 매핑**:
   - 2D 텍스처를 객체에 매핑하여 사실적인 이미지를 렌더링합니다.
4. **프레임버퍼**:
   - 커스텀 프레임버퍼를 생성하여 오프스크린 렌더링을 수행합니다.
5. **모델 로딩**:
   - 다양한 파일 포맷 (예: OBJ, FBX)에서 모델을 로드하고 렌더링합니다.

##### 예제 코드

다음은 간단한 삼각형을 렌더링하는 OpenGL 코드입니다:

```cpp
#include <GL/glew.h>
#include <GLFW/glfw3.h>
#include <iostream>

// Vertex Shader source code
const char* vertexShaderSource = R"(
#version 330 core
layout(location = 0) in vec3 aPos;
void main() {
    gl_Position = vec4(aPos, 1.0);
}
)";

// Fragment Shader source code
const char* fragmentShaderSource = R"(
#version 330 core
out vec4 FragColor;
void main() {
    FragColor = vec4(1.0, 0.5, 0.2, 1.0);
}
)";

void framebuffer_size_callback(GLFWwindow* window, int width, int height) {
    glViewport(0, 0, width, height);
}

void processInput(GLFWwindow* window) {
    if (glfwGetKey(window, GLFW_KEY_ESCAPE) == GLFW_PRESS) {
        glfwSetWindowShouldClose(window, true);
    }
}

int main() {
    if (!glfwInit()) {
        std::cerr << "Failed to initialize GLFW" << std::endl;
        return -1;
    }

    GLFWwindow* window = glfwCreateWindow(800, 600, "OpenGL Triangle", nullptr, nullptr);
    if (!window) {
        std::cerr << "Failed to create GLFW window" << std::endl;
        glfwTerminate();
        return -1;
    }

    glfwMakeContextCurrent(window);
    glfwSetFramebufferSizeCallback(window, framebuffer_size_callback);

    if (glewInit() != GLEW_OK) {
        std::cerr << "Failed to initialize GLEW" << std::endl;
        return -1;
    }

    // Vertex Shader
    GLuint vertexShader = glCreateShader(GL_VERTEX_SHADER);
    glShaderSource(vertexShader, 1, &vertexShaderSource, nullptr);
    glCompileShader(vertexShader);

    // Fragment Shader
    GLuint fragmentShader = glCreateShader(GL_FRAGMENT_SHADER);
    glShaderSource(fragmentShader, 1, &fragmentShaderSource, nullptr);
    glCompileShader(fragmentShader);

    // Shader Program
    GLuint shaderProgram = glCreateProgram();
    glAttachShader(shaderProgram, vertexShader);
    glAttachShader(shaderProgram, fragmentShader);
    glLinkProgram(shaderProgram);

    glDeleteShader(vertexShader);
    glDeleteShader(fragmentShader);

    // Vertex data
    float vertices[] = {
        -0.5f, -0.5f, 0.0f,
         0.5f, -0.5f, 0.0f,
         0.0f,  0.5f, 0.0f
    };

    GLuint VBO, VAO;
    glGenVertexArrays(1, &VAO);
    glGenBuffers(1, &VBO);

    glBindVertexArray(VAO);

    glBindBuffer(GL_ARRAY_BUFFER, VBO);
    glBufferData(GL_ARRAY_BUFFER, sizeof(vertices), vertices, GL_STATIC_DRAW);

    glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE, 3 * sizeof(float), (void*)0);
    glEnableVertexAttribArray(0);

    glBindBuffer(GL_ARRAY_BUFFER, 0);
    glBindVertexArray(0);

    while (!glfwWindowShouldClose(window)) {
        processInput(window);

        glClearColor(0.2f, 0.3f, 0.3f, 1.0f);
        glClear(GL_COLOR_BUFFER_BIT);

        glUseProgram(shaderProgram);
        glBindVertexArray(VAO);
        glDrawArrays(GL_TRIANGLES, 0, 3);

        glfwSwapBuffers(window);
        glfwPollEvents();
    }

    glDeleteVertexArrays(1, &VAO);
    glDeleteBuffers(1, &VBO);
    glDeleteProgram(shaderProgram);

    glfwTerminate();
    return 0;
}
```

이 코드는 간단한 삼각형을 OpenGL을 사용하여 렌더링합니다. 쉐이더 프로그램을 설정하고, 버텍스 데이터를 생성한 후 렌더링 루프에서 삼각형을 그립니다.
