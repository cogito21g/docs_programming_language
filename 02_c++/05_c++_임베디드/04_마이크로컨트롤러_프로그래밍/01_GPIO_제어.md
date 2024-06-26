### 4. 마이크로컨트롤러 프로그래밍

#### 4.1. GPIO 제어

GPIO(General Purpose Input/Output)는 마이크로컨트롤러에서 가장 기본적이고 중요한 기능 중 하나로, 다양한 주변 장치와의 인터페이스를 제공합니다. GPIO 핀은 입력 또는 출력 모드로 설정할 수 있으며, 디지털 신호를 처리할 수 있습니다.

##### GPIO 핀 설정

1. **핀 모드 설정**: GPIO 핀을 입력 또는 출력 모드로 설정합니다.
2. **핀 상태 설정**: 출력 모드일 때 핀의 상태(높음 또는 낮음)를 설정합니다.
3. **핀 상태 읽기**: 입력 모드일 때 핀의 상태를 읽습니다.

##### 예제: STM32 마이크로컨트롤러에서 GPIO 제어

**1. STM32CubeMX 설정**

STM32CubeMX는 STM32 마이크로컨트롤러의 설정과 초기화 코드를 생성하는 도구입니다. 이를 사용하여 GPIO 핀을 설정합니다.

- **프로젝트 생성**: STM32CubeMX를 실행하고 새로운 프로젝트를 생성합니다.
- **핀 설정**: 원하는 GPIO 핀을 선택하고, 모드를 설정합니다(예: 출력 모드).
- **코드 생성**: 설정이 완료되면, "Project" -> "Generate Code"를 선택하여 초기화 코드를 생성합니다.

**2. 코드 작성**

STM32CubeMX에서 생성된 프로젝트에서 main.c 파일을 열고, GPIO 핀을 제어하는 코드를 작성합니다.

**main.c 예제 코드**:
```c
#include "main.h"

void SystemClock_Config(void);
static void MX_GPIO_Init(void);

int main(void) {
    HAL_Init();
    SystemClock_Config();
    MX_GPIO_Init();

    while (1) {
        HAL_GPIO_TogglePin(GPIOA, GPIO_PIN_5); // PA5 핀 상태 토글
        HAL_Delay(1000); // 1초 대기
    }
}

void SystemClock_Config(void) {
    // 시스템 클럭 설정 코드 (생략)
}

static void MX_GPIO_Init(void) {
    GPIO_InitTypeDef GPIO_InitStruct = {0};

    __HAL_RCC_GPIOA_CLK_ENABLE(); // GPIOA 클럭 활성화

    // PA5 핀을 출력 모드로 설정
    GPIO_InitStruct.Pin = GPIO_PIN_5;
    GPIO_InitStruct.Mode = GPIO_MODE_OUTPUT_PP;
    GPIO_InitStruct.Pull = GPIO_NOPULL;
    GPIO_InitStruct.Speed = GPIO_SPEED_FREQ_LOW;
    HAL_GPIO_Init(GPIOA, &GPIO_InitStruct);
}
```

##### 예제: Arduino에서 GPIO 제어

Arduino는 GPIO 제어가 매우 간단합니다. Arduino IDE를 사용하여 코드를 작성하고 업로드합니다.

**예제 코드**:
```cpp
const int ledPin = 13; // LED가 연결된 핀 번호

void setup() {
    pinMode(ledPin, OUTPUT); // 핀을 출력 모드로 설정
}

void loop() {
    digitalWrite(ledPin, HIGH); // LED 켜기
    delay(1000); // 1초 대기
    digitalWrite(ledPin, LOW); // LED 끄기
    delay(1000); // 1초 대기
}
```

##### 예제: AVR 마이크로컨트롤러에서 GPIO 제어

AVR 마이크로컨트롤러는 직접 레지스터를 제어하여 GPIO 핀을 설정하고 제어할 수 있습니다.

**예제 코드**:
```c
#include <avr/io.h>
#include <util/delay.h>

int main(void) {
    DDRB |= (1 << PB0); // PB0 핀을 출력 모드로 설정

    while (1) {
        PORTB |= (1 << PB0); // PB0 핀을 HIGH로 설정
        _delay_ms(1000); // 1초 대기
        PORTB &= ~(1 << PB0); // PB0 핀을 LOW로 설정
        _delay_ms(1000); // 1초 대기
    }

    return 0;
}
```

##### GPIO 제어의 주요 사항

1. **핀 모드 설정**: 각 핀은 입력 또는 출력 모드로 설정해야 합니다.
2. **풀업/풀다운 저항**: 입력 모드일 때, 핀이 공중 부양 상태(floating)가 되지 않도록 풀업 또는 풀다운 저항을 설정합니다.
3. **핀 속도 설정**: 출력 모드일 때, 핀의 스위칭 속도를 설정할 수 있습니다. 고속 모드는 전력 소모가 증가할 수 있습니다.
4. **인터럽트 설정**: 일부 GPIO 핀은 외부 인터럽트 소스로 사용할 수 있으며, 이를 통해 핀의 상태 변화에 반응할 수 있습니다.

GPIO 제어는 임베디드 시스템에서 다양한 주변 장치와 상호작용하는 기본적인 방법입니다. 각 마이크로컨트롤러는 고유한 레지스터와 설정 방식을 가지고 있으므로, 해당 마이크로컨트롤러의 데이터시트를 참조하여 정확한 설정 방법을 이해하는 것이 중요합니다.
