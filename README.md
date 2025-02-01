# edu-python-pico

## Prepare

```bash
pipx install mpremote
```

## Instructions

```bash
cd ~
cd ws
mkdir -p python-pico
cd python-pico
```

```bash
cat << EOF > blink.py
from machine import Pin
import time

class Blinker:
    def __init__(self, pin='LED'):  # Default: onboard LED (for Pico, not Pico W)
        self.led = Pin(pin, Pin.OUT)

    def blink(self, times=5, interval=0.5):
        for _ in range(times):
            self.led.toggle()
            time.sleep(interval)
EOF
```

```bash
cat << EOF > main.py
import blink
import time

blinker = blink.Blinker()

while True:
    blinker.blink(times=3, interval=0.3)  # Blink 3 times with 0.3s interval
    time.sleep(2)  # Pause before next cycle
EOF
```

```
ls /dev/cu.usbmodem*  # macOS
ls /dev/ttyACM*       # Linux

PICO_PORT="/dev/ttyACM0"  # Change this if necessary
PICO_PORT="/dev/cu.usbmodem101"  # Change this if necessary

mpremote connect $PICO_PORT cp blink.py :
mpremote connect $PICO_PORT cp main.py :

mpremote connect $PICO_PORT run main.py

mpremote connect $PICO_PORT cp main.py :boot.py
mpremote connect $PICO_PORT soft-reset
```


