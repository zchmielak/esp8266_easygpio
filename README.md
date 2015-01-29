# esp8266_easygpio
An easy way of setting up esp8266 GPIO pins.

I grew tired of juggling gpio pin numbers, gpio names and gpio functions. So i created this little helper library.

To setup a pin as a GPIO input you can now just do this:

```
#include "easygpio/easygpio.h"
...

uint8_t gpio_pin = 0;
PullStatus pullStatus = NOPULL;
PinMode pinMode = INPUT;
bool easygpio_pinMode(gpio_pin, pullStatus, pinMode);
```

Same thing with outputs:
```
uint8_t gpio_pin = 0;
PullStatus pullStatus = NOPULL;
PinMode pinMode = OUTPUT;
bool easygpio_pinMode(gpio_pin, pullStatus, pinMode);
```
pullStatus does not apply to output pins.

You might still need the gpio_name and func. No problem:
```
bool easygpio_getGPIONameFunc(uint8_t gpio_pin, uint32_t *gpio_name, uint8_t *gpio_func)
```

You can even setup an interrupt handler:
```
bool easygpio_attachInterrupt(uint8_t gpio_pin, PullStatus pullStatus, void (*interruptHandler)(void))
```

But you will still have to do this little dance in your interrupt handler code:
```

static void interrupt_handler(void) {
  uint32_t gpio_status = GPIO_REG_READ(GPIO_STATUS_ADDRESS);
  //clear interrupt status
  GPIO_REG_WRITE(GPIO_STATUS_W1TC_ADDRESS, gpio_status & BIT(my_interrupt_gpio_pin));
  ......
```

See an example [here](https://github.com/eadf/esp8266_digoleserial)
