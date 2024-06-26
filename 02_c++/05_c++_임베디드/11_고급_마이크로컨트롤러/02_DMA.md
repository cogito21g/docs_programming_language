##### 11.2. DMA(Direct Memory Access)

DMA(Direct Memory Access)는 CPU를 거치지 않고 메모리와 주변 장치 간에 직접 데이터를 전송할 수 있게 하는 기술입니다. DMA를 사용하면 CPU의 부하를 줄이고, 데이터 전송 속도를 향상시킬 수 있습니다. 임베디드 시스템에서는 특히 대용량 데이터 전송 시 효율성을 높이는 데 유용합니다.

##### DMA의 기본 개념

1. **DMA 컨트롤러**:
   - DMA 컨트롤러는 DMA 전송을 관리하는 하드웨어 모듈입니다. 일반적으로 MCU 내에 포함되어 있습니다.
   - DMA 요청을 수신하고, 메모리 주소와 데이터 크기를 설정하여 전송을 수행합니다.

2. **DMA 채널**:
   - DMA 컨트롤러는 여러 개의 DMA 채널을 지원할 수 있습니다.
   - 각 채널은 독립적으로 설정할 수 있으며, 특정 주변 장치나 메모리 영역과 연결됩니다.

3. **DMA 전송 모드**:
   - **메모리-메모리 전송**: 메모리 간의 데이터 전송.
   - **주변 장치-메모리 전송**: 주변 장치에서 메모리로 데이터 전송.
   - **메모리-주변 장치 전송**: 메모리에서 주변 장치로 데이터 전송.

##### DMA 설정 과정

DMA를 설정하는 과정은 다음과 같습니다.

1. **DMA 클럭 활성화**:
   - DMA 컨트롤러의 클럭을 활성화합니다.

2. **DMA 채널 설정**:
   - 소스 주소와 목적지 주소를 설정합니다.
   - 데이터 전송 크기와 전송 모드를 설정합니다.

3. **DMA 요청 활성화**:
   - 주변 장치나 타이머 등의 DMA 요청을 활성화합니다.

4. **DMA 전송 시작**:
   - DMA 전송을 시작합니다.

##### STM32에서 DMA 설정 예제

여기서는 STM32 마이크로컨트롤러에서 ADC(Analog-to-Digital Converter) 데이터를 DMA를 사용하여 메모리로 전송하는 예제를 다룹니다.

**DMA를 이용한 ADC 데이터 전송 예제**:
```c
#include "stm32f4xx.h"

#define ADC_BUF_LEN 10
uint16_t adc_buf[ADC_BUF_LEN];

void DMA2_Stream0_IRQHandler(void);

int main(void) {
    // 시스템 초기화
    SystemInit();

    // GPIO 초기화 (ADC1 채널0 -> PA0)
    RCC->AHB1ENR |= RCC_AHB1ENR_GPIOAEN;
    GPIOA->MODER |= GPIO_MODER_MODER0; // PA0을 아날로그 모드로 설정

    // ADC1 초기화
    RCC->APB2ENR |= RCC_APB2ENR_ADC1EN;
    ADC1->CR2 |= ADC_CR2_CONT; // 연속 변환 모드
    ADC1->SQR3 |= (0 << 0); // 채널0 선택
    ADC1->CR2 |= ADC_CR2_DMA | ADC_CR2_DDS; // DMA 모드 설정

    // DMA2 초기화
    RCC->AHB1ENR |= RCC_AHB1ENR_DMA2EN;
    DMA2_Stream0->CR &= ~DMA_SxCR_EN; // DMA 스트림 비활성화
    while (DMA2_Stream0->CR & DMA_SxCR_EN); // 스트림이 비활성화될 때까지 대기

    DMA2_Stream0->CR = DMA_SxCR_PL_1 | // 우선순위 높음
                       DMA_SxCR_MSIZE_0 | // 메모리 데이터 크기: 16비트
                       DMA_SxCR_PSIZE_0 | // 주변 장치 데이터 크기: 16비트
                       DMA_SxCR_MINC | // 메모리 주소 증가
                       DMA_SxCR_CIRC | // 순환 모드
                       DMA_SxCR_TCIE; // 전송 완료 인터럽트 활성화

    DMA2_Stream0->NDTR = ADC_BUF_LEN; // 전송 데이터 크기
    DMA2_Stream0->PAR = (uint32_t)&ADC1->DR; // 주변 장치 주소 (ADC 데이터 레지스터)
    DMA2_Stream0->M0AR = (uint32_t)adc_buf; // 메모리 주소

    DMA2_Stream0->CR |= DMA_SxCR_EN; // DMA 스트림 활성화

    // NVIC 설정 (DMA2 스트림0 인터럽트 활성화)
    NVIC_SetPriority(DMA2_Stream0_IRQn, 0);
    NVIC_EnableIRQ(DMA2_Stream0_IRQn);

    // ADC1 활성화
    ADC1->CR2 |= ADC_CR2_ADON;

    while (1) {
        // 메인 루프
    }
}

void DMA2_Stream0_IRQHandler(void) {
    if (DMA2->LISR & DMA_LISR_TCIF0) {
        DMA2->LIFCR |= DMA_LIFCR_CTCIF0; // 전송 완료 플래그 클리어

        // 데이터 처리 코드
        for (int i = 0; i < ADC_BUF_LEN; i++) {
            // 예: 데이터 출력
            printf("ADC Value[%d]: %d\n", i, adc_buf[i]);
        }
    }
}
```

