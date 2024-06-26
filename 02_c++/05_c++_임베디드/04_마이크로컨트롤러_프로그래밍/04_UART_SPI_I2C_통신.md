#### 4.4. UART, SPI, I2C 통신

임베디드 시스템에서 마이크로컨트롤러와 주변 장치 간의 데이터 통신은 매우 중요합니다. UART, SPI, I2C는 가장 널리 사용되는 세 가지 통신 프로토콜로, 각기 다른 용도와 특징을 가지고 있습니다.

##### UART (Universal Asynchronous Receiver/Transmitter)

UART는 직렬 통신 프로토콜로, 두 장치 간의 데이터 전송에 사용됩니다. 비동기 방식으로 동작하며, 스타트 비트, 데이터 비트, 패리티 비트, 스톱 비트를 사용하여 데이터를 전송합니다.

**특징**:
- **비동기 통신**: 클럭 신호가 필요하지 않습니다.
- **전송 속도**: 보드레이트(Baud Rate)로 설정됩니다.
- **풀 듀플렉스**: 동시에 송신과 수신이 가능합니다.

**예제: STM32에서 UART 사용**:
```c
#include "main.h"

UART_HandleTypeDef huart2;

void SystemClock_Config(void);
static void MX_GPIO_Init(void);
static void MX_USART2_UART_Init(void);

int main(void) {
    HAL_Init();
    SystemClock_Config();
    MX_GPIO_Init();
    MX_USART2_UART_Init();

    char *msg = "Hello UART!\r\n";
    HAL_UART_Transmit(&huart2, (uint8_t*)msg, strlen(msg), HAL_MAX_DELAY);

    while (1) {
        // 메인 루프
    }
}

static void MX_USART2_UART_Init(void) {
    huart2.Instance = USART2;
    huart2.Init.BaudRate = 115200;
    huart2.Init.WordLength = UART_WORDLENGTH_8B;
    huart2.Init.StopBits = UART_STOPBITS_1;
    huart2.Init.Parity = UART_PARITY_NONE;
    huart2.Init.Mode = UART_MODE_TX_RX;
    huart2.Init.HwFlowCtl = UART_HWCONTROL_NONE;
    huart2.Init.OverSampling = UART_OVERSAMPLING_16;
    HAL_UART_Init(&huart2);
}
```

##### SPI (Serial Peripheral Interface)

SPI는 동기식 직렬 통신 프로토콜로, 마스터-슬레이브 방식으로 동작합니다. 클럭 신호(SCK), 데이터 출력(MOSI), 데이터 입력(MISO), 슬레이브 선택(SS) 핀을 사용합니다.

**특징**:
- **동기식 통신**: 클럭 신호가 필요합니다.
- **빠른 데이터 전송 속도**: 고속 데이터 전송이 가능합니다.
- **다중 슬레이브 지원**: 하나의 마스터가 여러 슬레이브와 통신할 수 있습니다.

**예제: STM32에서 SPI 사용**:
```c
#include "main.h"

SPI_HandleTypeDef hspi1;

void SystemClock_Config(void);
static void MX_GPIO_Init(void);
static void MX_SPI1_Init(void);

int main(void) {
    HAL_Init();
    SystemClock_Config();
    MX_GPIO_Init();
    MX_SPI1_Init();

    uint8_t txData[] = {0xAA, 0xBB, 0xCC};
    uint8_t rxData[3] = {0};

    HAL_SPI_TransmitReceive(&hspi1, txData, rxData, sizeof(txData), HAL_MAX_DELAY);

    while (1) {
        // 메인 루프
    }
}

static void MX_SPI1_Init(void) {
    hspi1.Instance = SPI1;
    hspi1.Init.Mode = SPI_MODE_MASTER;
    hspi1.Init.Direction = SPI_DIRECTION_2LINES;
    hspi1.Init.DataSize = SPI_DATASIZE_8BIT;
    hspi1.Init.CLKPolarity = SPI_POLARITY_LOW;
    hspi1.Init.CLKPhase = SPI_PHASE_1EDGE;
    hspi1.Init.NSS = SPI_NSS_SOFT;
    hspi1.Init.BaudRatePrescaler = SPI_BAUDRATEPRESCALER_16;
    hspi1.Init.FirstBit = SPI_FIRSTBIT_MSB;
    hspi1.Init.TIMode = SPI_TIMODE_DISABLE;
    hspi1.Init.CRCCalculation = SPI_CRCCALCULATION_DISABLE;
    hspi1.Init.CRCPolynomial = 10;
    HAL_SPI_Init(&hspi1);
}
```

##### I2C (Inter-Integrated Circuit)

I2C는 동기식 직렬 통신 프로토콜로, 멀티마스터-멀티슬레이브 구조를 지원합니다. 클럭 신호(SCL)와 데이터 신호(SDA) 두 개의 라인을 사용합니다.

**특징**:
- **동기식 통신**: 클럭 신호가 필요합니다.
- **멀티마스터-멀티슬레이브 지원**: 여러 마스터와 슬레이브 간의 통신이 가능합니다.
- **슬로우 데이터 전송 속도**: 상대적으로 SPI보다 느린 전송 속도를 가집니다.

**예제: STM32에서 I2C 사용**:
```c
#include "main.h"

I2C_HandleTypeDef hi2c1;

void SystemClock_Config(void);
static void MX_GPIO_Init(void);
static void MX_I2C1_Init(void);

int main(void) {
    HAL_Init();
    SystemClock_Config();
    MX_GPIO_Init();
    MX_I2C1_Init();

    uint8_t txData[] = {0xAA, 0xBB, 0xCC};
    HAL_I2C_Master_Transmit(&hi2c1, (uint16_t)0xA0, txData, sizeof(txData), HAL_MAX_DELAY);

    while (1) {
        // 메인 루프
    }
}

static void MX_I2C1_Init(void) {
    hi2c1.Instance = I2C1;
    hi2c1.Init.ClockSpeed = 100000;
    hi2c1.Init.DutyCycle = I2C_DUTYCYCLE_2;
    hi2c1.Init.OwnAddress1 = 0;
    hi2c1.Init.AddressingMode = I2C_ADDRESSINGMODE_7BIT;
    hi2c1.Init.DualAddressMode = I2C_DUALADDRESS_DISABLE;
    hi2c1.Init.OwnAddress2 = 0;
    hi2c1.Init.GeneralCallMode = I2C_GENERALCALL_DISABLE;
    hi2c1.Init.NoStretchMode = I2C_NOSTRETCH_DISABLE;
    HAL_I2C_Init(&hi2c1);
}
```

##### 비교 요약

| 특성             | UART                             | SPI                              | I2C                             |
|------------------|----------------------------------|----------------------------------|---------------------------------|
| **통신 방식**    | 비동기                           | 동기                             | 동기                            |
| **클럭 신호**    | 필요 없음                        | 필요                             | 필요                            |
| **데이터 라인**  | 1(데이터)                        | 2(데이터 입력, 데이터 출력)      | 1(데이터)                       |
| **슬레이브 지원**| 점대점                           | 다중 슬레이브 지원               | 멀티마스터-멀티슬레이브 지원    |
| **속도**         | 중간                             | 빠름                             | 느림                            |
| **복잡도**       | 낮음                             | 중간                             | 높음                            |

UART, SPI, I2C는 각각의 특성과 용도를 가지고 있으며, 프로젝트의 요구 사항에 따라 적절한 통신 프로토콜을 선택하여 사용할 수 있습니다. 각 프로토콜의 동작 원리와 설정 방법을 잘 이해하고, 적절히 활용하는 것이 중요합니다.
