### 12. 실시간 운영체제 심화

#### 12.1. RTOS 커널 구조 분석

RTOS(Real-Time Operating System)는 실시간 성능이 중요한 임베디드 시스템에서 사용되는 운영체제입니다. RTOS는 결정론적인 응답 시간과 자원 관리 기능을 제공하여 실시간 애플리케이션을 지원합니다. 여기서는 RTOS 커널의 기본 구조와 주요 구성 요소를 분석합니다.

##### RTOS 커널의 기본 구성 요소

1. **태스크 관리(Task Management)**:
   - 태스크(Task)는 RTOS에서 실행되는 기본 단위입니다.
   - 태스크는 각각 독립적인 실행 흐름을 가지며, 스케줄링을 통해 실행됩니다.

2. **스케줄러(Scheduler)**:
   - 스케줄러는 어떤 태스크를 언제 실행할지 결정하는 RTOS의 핵심 구성 요소입니다.
   - 선점형(Preemptive) 스케줄링과 비선점형(Non-preemptive) 스케줄링이 있습니다.
   - 우선순위 기반 스케줄링, 라운드 로빈 스케줄링 등 다양한 알고리즘이 사용됩니다.

3. **인터럽트 처리(Interrupt Handling)**:
   - 인터럽트는 외부 이벤트에 대한 즉각적인 응답을 가능하게 합니다.
   - 인터럽트 서비스 루틴(ISR)은 가능한 짧게 유지하고, 긴 작업은 태스크로 넘깁니다.

4. **동기화(Synchronization)**:
   - 여러 태스크가 공유 자원에 접근할 때 충돌을 방지하기 위해 동기화 메커니즘을 제공합니다.
   - 세마포어(Semaphore), 뮤텍스(Mutex), 큐(Queue) 등이 사용됩니다.

5. **메모리 관리(Memory Management)**:
   - RTOS는 효율적인 메모리 관리를 위해 동적 메모리 할당 및 해제를 지원합니다.
   - 고정 크기 블록 할당(Fixed Block Allocation), 가변 크기 블록 할당(Variable Block Allocation) 등 다양한 기법이 사용됩니다.

##### FreeRTOS 커널 구조 예제

FreeRTOS는 널리 사용되는 오픈 소스 RTOS입니다. 여기서는 FreeRTOS의 커널 구조를 예제로 설명합니다.

1. **태스크 관리**:
   - FreeRTOS에서 태스크는 `Task Control Block(TCB)`에 의해 관리됩니다.
   - TCB는 태스크의 상태, 스택 포인터, 우선순위 등을 포함합니다.

   ```c
   typedef struct tskTaskControlBlock {
       volatile StackType_t *pxTopOfStack;
       ListItem_t xStateListItem;
       ListItem_t xEventListItem;
       UBaseType_t uxPriority;
       StackType_t *pxStack;
       char pcTaskName[ configMAX_TASK_NAME_LEN ];
   } tskTCB;
   ```

2. **스케줄러**:
   - FreeRTOS는 우선순위 기반 선점형 스케줄링을 기본으로 사용합니다.
   - `vTaskSwitchContext()` 함수가 스케줄링을 담당하며, 우선순위가 높은 태스크로 컨텍스트 스위칭을 수행합니다.

   ```c
   void vTaskSwitchContext( void ) {
       // 현재 태스크의 TCB 업데이트
       // 다음 실행할 태스크의 TCB 선택
       // 컨텍스트 스위칭 수행
   }
   ```

3. **인터럽트 처리**:
   - FreeRTOS는 인터럽트 안전 함수를 제공하여 ISR에서도 안전하게 RTOS API를 호출할 수 있습니다.
   - `portYIELD_FROM_ISR()` 매크로를 사용하여 ISR 종료 후 스케줄러를 호출합니다.

   ```c
   void vISRHandler( void ) {
       BaseType_t xHigherPriorityTaskWoken = pdFALSE;

       // 인터럽트 처리 코드

       // 인터럽트 종료 후 스케줄러 호출
       portYIELD_FROM_ISR( xHigherPriorityTaskWoken );
   }
   ```

