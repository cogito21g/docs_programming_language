#### 4.5. ADC/DAC (아날로그-디지털 변환 / 디지털-아날로그 변환)

ADC(Analog-to-Digital Converter)와 DAC(Digital-to-Analog Converter)는 임베디드 시스템에서 아날로그 신호와 디지털 신호 간의 변환을 담당하는 중요한 기능입니다. 이 변환을 통해 마이크로컨트롤러가 아날로그 센서 데이터를 처리하거나 디지털 데이터를 아날로그 신호로 변환하여 출력할 수 있습니다.

##### ADC (Analog-to-Digital Converter)

ADC는 아날로그 신호를 디지털 값으로 변환하는 장치입니다. 예를 들어, 온도 센서, 광 센서 등의 아날로그 신호를 마이크로컨트롤러가 처리할 수 있는 디지털 값으로 변환합니다.

**특징**:
- **해상도**: ADC의 해상도는 변환된 디지털 값의 비트 수로 표현됩니다(예: 10비트, 12비트). 해상도가 높을수록 더 정밀한 변환이 가능합니다.
- **참조 전압**: ADC는 참조 전압을 기준으로 아날로그 신호를 변환합니다. 예를 들어, 3.3V 참조 전압을 사용하는 10비트 ADC는 0V에서 3.3V 사이의 아날로그 신호를 0에서 1023 사이의 디지털 값으로 변환합니다.
- **샘플링 속도**: ADC의 샘플링 속도는 초당 변환 가능한 샘플 수를 나타냅니다. 샘플링 속도가 높을수록 빠른 신호를 정확하게 변환할 수 있습니다.

**예제: STM32에서 ADC 사용**:
```c
#include "main.h"

ADC_HandleTypeDef hadc1;

void SystemClock_Config(void);
static void MX_GPIO_Init(void);
static void MX_ADC1_Init(void);

int main(void) {
    HAL_Init();
    SystemClock_Config();
    MX_GPIO_Init();
    MX_ADC1_Init();

    HAL_ADC_Start(&hadc1);
    if (HAL_ADC_PollForConversion(&hadc1, HAL_MAX_DELAY) == HAL_OK) {
        uint32_t adcValue = HAL_ADC_GetValue(&hadc1);
        // adcValue를 사용하여 필요한 작업 수행
    }
    HAL_ADC_Stop(&hadc1);

    while (1) {
        // 메인 루프
    }
}

static void MX_ADC1_Init(void) {
    ADC_ChannelConfTypeDef sConfig = {0};

    hadc1.Instance = ADC1;
    hadc1.Init.ClockPrescaler = ADC_CLOCK_SYNC_PCLK_DIV4;
    hadc1.Init.Resolution = ADC_RESOLUTION_12B;
    hadc1.Init.ScanConvMode = DISABLE;
    hadc1.Init.ContinuousConvMode = DISABLE;
    hadc1.Init.DiscontinuousConvMode = DISABLE;
    hadc1.Init.ExternalTrigConvEdge = ADC_EXTERNALTRIGCONVEDGE_NONE;
    hadc1.Init.ExternalTrigConv = ADC_SOFTWARE_START;
    hadc1.Init.DataAlign = ADC_DATAALIGN_RIGHT;
    hadc1.Init.NbrOfConversion = 1;
    hadc1.Init.DMAContinuousRequests = DISABLE;
    hadc1.Init.EOCSelection = ADC_EOC_SINGLE_CONV;
    HAL_ADC_Init(&hadc1);

    sConfig.Channel = ADC_CHANNEL_0;
    sConfig.Rank = 1;
    sConfig.SamplingTime = ADC_SAMPLETIME_3CYCLES;
    HAL_ADC_ConfigChannel(&hadc1, &sConfig);
}
```

##### DAC (Digital-to-Analog Converter)

DAC는 디지털 신호를 아날로그 신호로 변환하는 장치입니다. 예를 들어, 오디오 신호 생성, 모터 제어, 조명 제어 등에 사용됩니다.

**특징**:
- **해상도**: DAC의 해상도는 변환된 아날로그 값의 정밀도를 나타냅니다(예: 8비트, 12비트). 해상도가 높을수록 더 정밀한 아날로그 신호를 생성할 수 있습니다.
- **출력 전압 범위**: DAC는 지정된 출력 전압 범위 내에서 아날로그 신호를 생성합니다.
- **변환 속도**: DAC의 변환 속도는 초당 변환 가능한 값의 수를 나타냅니다. 빠른 변환 속도가 필요한 경우 고속 DAC를 사용해야 합니다.

**예제: STM32에서 DAC 사용**:
```c
#include "main.h"

DAC_HandleTypeDef hdac;

void SystemClock_Config(void);
static void MX_GPIO_Init(void);
static void MX_DAC_Init(void);

int main(void) {
    HAL_Init();
    SystemClock_Config();
    MX_GPIO_Init();
    MX_DAC_Init();

    HAL_DAC_Start(&hdac, DAC_CHANNEL_1);
    HAL_DAC_SetValue(&hdac, DAC_CHANNEL_1, DAC_ALIGN_12B_R, 2048); // 12비트 값 설정

    while (1) {
        // 메인 루프
    }
}

static void MX_DAC_Init(void) {
    DAC_ChannelConfTypeDef sConfig = {0};

    hdac.Instance = DAC;
    HAL_DAC_Init(&hdac);

    sConfig.DAC_Trigger = DAC_TRIGGER_NONE;
    sConfig.DAC_OutputBuffer = DAC_OUTPUTBUFFER_ENABLE;
    HAL_DAC_ConfigChannel(&hdac, &sConfig, DAC_CHANNEL_1);
}
```

##### 예제: Arduino에서 ADC 및 DAC 사용

Arduino는 내장 ADC를 사용하여 아날로그 입력을 디지털 값으로 변환할 수 있으며, 아날로그 출력(PWM 신호)를 사용하여 DAC와 유사한 기능을 구현할 수 있습니다.

**ADC 예제 코드**:
```cpp
const int analogPin = A0;

void setup() {
    Serial.begin(9600);
}

void loop() {
    int adcValue = analogRead(analogPin); // 아날로그 입력 읽기
    Serial.println(adcValue); // ADC 값 출력
    delay(1000);
}
```

**DAC 예제 코드**:
```cpp
const int pwmPin = 9;

void setup() {
    pinMode(pwmPin, OUTPUT);
}

void loop() {
    for (int i = 0; i < 256; i++) {
        analogWrite(pwmPin, i); // PWM 출력 설정
        delay(10);
    }
}
```

##### ADC/DAC 설정의 주요 사항

1. **참조 전압 설정**: ADC의 참조 전압을 설정하여 변환 범위를 정의합니다.
2. **해상도 선택**: 필요한 변환 정밀도에 따라 ADC/DAC의 해상도를 선택합니다.
3. **샘플링 속도**: ADC의 샘플링 속도를 설정하여 신호의 주파수 특성에 맞게 데이터를 수집합니다.
4. **연속 변환 모드**: ADC가 연속적으로 변환을 수행할 수 있도록 설정할 수 있습니다.
5. **DMA 사용**: 많은 양의 데이터를 효율적으로 처리하기 위해 DMA(Direct Memory Access)를 사용할 수 있습니다.

ADC와 DAC는 임베디드 시스템에서 아날로그 신호와 디지털 신호 간의 변환을 효율적으로 수행하여 다양한 애플리케이션에서 필수적인 역할을 합니다. 각 마이크로컨트롤러의 데이터시트를 참조하여 정확한 설정 방법을 이해하고, 적절히 활용하는 것이 중요합니다.
