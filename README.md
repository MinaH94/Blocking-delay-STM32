# Blocking delay library for STM32
A blocking delay library written for STM32.

# How to use
1. After setting-up the HCLK (usually AHB bus prescaler) call `delay_setCPUclockFactor(...)` with the value of of the HCLK in Hz. Example:
   ```c++
   delay_setCPUclockFactor(8000000); /* Initialize the delay library with HCLK = 8MHz */
   ```
2. You can now call `delay_ms(...)` with the required time in milliseconds, or `delay_us(...)` with the required time in microseconds.

The library has a leading error, for example `delay_ms(1000)` will delay by ~900ms.
