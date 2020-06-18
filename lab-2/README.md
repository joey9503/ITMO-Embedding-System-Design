## Lab-2 Report

### Objective
1. Know what is *RTOS*.
2. Know the basic idea of *RTOS*.
3. Know how to make a simple project with *RTOS* in *STM32CubeIDE*.

### Task Description

This task is achieving different frequency of LED flashing through freeRTOS.
#### 1. Append Task
Task should be added by `.ioc` file:  
![tasks](https://tva1.sinaimg.cn/large/007S8ZIlly1gfwerpgr8lj30hk09174j.jpg)  
#### 2. Code Generation
From graph above, 2 tasks have been added to the project, then code will be generated automatically:
```C
void StartDefaultTask(void *argument)
{
  for(;;)
  {
    HAL_GPIO_TogglePin(GPIOB, GPIO_PIN_7);
    osDelay(500);
  }
}
```
This is the first task which controls blue LED.
```C
void StartTask02(void *argument)
{
  for(;;)
  {
    HAL_GPIO_TogglePin(GPIOB, GPIO_PIN_14);
    osDelay(1000);
  }
}
```
This is the second task which controls red LED.
<br></br> 
It is easy to find that the 2 tasks are running simultaneously.
### Question
1. What features of *FreeRTOS* do you remember?  
    - FreeRTOS is a mini real-time operating system kernel. As a lightweight operating system, its functions include: task management, time management, semaphore, message queue, memory management, recording function, software timer, coroutine, etc., which can basically meet the needs of smaller systems.  
2. What are the differences between FreeRTOS, OpenRTOS and SafeRTOS?  
    - **OpenRTOS** provides a commercial license for FreeRTOS. This includes a license for the FreeRTOS kernel as well as, if needed, the additional software libraries that make up FreeRTOS.
    - **SafeRTOS** is based on the functional model of the FreeRTOS kernel. However, SAFERTOS is not the FreeRTOS kernel. It has been completely redesigned by our team of safety experts. Our engineers took the FreeRTOS kernel functional model, subjected it to a full HAZOP, identified all areas of weakness within the functional model and API, and generated a set of safety requirements. The resulting functional and safety requirements sets were put through an IEC 61508-3 SIL 3 development life cycle, the highest possible for a software only component, creating the SAFERTOS code base and DAP.
3. Why do we need the `vTaskStartScheduler()` function?
    - It is a function starts the FreeRTOS scheduler running.
      Typically, before the scheduler has been started, `main()` (or a function called by `main()`) will be executing. After the scheduler has been started, only tasks and interrupts will ever execute.Starting the scheduler causes the highest priority task that was created while the scheduler was in the Initialization state to enter the Running state.
4. Why do we need the `xTaskCreate()` function?
    - Creates a new instance of a task.
      Each task requires RAM that is used to hold the task state (the task control block, or TCB), and used by the task as its stack. If a task is created using `xTaskCreate()` then the required RAM is automatically allocated from the FreeRTOS heap. If a task is created using `xTaskCreateStatic()` then the RAM is provided by the application writer, which results in two additional function parameters, but allows the RAM to be statically allocated at compile time. Newly created tasks are initially placed in the Ready state, but will immediately become the Running state task if there are no higher priority tasks that are able to run. Tasks can be created both before and after the scheduler has been started.

### Summary
In this lab work I knew how to:
1. Install *FreeRTOS* in *STM32CubeIDE*.
2. Understand the *FreeRTOS* organization.
3. Make multiple tasks with *FreeRTOS*.