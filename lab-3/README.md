# lab3-report
## Objective
1. Know how to communicate between several tasks with `quque`.
2. Know how to create `queue` in *FreeRTOS*.
## Task Description
One task is going to put data into a queue. Meanwhile another task should be recieved data and flashes the corresponding LED.
## Code
Firstly, we should include head file and declare a `queue`:
```C
...
#include "queue.h"
...
/* Declare a queue structure.*/
xQueueHandle q;
```
Then, the queue is going to be initialized inside the `main()`:
```C
...
int main()
{
 ...
 /* Queue initializtion*/
 q = xQueueCreate(8, sizeof(unsigned int));
 ...   
}
```
The first task is responsible for sending data:
```C
void StartDefaultTask(void)
{
    unsigned int data = 1;
    for (;;)
    {
        /* Sending data. */
        xQueueSend(q, (void *)&data, portMAX_DELAY);
        osDelay(data*1000);
        ++data;
    } 
}
```
The second task will be received data and flashes red LED.
```C
void StartTask02(void *argument)
{
    for (;;)
    {
        unsigned int rec;
        if (uxQueueMessagesWaiting(q)>1)
        {
            /* Receiving data from task 1, stored into variable 'rec'*/
            xQueueReceive(q, &(rec), portMAX_DELAY);
            for (int i = 0; i <=rec; ++i)
            {
                HAL_GPIO_TogglePin(GPIOB, GPIO_PIN_14);
                osDelay(100);
            }
            osDealy(1);
        }
    }
}
```
## Summary
In this lab work I knew how to:
1. Create a queue in *FreeRTOS*.
2. Communicate two tasks with queue.
