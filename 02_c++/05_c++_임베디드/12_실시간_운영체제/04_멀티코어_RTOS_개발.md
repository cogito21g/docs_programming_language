#### 12.4. 멀티코어 RTOS 개발 및 활용

멀티코어 RTOS(Real-Time Operating System)는 다중 프로세서 코어를 활용하여 성능과 효율성을 극대화하는 운영체제입니다. 멀티코어 RTOS는 실시간 성능을 보장하면서 여러 태스크를 병렬로 처리할 수 있습니다. 여기서는 멀티코어 RTOS의 기본 개념과 설계, 주요 기술을 설명합니다.

##### 멀티코어 RTOS의 기본 개념

1. **멀티코어 아키텍처**:
   - 여러 개의 독립적인 프로세서 코어를 포함한 시스템입니다.
   - 각 코어는 별도의 명령어와 데이터를 처리할 수 있습니다.

2. **대칭 멀티프로세싱(Symmetric Multiprocessing, SMP)**:
   - 모든 코어가 동일한 메모리 공간을 공유하며, 동일한 역할을 수행합니다.
   - 태스크는 어떤 코어에서든 실행될 수 있습니다.

3. **비대칭 멀티프로세싱(Asymmetric Multiprocessing, AMP)**:
   - 각 코어가 특정한 역할을 수행하며, 독립적인 메모리 공간을 가질 수 있습니다.
   - 태스크는 지정된 코어에서만 실행됩니다.

##### 멀티코어 RTOS 설계

1. **태스크 스케줄링**:
   - 멀티코어 RTOS는 각 코어에서 실행될 태스크를 스케줄링합니다.
   - SMP 시스템에서는 전역 스케줄러를 사용하여 태스크를 모든 코어에 할당합니다.
   - AMP 시스템에서는 각 코어가 독립적인 스케줄러를 가집니다.

2. **컨텍스트 스위칭**:
   - 각 코어에서 독립적으로 컨텍스트 스위칭을 수행합니다.
   - 스케줄러는 태스크의 상태와 컨텍스트를 관리합니다.

3. **동기화 및 통신**:
   - 멀티코어 시스템에서는 코어 간 동기화와 통신이 필요합니다.
   - 세마포어, 뮤텍스, 큐 등을 통해 코어 간 동기화와 통신을 지원합니다.

4. **메모리 관리**:
   - 모든 코어가 동일한 메모리 공간을 공유하므로, 메모리 관리가 중요합니다.
   - 캐시 일관성 유지, 메모리 보호, 메모리 할당 등의 기능이 필요합니다.

##### 멀티코어 RTOS 예제

간단한 SMP RTOS를 설계하여 멀티코어 시스템에서 태스크를 관리하는 방법을 설명합니다.

**멀티코어 RTOS 설계 예제**:
```c
#include <stdint.h>
#include <stddef.h>

#define MAX_TASKS 10
#define STACK_SIZE 256
#define CORE_COUNT 2

typedef enum {
    TASK_READY,
    TASK_RUNNING,
    TASK_WAITING
} task_state_t;

typedef struct {
    uint32_t *stack_ptr;
    uint32_t *stack_base;
    task_state_t state;
    uint8_t priority;
    void (*task_func)(void);
} tcb_t;

tcb_t task_list[MAX_TASKS];
tcb_t *current_task[CORE_COUNT] = {NULL, NULL};
uint8_t task_count = 0;

void os_init(void);
void os_start(void);
void os_create_task(void (*task_func)(void), uint8_t priority);
void os_scheduler(uint8_t core_id);
void os_tick_handler(uint8_t core_id);

void os_init(void) {
    task_count = 0;
}

void os_start(void) {
    // 각 코어에서 스케줄러 실행
    for (uint8_t i = 0; i < CORE_COUNT; i++) {
        os_scheduler(i);
    }
}

void os_create_task(void (*task_func)(void), uint8_t priority) {
    if (task_count < MAX_TASKS) {
        tcb_t *tcb = &task_list[task_count++];
        tcb->stack_base = malloc(STACK_SIZE * sizeof(uint32_t));
        tcb->stack_ptr = tcb->stack_base + STACK_SIZE - 1;
        tcb->state = TASK_READY;
        tcb->priority = priority;
        tcb->task_func = task_func;
    }
}

void os_scheduler(uint8_t core_id) {
    while (1) {
        uint8_t highest_priority = 0xFF;
        tcb_t *next_task = NULL;

        for (uint8_t i = 0; i < task_count; i++) {
            if (task_list[i].state == TASK_READY && task_list[i].priority < highest_priority) {
                highest_priority = task_list[i].priority;
                next_task = &task_list[i];
            }
        }

        if (next_task) {
            if (current_task[core_id]) {
                current_task[core_id]->stack_ptr = (uint32_t *)__get_PSP();
                current_task[core_id]->state = TASK_READY;
            }

            current_task[core_id] = next_task;
            current_task[core_id]->state = TASK_RUNNING;
            __set_PSP((uint32_t)current_task[core_id]->stack_ptr);
            current_task[core_id]->task_func();
        }
    }
}

void os_tick_handler(uint8_t core_id) {
    if (current_task[core_id]) {
        current_task[core_id]->stack_ptr = (uint32_t *)__get_PSP();
    }
    os_scheduler(core_id);
}

void SysTick_Handler(void) {
    // 각 코어에서 시스템 틱 핸들러 호출
    for (uint8_t i = 0; i < CORE_COUNT; i++) {
        os_tick_handler(i);
    }
}

void task1(void) {
    while (1) {
        // 태스크 1 코드
    }
}

void task2(void) {
    while (1) {
        // 태스크 2 코드
    }
}

int main(void) {
    os_init();
    os_create_task(task1, 1);
    os_create_task(task2, 2);
    os_start();
    while (1) {
        // 메인 루프
    }
    return 0;
}
```

