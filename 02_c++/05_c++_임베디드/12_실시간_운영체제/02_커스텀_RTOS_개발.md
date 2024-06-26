#### 12.2. 커스텀 RTOS 개발

커스텀 RTOS(Real-Time Operating System) 개발은 특정 애플리케이션 요구 사항에 맞게 최적화된 RTOS를 만드는 과정입니다. 여기서는 간단한 커스텀 RTOS를 개발하는 과정을 단계별로 설명합니다.

##### 커스텀 RTOS 개발의 기본 개념

1. **태스크 관리**:
   - 태스크는 실행 가능한 코드의 단위입니다.
   - 태스크 스케줄링, 상태 관리, 우선순위 등을 포함합니다.

2. **스케줄링**:
   - 스케줄러는 어떤 태스크를 언제 실행할지 결정합니다.
   - 우선순위 기반 스케줄링, 라운드 로빈 스케줄링 등 다양한 스케줄링 알고리즘이 있습니다.

3. **컨텍스트 스위칭**:
   - 태스크 간의 전환을 위해 현재 태스크의 상태를 저장하고, 다음 태스크의 상태를 복원합니다.

4. **동기화 및 통신**:
   - 세마포어, 뮤텍스, 큐 등을 통해 태스크 간 동기화와 통신을 지원합니다.

##### 커스텀 RTOS 개발 단계

1. **기본 구조 설계**:
   - RTOS의 기본 구조와 주요 구성 요소를 설계합니다.

2. **시스템 틱 타이머 설정**:
   - 시스템 틱 타이머를 설정하여 주기적인 인터럽트를 발생시킵니다.

3. **태스크 관리 및 스케줄러 구현**:
   - 태스크를 생성, 삭제, 관리하고, 스케줄러를 구현합니다.

4. **컨텍스트 스위칭 구현**:
   - 태스크 간의 전환을 위한 컨텍스트 스위칭 메커니즘을 구현합니다.

5. **동기화 및 통신 메커니즘 구현**:
   - 세마포어, 뮤텍스, 큐 등을 구현하여 태스크 간 동기화와 통신을 지원합니다.

##### 커스텀 RTOS 예제

간단한 커스텀 RTOS를 예제로 설명합니다. 이 RTOS는 선점형 우선순위 기반 스케줄링을 사용합니다.

**기본 구조 설계**:
- 태스크 컨트롤 블록(TCB)
- 태스크 리스트
- 스케줄러

```c
#include <stdint.h>
#include <stddef.h>

#define MAX_TASKS 5
#define STACK_SIZE 256

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
tcb_t *current_task = NULL;
uint8_t task_count = 0;

void os_init(void);
void os_start(void);
void os_create_task(void (*task_func)(void), uint8_t priority);
void os_scheduler(void);
void os_tick_handler(void);

void os_init(void) {
    task_count = 0;
}

void os_start(void) {
    // 시스템 틱 타이머 설정
    SysTick_Config(SystemCoreClock / 1000); // 1ms 간격
    os_scheduler();
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

void os_scheduler(void) {
    while (1) {
        for (uint8_t i = 0; i < task_count; i++) {
            if (task_list[i].state == TASK_READY) {
                current_task = &task_list[i];
                current_task->state = TASK_RUNNING;
                __set_PSP((uint32_t)current_task->stack_ptr);
                current_task->task_func();
                current_task->state = TASK_READY;
            }
        }
    }
}

void os_tick_handler(void) {
    // 시스템 틱 인터럽트 핸들러
    if (current_task) {
        current_task->stack_ptr = (uint32_t *)__get_PSP();
    }
    os_scheduler();
}

void SysTick_Handler(void) {
    os_tick_handler();
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
   - `current_task`: 현재 실행 중인 태스크를 가리킵니다.
   - `task_count`: 생성된 태스크의 수를 저장합니다.

2. **시스템 틱 타이머 설정**:
   - `SysTick_Config(SystemCoreClock / 1000);`: 시스템 틱 타이머를 1ms 간격으로 설정합니다.

3. **태스크 관리 및 스케줄러 구현**:
   - `os_init()`: RTOS 초기화 함수로, 태스크 수를 0으로 초기화합니다.
   - `os_start()`: RTOS 시작 함수로, 시스템 틱 타이머를 설정하고 스케줄러를 호출합니다.
   - `os_create_task()`: 새로운 태스크를 생성하는 함수로, TCB를 초기화하고 태스크 리스트에 추가합니다.
   - `os_scheduler()`: 스케줄러 함수로, 실행 가능한 태스크를 찾아 실행합니다.

4. **컨텍스트 스위칭 구현**:
   - `os_tick_handler()`: 시스템 틱 인터럽트 핸들러로, 현재 태스크의 스택 포인터를 저장하고 스케줄러를 호출합니다.
   - `SysTick_Handler()`: 시스템 틱 인터럽트 서비스 루틴(ISR)으로, `os_tick_handler()`를 호출합니다.

##### 동기화 및 통신 메커니즘 구현

간단한 세마포어를 구현하여 태스크 간 동기화를 지원할 수 있습니다.

**세마포어 구현 예제**:
```c
typedef struct {
    volatile int count;
} semaphore_t;

void sem_init(semaphore_t *sem, int value) {
    sem->count = value;
}

void sem_wait(semaphore_t *sem) {
    while (sem->count <= 0);
    sem->count--;
}

void sem_signal(semaphore_t *sem) {
    sem->count++;
}

semaphore_t sem;

void task1(void) {
    while (1) {
        sem_wait(&sem);
        // 공유 자원 접근
        sem_signal(&sem);
    }
}

void task2(void) {
    while (1) {
        sem_wait(&sem);
        // 공유 자원 접근
        sem_signal(&sem);
    }
}

int main(void) {
    os_init();
    sem_init(&sem, 1);
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

1. **세마포어 구조체**:
   - `semaphore_t`: 세마포어 구조체로, 카운트를 저장합니다.

2. **세마포어 초기화**:
   - `sem_init()`: 세마포어를 초기화하는 함수입니다.

3. **세마포어 대기 및 신호**:
   - `sem_wait()`: 세마포어 대기 함수로, 카운트가 0보다 클 때까지 대기합니다.
   - `sem_signal()`: 세마포어 신호 함수로, 카운트를 증가시킵니다.

4. **태스크**:
   - `task1`과 `task2`는 세마포어를 사용하여 공유 자원에 접근합니다.

5. **메인 함수**:
   - RTOS를 초기화하고 세마포어를 생성한 후, 태스크를 생성하고 RTOS를 시작합니다.

커스텀 RTOS 개발은 임베디드 시스템의 특정 요구 사항에 맞게 최적화된 운영체제를 만드는 과정입니다. 기본적인 태스크 관리, 스케줄링, 동기화 메커니즘을 구현하여 실시간 성능을 보장할 수 있습니다.