##### 코드 설명

1. **GPIO 및 ADC 초기화**:
   - `RCC->AHB1ENR |= RCC_AHB1ENR_GPIOAEN;`: GPIOA 클럭을 활성화합니다.
   - `GPIOA->MODER |= GPIO_MODER_MODER0;`: PA0 핀을 아날로그 모드로 설정합니다.
   - `RCC->APB2ENR |= RCC_APB2ENR_ADC1EN;`: ADC1 클럭을 활성화합니다.
   - `ADC1->CR2 |= ADC_CR2_CONT;`: 연속 변환 모드를 설정합니다.
   - `ADC1->SQR3 |= (0 << 0);`: ADC1 채널0을 선택합니다.
   - `ADC1->CR2 |= ADC_CR2_DMA | ADC_CR2_DDS;`: DMA 모드를 설정합니다.

2. **DMA 초기화**:
   - `RCC->AHB1ENR |= RCC_AHB1ENR_DMA2EN;`: DMA2 클럭을 활성화합니다.
   - `DMA2_Stream0->CR &= ~DMA_SxCR_EN;`: DMA 스트림을 비활성화합니다.
   - `DMA2_Stream0->CR`: DMA 스트림 설정 (우선순위, 데이터 크기, 메모리 주소 증가, 순환 모드, 전송 완료 인터럽트 활성화).
   - `DMA2_Stream0->NDTR = ADC_BUF_LEN;`: 전송 데이터 크기를 설정합니다.
   - `DMA2_Stream0->PAR = (uint32_t)&ADC1->DR;`: 주변 장치 주소를 설정합니다 (ADC 데이터 레지스터).
   - `DMA2_Stream0->M0AR = (uint32_t)adc_buf;`: 메모리 주소를 설정합니다.
   - `DMA2_Stream0->CR |= DMA_SxCR_EN;`: DMA 스트림을 활성화합니다.

3. **NVIC 설정**:
   - `NVIC_SetPriority(DMA2_Stream0_IRQn, 0);`: DMA2 스트림0 인터럽트 우선순위를 설정합니다.
   - `NVIC_EnableIRQ(DMA2_Stream0_IRQn);`: DMA2 스트림0 인터럽트를 활성화합니다.

4. **ADC 활성화**:
   - `ADC1->CR2 |= ADC_CR2_ADON;`: ADC1을 활성화합니다.

5. **DMA 인터럽트 핸들러**:
   - `void DMA2_Stream0_IRQHandler(void)`: DMA2 스트림0 인터럽트 핸들러입니다. 전송 완료 시 데이터 처리를 수행합니다.
   - `DMA2->LISR & DMA_LISR_TCIF0`: 전송 완료 플래그를 확인합니다.
   - `DMA2->LIFCR |= DMA_LIFCR_CTCIF0;`: 전송 완료 플래그를 클리어합니다.
   - 데이터 처리 코드: 전송된 ADC 데이터를 출력합니다.

##### DMA의 이점

1. **CPU 부하 감소**:
   - DMA를 사용하면 CPU가 데이터 전송 작업에서 해방되어 다른 작업을 수행할 수 있습니다.

2. **빠른 데이터 전송**:
   - DMA는 메모리와 주변 장치 간의 데이터 전송 속도를 크게 향상시킵니다.

3. **실시간 성능 향상**:
   - DMA는 실시간 시스템에서 중요한 데이터 전송을 효율적으로 처리할 수 있습니다.

DMA는 임베디드 시스템에서 데이터 전송 효율성을 극대화하는 강력한 도구입니다. 적절한 설정과 활용을 통해 시스템 성능을 최적화할 수 있습니다.