##### 코드 설명

1. **기본 구조 설계**:
   - `tcb_t`: 태스크 컨트롤 블록(TCB)은 각 태스크의 상태, 스택 포인터, 우선순위 등을 포함합니다.
   - `task_list`: 최대 `MAX_TASKS`개의 태스크를 저장할 수 있는 태스크 리스트입니다.
   - `current_task`: 각 코어에서 현재 실행 중인 태스크를 가리킵니다.
   - `task_count`: 생성된 태스크의 수를 저장합니다.

2. **태스크 관리 및 스케줄러 구현**:
   - `os_init()`: RTOS 초기화 함수로, 태스크 수를 0으로 초기화합니다.
   - `os_start()`: RTOS 시작 함수로, 각 코어에서 스케줄러를 실행합니다.
   - `os_create_task()`: 새로운 태스크를 생성하는 함수로, TCB를 초기화하고 태스크 리스트에 추가합니다.
   - `os_scheduler()`: 각 코어에서 실행 가능한 태스크 중 가장 높은 우선순위를 가진 태스크를 찾아 실행합니다.

3. **컨텍스트 스위칭 구현**:
   - `os_tick_handler()`: 시스템 틱 인터럽트 핸들러로, 현재 태스크의 스택 포인터를 저장하고 스케줄러를 호출합니다.
   - `SysTick_Handler()`: 시스템 틱 인터럽트 서비스 루틴(ISR)으로, 각 코어에서 `os_tick_handler()`를 호출합니다.

4. **태스크 예제**:
   - `task1`과 `task2`는 각각의 태스크 함수로, RTOS에 의해 스케줄링됩니다.

##### 멀티코어 RTOS의 이점

1. **성능 향상**:
   - 멀티코어 RTOS는 여러 코어를 활용하여 성능을 극대화할 수 있습니다.
   - 병렬 처리를 통해 태스크의 실행 시간을 단축할 수 있습니다.

2. **효율적인 자원 관리**:
   - 멀티코어 시스템에서 자원을 효율적으로 관리하여 시스템 효율성을 높일 수 있습니다.

3. **확장성**:
   - 멀티코어 RTOS는 시스템 확장을 용이하게 하며, 추가적인 코어를 통해 더 많은 태스크를 처리할 수 있습니다.

4. **신뢰성 및 안정성**:
   - 멀티코어 시스템은 하나의 코어에 문제가 발생해도 다른 코어에서 계속해서 작업을 수행할 수 있어 시스템의 신뢰성과 안정성을 높일 수 있습니다.

멀티코어 RTOS는 다양한 실시간 애플리케이션에서 성능과 효율성을 극대화하는 데 중요한 역할을 합니다. 멀티코어 아키텍처와 고급 스케줄링 기법을 이해하고 활용하여 효과적인 멀티코어 시스템을 설계할 수 있습니다.