4. **동기화**:
   - FreeRTOS는 세마포어, 뮤텍스, 큐 등을 통해 태스크 간 동기화를 지원합니다.
   - 세마포어 예제:

   ```c
   SemaphoreHandle_t xSemaphore = xSemaphoreCreateBinary();

   void vTask1( void *pvParameters ) {
       for( ;; ) {
           if( xSemaphoreTake( xSemaphore, portMAX_DELAY ) == pdTRUE ) {
               // 공유 자원 접근
           }
       }
   }

   void vTask2( void *pvParameters ) {
       for( ;; ) {
           // 이벤트 발생 시 세마포어를 줍니다.
           xSemaphoreGive( xSemaphore );
           vTaskDelay( 1000 / portTICK_PERIOD_MS );
       }
   }
   ```

5. **메모리 관리**:
   - FreeRTOS는 다양한 메모리 할당 스키마를 제공하여 동적 메모리 관리를 지원합니다.
   - 기본적으로 `pvPortMalloc()`와 `vPortFree()` 함수를 사용합니다.

   ```c
   void *pvMemory = pvPortMalloc( sizeof( MyStruct_t ) );
   if( pvMemory != NULL ) {
       // 메모리 사용
       vPortFree( pvMemory );
   }
   ```

##### RTOS 커널의 동작 원리

RTOS 커널은 주기적인 타이머 인터럽트를 통해 시스템 틱(System Tick)을 발생시키며, 이 틱을 기준으로 스케줄러를 실행합니다. 스케줄러는 현재 실행 중인 태스크를 중단하고, 다음 실행할 태스크를 선택하여 컨텍스트 스위칭을 수행합니다.

1. **시스템 틱**:
   - 시스템 틱은 일정 주기마다 발생하는 타이머 인터럽트입니다.
   - 시스템 틱 인터럽트가 발생하면 `xTaskIncrementTick()` 함수가 호출되어 태스크 상태를 업데이트합니다.

   ```c
   void xTaskIncrementTick( void ) {
       // 시스템 틱 증가
       // 대기 리스트 업데이트
       // 태스크 상태 변경
   }
   ```

2. **컨텍스트 스위칭**:
   - 컨텍스트 스위칭은 현재 실행 중인 태스크의 상태를 저장하고, 다음 실행할 태스크의 상태를 복원하는 과정입니다.
   - FreeRTOS에서는 `portSAVE_CONTEXT()`와 `portRESTORE_CONTEXT()` 매크로를 사용하여 컨텍스트 스위칭을 수행합니다.

   ```c
   void vTaskSwitchContext( void ) {
       // 현재 태스크의 컨텍스트 저장
       portSAVE_CONTEXT();

       // 다음 실행할 태스크 선택
       pxCurrentTCB = pxNextTCB;

       // 선택된 태스크의 컨텍스트 복원
       portRESTORE_CONTEXT();
   }
   ```

##### RTOS의 장점

1. **결정론적 응답 시간**:
   - RTOS는 실시간 성능을 보장하며, 예측 가능한 응답 시간을 제공합니다.

2. **효율적인 자원 관리**:
   - RTOS는 태스크, 메모리, 동기화 자원 등을 효율적으로 관리하여 시스템 성능을 최적화합니다.

3. **모듈성 및 확장성**:
   - RTOS 기반 시스템은 모듈화된 구조를 가지며, 새로운 기능을 쉽게 추가할 수 있습니다.

4. **안정성**:
   - RTOS는 고신뢰성이 요구되는 임베디드 시스템에서 안정적인 운영을 보장합니다.

RTOS 커널 구조를 이해하면 실시간 임베디드 시스템을 보다 효과적으로 설계하고 구현할 수 있습니다. FreeRTOS와 같은 오픈 소스 RTOS를 활용하여 다양한 실시간 애플리케이션을 개발할 수 있습니다.
