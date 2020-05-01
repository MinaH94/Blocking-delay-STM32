# A cheap blocking-delay library for STM32

## How to use it
1. After setting-up the HCLK (usually AHB bus prescaler) call `delay_setCPUclockFactor(...)` with the value of of the HCLK in Hz. Example:
   ```c++
   #include "Delay_interface.h"
   
   void main(void)
   {
      /* initialize HCLK here */
      
      delay_setCPUclockFactor(8000000); /* Initialize the delay library with HCLK = 8MHz */
      
      /* some code */
      
      delay_ms(500); /* delay be 0.5 sec */
   }
   ```
2. You can now call `delay_ms(...)` with the required time in milliseconds, or `delay_us(...)` with the required time in microseconds.

The library has a leading error, for example `delay_ms(1000)` will delay by ~900ms.

## How it was made
Each section of the code was calcualted by setting a pin HIGH before executing it, then setting it to LOW afterwards. \
The following sections were calculated:
1. Function prologue
2. The volatile counter initialization
3. The const counter value
4. The time of each iteration
5. Function epilogue

All the calculations are normalized to 8MHz, because all the previous calculations were based on an 8MHz crystal oscillator.

## A simplified version of the equation:

1. Normalize the value based on the value of HCLK used in measurement: \
`required_time *= current_hclk_val / measured_hclk_val`
2. Subtract the wasted time from the required time: \
`required_time -= prologue + loop_counter_initialization + loop_iterations_initialization + epilogue` 
3. Calculate the required amount of iterations: \
`loop_iterations = required_time / time_of_1_iteration` 

