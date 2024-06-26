##### 11.3. 고속 GPIO 제어

고속 GPIO 제어는 임베디드 시스템에서 빠르고 정확한 신호 처리를 필요로 하는 애플리케이션에서 매우 중요합니다. GPIO를 효율적으로 제어하는 방법을 알아보겠습니다.

##### GPIO 제어의 기본 개념

1. **GPIO란?**:
   - GPIO(General Purpose Input/Output)는 다양한 디지털 신호를 읽거나 쓸 수 있는 다목적 핀입니다.
   - GPIO는 입력 모드와 출력 모드로 설정할 수 있습니다.

2. **GPIO 설정**:
   - GPIO 핀의 모드, 출력 타입, 속도, 풀업/풀다운 저항 등을 설정합니다.

##### STM32에서 고속 GPIO 제어 예제

STM32 마이크로컨트롤러를 사용하여 고속으로 GPIO를 제어하는 방법을 설명합니다.

**GPIO 제어 설정 예제**:
```c
#include "stm32f4xx.h"

void GPIO_Init(void);

int main(void) {
    // 시스템 초기화
    SystemInit();
    
    // GPIO 초기화
    GPIO_Init();

    while (1) {
        // GPIO 토글
        GPIOD->ODR ^= (1 << 12); // PD12 핀 토글
        for (volatile int i = 0; i < 100000; i++); // 딜레이
    }
}

void GPIO_Init(void) {
    // GPIOD 클럭 활성화
    RCC->AHB1ENR |= RCC_AHB1ENR_GPIODEN;

    // PD12를 출력 모드로 설정
    GPIOD->MODER |= (1 << (12 * 2)); // MODER12[1:0] = 01 (출력 모드)
    GPIOD->OTYPER &= ~(1 << 12);     // OTYPER12 = 0 (푸시-풀 출력)
    GPIOD->OSPEEDR |= (3 << (12 * 2)); // OSPEEDR12[1:0] = 11 (고속)
    GPIOD->PUPDR &= ~(3 << (12 * 2)); // PUPDR12[1:0] = 00 (풀업/풀다운 없음)
}
```

##### 코드 설명

1. **시스템 초기화**:
   - `SystemInit()`: 시스템 초기화 함수입니다.

2. **GPIO 초기화**:
   - `RCC->AHB1ENR |= RCC_AHB1ENR_GPIODEN;`: GPIOD 클럭을 활성화합니다.
   - `GPIOD->MODER |= (1 << (12 * 2));`: PD12 핀을 출력 모드로 설정합니다.
   - `GPIOD->OTYPER &= ~(1 << 12);`: PD12 핀을 푸시-풀 출력 타입으로 설정합니다.
   - `GPIOD->OSPEEDR |= (3 << (12 * 2));`: PD12 핀의 속도를 고속으로 설정합니다.
   - `GPIOD->PUPDR &= ~(3 << (12 * 2));`: PD12 핀의 풀업/풀다운 저항을 비활성화합니다.

3. **메인 루프**:
   - `GPIOD->ODR ^= (1 << 12);`: PD12 핀을 토글합니다.
   - `for (volatile int i = 0; i < 100000; i++);`: 딜레이를 추가하여 토글 주기를 조절합니다.

##### 고속 GPIO 제어 기법

1. **비트 밴딩(Bit-Banding)**:
   - 메모리의 특정 비트를 직접 제어하여 GPIO 속도를 높입니다.
   - Cortex-M3/M4에서는 비트 밴딩을 통해 빠른 GPIO 제어가 가능합니다.

   ```c
   #define BITBAND_PERI_BASE 0x42000000
   #define GPIOA_ODR_OFFSET  0x00000014
   #define BITBAND_PERI(addr, bit) ((volatile uint32_t *)(BITBAND_PERI_BASE + ((addr) - 0x40000000) * 32 + (bit) * 4))

   // GPIOA의 ODR 비트0을 비트 밴딩으로 제어
   #define GPIOA_ODR_BIT0 BITBAND_PERI((uint32_t)&GPIOA->ODR, 0)

   int main(void) {
       // 시스템 초기화
       SystemInit();
       
       // GPIO 초기화
       GPIO_Init();

       while (1) {
           *GPIOA_ODR_BIT0 = 1; // GPIOA 핀0을 설정
           *GPIOA_ODR_BIT0 = 0; // GPIOA 핀0을 클리어
       }
   }
   ```

2. **어셈블리 코드 사용**:
   - 고속 제어가 필요한 경우, C 코드 대신 어셈블리 코드를 사용하여 직접 레지스터를 제어합니다.

   ```asm
   .section .text
   .global main

   main:
       /* GPIO 초기화 코드 생략 */
       /* 무한 루프 */
   loop:
       LDR R0, =0x40020014  /* GPIOA ODR 주소 로드 */
       LDR R1, [R0]         /* ODR 레지스터 값 로드 */
       ORR R1, R1, #1       /* 비트0 설정 */
       STR R1, [R0]         /* ODR 레지스터 업데이트 */

       LDR R1, [R0]         /* ODR 레지스터 값 로드 */
       BIC R1, R1, #1       /* 비트0 클리어 */
       STR R1, [R0]         /* ODR 레지스터 업데이트 */
       B loop
   ```

3. **DMA를 사용한 GPIO 제어**:
   - DMA를 사용하여 GPIO 데이터를 자동으로 전송합니다.
   - CPU가 관여하지 않고, 메모리에서 GPIO로 데이터를 전송하여 고속 제어가 가능합니다.

   ```c
   void DMA_Init(void) {
       // DMA 클럭 활성화
       RCC->AHB1ENR |= RCC_AHB1ENR_DMA2EN;

       // DMA 설정
       DMA2_Stream0->CR = DMA_SxCR_PL_1 |    // 높은 우선순위
                          DMA_SxCR_MSIZE_0 | // 메모리 데이터 크기: 16비트
                          DMA_SxCR_PSIZE_0 | // 주변 장치 데이터 크기: 16비트
                          DMA_SxCR_MINC |    // 메모리 주소 증가
                          DMA_SxCR_DIR_0;    // 메모리 -> 주변 장치

       DMA2_Stream0->NDTR = 100;  // 전송 데이터 크기
       DMA2_Stream0->PAR = (uint32_t)&GPIOD->ODR; // 주변 장치 주소
       DMA2_Stream0->M0AR = (uint32_t)data_buffer; // 메모리 주소

       DMA2_Stream0->CR |= DMA_SxCR_EN; // DMA 스트림 활성화
   }
   ```

##### 고속 GPIO 제어의 이점

1. **실시간 성능 향상**:
   - GPIO를 빠르게 제어함으로써 실시간 시스템의 성능을 향상시킬 수 있습니다.

2. **CPU 부하 감소**:
   - DMA 등을 활용하여 CPU의 부하를 줄이고, 다른 작업에 더 많은 리소스를 할당할 수 있습니다.

3. **정확한 타이밍**:
   - 고속 GPIO 제어는 정확한 타이밍이 필요한 애플리케이션에서 유리합니다.

고속 GPIO 제어는 임베디드 시스템에서 중요한 역할을 합니다. 다양한 기법을 적절히 활용하여 효율적인 GPIO 제어를 구현할 수 있습니다.
