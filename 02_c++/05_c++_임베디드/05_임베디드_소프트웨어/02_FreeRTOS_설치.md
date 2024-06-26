#### 5.2. FreeRTOS 설치 및 기본 사용법

FreeRTOS는 오픈 소스 실시간 운영체제(RTOS)로, 다양한 마이크로컨트롤러에서 사용됩니다. FreeRTOS는 효율적인 태스크 관리, 실시간 스케줄링, 동기화 메커니즘 등을 제공하여 임베디드 시스템 개발에 널리 활용됩니다.

##### FreeRTOS 설치

FreeRTOS 설치는 사용하려는 마이크로컨트롤러 및 개발 환경에 따라 다릅니다. 여기서는 STM32CubeMX와 STM32CubeIDE를 사용하여 FreeRTOS를 설정하는 방법을 설명합니다.

**1. STM32CubeMX에서 FreeRTOS 설정**

STM32CubeMX는 STM32 마이크로컨트롤러의 설정과 코드 생성을 도와주는 도구입니다. STM32CubeMX를 사용하여 FreeRTOS를 설정하는 절차는 다음과 같습니다:

- **프로젝트 생성**: STM32CubeMX를 실행하고, 새로운 프로젝트를 생성합니다. 사용하려는 STM32 마이크로컨트롤러를 선택합니다.
- **FreeRTOS 활성화**: "Middleware" 탭에서 "FreeRTOS"를 활성화합니다. "Tasks and Queues" 섹션에서 기본 태스크를 생성할 수 있습니다.
- **클럭 설정**: "Clock Configuration" 탭에서 시스템 클럭을 설정합니다.
- **핀 설정**: "Pinout & Configuration" 탭에서 사용할 핀을 설정합니다.
- **코드 생성**: 설정이 완료되면, "Project" -> "Generate Code"를 선택하여 프로젝트를 생성합니다.

**2. STM32CubeIDE에서 FreeRTOS 프로젝트 열기**

STM32CubeIDE는 STM32CubeMX와 통합된 개발 환경입니다.

- **프로젝트 열기**: STM32CubeIDE를 실행하고, STM32CubeMX에서 생성된 프로젝트를 엽니다.
- **빌드 및 실행**: 프로젝트를 빌드하고, 마이크로컨트롤러 보드에 업로드하여 실행합니다.

##### FreeRTOS 기본 사용법

FreeRTOS는 태스크, 큐, 세마포어 등의 기본 개념을 제공합니다. 여기서는 기본적인 태스크 생성과 스케줄링을 설명합니다.

**1. 태스크 생성**

FreeRTOS에서는 `xTaskCreate` 함수를 사용하여 태스크를 생성합니다. 태스크는 독립적인 실행 단위로, 고유한 스택과 우선순위를 가집니다.

```c
#include "FreeRTOS.h"
#include "task.h"

// 태스크 함수 선언
void vTask1(void *pvParameters);
void vTask2(void *pvParameters);

int main(void) {
    // FreeRTOS 초기화 코드

    // 태스크 생성
    xTaskCreate(vTask1, "Task 1", 100, NULL, 1, NULL);
    xTaskCreate(vTask2, "Task 2", 100, NULL, 2, NULL);

    // 스케줄러 시작
    vTaskStartScheduler();

    while (1) {
        // 메인 루프
    }
}

// 태스크 함수 정의
void vTask1(void *pvParameters) {
    while (1) {
        // Task 1 실행 코드
        vTaskDelay(pdMS_TO_TICKS(1000)); // 1초 지연
    }
}

void vTask2(void *pvParameters) {
    while (1) {
        // Task 2 실행 코드
        vTaskDelay(pdMS_TO_TICKS(500)); // 0.5초 지연
    }
}
```

**2. 태스크 스케줄링**

FreeRTOS는 선점형 스케줄링을 지원하여 우선순위가 높은 태스크가 실행 중인 태스크를 중단하고 실행될 수 있습니다. `vTaskDelay` 함수는 지정된 시간 동안 태스크를 지연시키고, 다른 태스크가 실행될 수 있도록 합니다.

**3. 동기화와 통신**

FreeRTOS는 태스크 간의 동기화와 통신을 위해 세마포어, 뮤텍스, 큐 등의 메커니즘을 제공합니다. 여기서는 기본적인 세마포어 사용 예제를 설명합니다.

```c
#include "FreeRTOS.h"
#include "task.h"
#include "semphr.h"

SemaphoreHandle_t xSemaphore;

void vTask1(void *pvParameters);
void vTask2(void *pvParameters);

int main(void) {
    // FreeRTOS 초기화 코드

    // 세마포어 생성
    xSemaphore = xSemaphoreCreateBinary();

    // 태스크 생성
    xTaskCreate(vTask1, "Task 1", 100, NULL, 1, NULL);
    xTaskCreate(vTask2, "Task 2", 100, NULL, 2, NULL);

    // 스케줄러 시작
    vTaskStartScheduler();

    while (1) {
        // 메인 루프
    }
}

void vTask1(void *pvParameters) {
    while (1) {
        // Task 1 실행 코드
        if (xSemaphoreTake(xSemaphore, (TickType_t)10) == pdTRUE) {
            // 세마포어를 획득하면 실행되는 코드
            vTaskDelay(pdMS_TO_TICKS(1000)); // 1초 지연
            xSemaphoreGive(xSemaphore); // 세마포어 반환
        }
    }
}

void vTask2(void *pvParameters) {
    while (1) {
        // Task 2 실행 코드
        xSemaphoreGive(xSemaphore); // 세마포어 설정
        vTaskDelay(pdMS_TO_TICKS(500)); // 0.5초 지연
    }
}
```

##### FreeRTOS의 주요 API

1. **xTaskCreate**: 새로운 태스크를 생성합니다.
2. **vTaskDelete**: 태스크를 삭제합니다.
3. **vTaskDelay**: 지정된 시간 동안 태스크를 지연시킵니다.
4. **xSemaphoreCreateBinary**: 이진 세마포어를 생성합니다.
5. **xSemaphoreTake**: 세마포어를 획득합니다.
6. **xSemaphoreGive**: 세마포어를 설정합니다.
7. **xQueueCreate**: 큐를 생성합니다.
8. **xQueueSend**: 큐에 데이터를 보냅니다.
9. **xQueueReceive**: 큐에서 데이터를 수신합니다.

FreeRTOS는 임베디드 시스템에서 실시간 성능을 요구하는 애플리케이션을 효과적으로 관리하고 실행하기 위한 강력한 도구입니다. 각 프로젝트의 요구 사항에 맞게 FreeRTOS의 기능을 잘 활용하여 안정적이고 효율적인 시스템을 구현하는 것이 중요합니다.
