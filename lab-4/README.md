# lab-4 report
## Objective
Know how to use `Semaphore` to synchronize between tasks.
## Code
1. Define `queue` and `semaphore`.
```C
...
#include "queue.h"
#include "semphr.h"
...
/* Definition of queues. */
xQueueHandle q1;
xQueueHandle q2;
/* Definition of semaphore. */
SemaphoreHandle_t xSemaphore;
...
```
<br></br>
2. Initialization of `queue` and `semaphore`.
```C
int main(void)
{
    ...
    /* Size of queue is 7. */
    q1 = xQueueCreate(7, sizeof(unsigned int));
    q2 = xQueueCreate(7, sizeof(unsigned int));
    ...
    /* There is only 1 LED so the maximum value of semaphore is 1. */
    xSemaphore = xSemaphoreCreateCounting(1, 1);
    ...
}
```
<br></br>
3. The first task is going to send data to `q1` and `q2` separately. Variable `c` here controls the time. Please note that if the queue is full, it will blocked 500 ms.
```C
void StartTask01(void *argument) {
	unsigned int counter = 0;
	unsigned int c = 0;
	for (;;) {
		xQueueSend(q1, (void* )&counter, 500);
		++counter;
		osDelay(1000);
		if (c++ % 2 != 0)
			xQueueSend(q2, (void* )&counter, 500);
	}
}
```
<br></br>
4. The second and third task receive the data from the first task. `Seamphore` is used to protect the shared resource LED.
```C
 void StartTask02(void *argument) {
	for (;;) {
		unsigned int data;
		if (uxQueueMessagesWaiting(q1) > 1) {
			xQueueReceive(q1, &(data), 500);
			if (xSemaphore != NULL) {
				xSemaphoreTake(xSemaphore, portMAX_DELAY);
				for (int i = 0; i <= data; ++i) {
					HAL_GPIO_TogglePin(GPIOB, GPIO_PIN_14);
					osDelay(500);
				}
				xSemaphoreGive(xSemaphore);
			}
		}
	}
}
...
void StartTask03(void *argument) {
	for (;;) {
		unsigned int data;
		if (uxQueueMessagesWaiting(q2) > 1) {
			xQueueReceive(q2, &(data), 500);
			if (xSemaphore != NULL) {
				xSemaphoreTake(xSemaphore, portMAX_DELAY);
				for (int i = 0; i <= data; ++i) {
					HAL_GPIO_TogglePin(GPIOB, GPIO_PIN_14);
					osDelay(500);
				}
				xSemaphoreGive(xSemaphore);
			}
		}
	}
}
```
When `xSemaphoreTake(xSemaphore, portMAX_DELAY)` is called successfully, the value of semaphore will be reduced by 1. When semaphore is not available, it will be blocked until it is available. On the contrary, when `xSemaphoreGive()` is called, the value of semaphore will be added by 1, which means there is a new resource has been available.
## Summary
In this lab work, I knew how to use semaphore in *FreeRTOS* and to control the communication between multiple tasks.