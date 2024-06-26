#### 9.3. RTOS를 활용한 다중 태스크 제어 프로젝트

실시간 운영체제(RTOS, Real-Time Operating System)는 임베디드 시스템에서 다중 태스크를 효율적으로 관리하고 실행하기 위해 사용됩니다. RTOS는 태스크 스케줄링, 동기화, 인터럽트 관리 등의 기능을 제공하여 시스템의 성능과 응답성을 향상시킵니다. 여기서는 FreeRTOS를 사용하여 다중 태스크를 제어하는 방법을 설명합니다.

##### 프로젝트 개요

이 프로젝트에서는 FreeRTOS를 사용하여 두 개의 태스크를 실행합니다. 첫 번째 태스크는 주기적으로 LED를 깜박이게 하고, 두 번째 태스크는 주기적으로 시리얼 모니터에 메시지를 출력합니다.

##### 필요한 부품

- Arduino 보드 (예: Arduino Uno, ESP32)
- LED와 저항
- 점퍼 와이어
- 브레드보드 (선택 사항)

##### 하드웨어 연결

1. **LED 핀 구성**:
   - LED의 긴 다리(양극)를 저항을 통해 Arduino의 디지털 핀 (예: D13) 에 연결합니다.
   - LED의 짧은 다리(음극)를 Arduino의 GND 핀에 연결합니다.

##### 소프트웨어 설정

1. **Arduino IDE 설정**:
   - Arduino IDE를 설치하고 실행합니다.
   - FreeRTOS 라이브러리를 설치합니다. (Sketch > Include Library > Manage Libraries > FreeRTOS 검색 후 설치)

2. **코드 작성**:
   - FreeRTOS를 사용하여 두 개의 태스크를 실행하는 코드를 작성합니다.

**예제 코드**:
```cpp
#include <Arduino_FreeRTOS.h>

// LED 핀 정의
#define LED_PIN 13

void setup() {
    // LED 핀 설정
    pinMode(LED_PIN, OUTPUT);

    // 태스크 생성
    xTaskCreate(blinkTask, "Blink", 128, NULL, 1, NULL);
    xTaskCreate(messageTask, "Message", 128, NULL, 1, NULL);

    // FreeRTOS 스케줄러 시작
    vTaskStartScheduler();
}

void loop() {
    // FreeRTOS에서는 loop()를 사용하지 않습니다.
}

// LED를 주기적으로 깜박이는 태스크
void blinkTask(void *pvParameters) {
    (void) pvParameters;

    for (;;) {
        digitalWrite(LED_PIN, HIGH);
        vTaskDelay(500 / portTICK_PERIOD_MS); // 500ms 대기
        digitalWrite(LED_PIN, LOW);
        vTaskDelay(500 / portTICK_PERIOD_MS); // 500ms 대기
    }
}

// 시리얼 모니터에 메시지를 출력하는 태스크
void messageTask(void *pvParameters) {
    (void) pvParameters;

    for (;;) {
        Serial.println("Hello from FreeRTOS!");
        vTaskDelay(1000 / portTICK_PERIOD_MS); // 1초 대기
    }
}
```

##### 코드 설명

1. **라이브러리 포함 및 핀 정의**:
   - `#include <Arduino_FreeRTOS.h>`: FreeRTOS 라이브러리를 포함합니다.
   - `#define LED_PIN 13`: LED 핀을 정의합니다.

2. **설정 함수**:
   - `void setup()`: LED 핀을 출력으로 설정하고, 두 개의 태스크를 생성합니다. 태스크 생성 후 FreeRTOS 스케줄러를 시작합니다.
   - `pinMode(LED_PIN, OUTPUT);`: LED 핀을 출력으로 설정합니다.
   - `xTaskCreate()`: 태스크를 생성합니다.
   - `vTaskStartScheduler()`: FreeRTOS 스케줄러를 시작합니다.

3. **루프 함수**:
   - `void loop()`: FreeRTOS에서는 `loop()` 함수를 사용하지 않습니다.

4. **태스크 함수**:
   - `void blinkTask(void *pvParameters)`: LED를 주기적으로 깜박이게 하는 태스크입니다. 500ms 간격으로 LED를 켜고 끕니다.
   - `void messageTask(void *pvParameters)`: 시리얼 모니터에 주기적으로 메시지를 출력하는 태스크입니다. 1초 간격으로 메시지를 출력합니다.

##### 프로젝트 실행

1. **코드 업로드**:
   - Arduino 보드를 컴퓨터에 연결하고, 작성한 코드를 업로드합니다.

2. **시리얼 모니터 확인**:
   - Arduino IDE에서 시리얼 모니터를 열고, 메시지가 주기적으로 출력되는지 확인합니다.

3. **LED 확인**:
   - LED가 500ms 간격으로 깜박이는지 확인합니다.

##### 확장 가능성

이 프로젝트를 확장하여 더 많은 태스크를 추가하거나, 센서 데이터를 수집하고 처리하는 태스크를 구현할 수 있습니다. 또한, 태스크 간의 동기화와 통신을 위해 큐(queue), 세마포어(semaphore), 뮤텍스(mutex)와 같은 FreeRTOS의 다양한 동기화 기법을 활용할 수 있습니다.
