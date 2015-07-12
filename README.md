##About

Node.js bindings to [wiringPi](http://www.wiringpi.com)
for Raspberry Pi 2 and the latest WiringPi development sources.

Based on the awesome work of:

1. [Eugene Ware](https://github.com/eugeneware/wiring-pi)
2. [Soarez](https://github.com/Soarez/node-wiring-pi)


## Motivation

I wanted to create a simple LED animation with a [node.js](https://nodejs.org/) based Javascript.
For certain reasons, the performance impact of the GPIO related code should be 
as small as possible. Thus calls to external command line tools aren't an option.

Currently (July 2015), there are a handful libraries out there,
which are wrappers for the [wiringPi](http://www.wiringpi.com) library.
My fokus went on 'wiring-pi' libs, because they seemed to me matured enough. But none of the
[blink.js](https://github.com/eugeneware/wiring-pi/blob/master/examples/blink.js)
examples worked on my Pi 2 - I couldn't set any GPIO pins to low nor high.
I figured out that wiring-pi used out-dated wiringPi code and latest development
in wiringPi fixed certain RaspPi2 issues.

So I rolled up my sleeves up and started the pebbly way to patch wiring-pi. 

## Install

```
npm install wiring-pi
```

## LED animation example

![LED animation with Raspberry Pi 2](/docs/raspberry_pi2_led_animation.gif?raw=true)

```javascript
var wpi = require('/home/pi/wiring-pi/lib/exports.js');
wpi.wiringPiSetup()

// for pinout, see http://pi.gadgetoid.com/pinout
var LED_PINS_WiringPi = [8, 9, 7, 0, 1];

for (var i=0; i < LED_PINS_WiringPi.length; i++) {
  wpi.pinMode(LED_PINS_WiringPi[i], wpi.OUTPUT);
}

var values = [1,1,1,1,1];
var index = 0;

setInterval(function() {
  // previous LED
  var pin = LED_PINS_WiringPi[index];
  values[index] = +!values[index];
  wpi.digitalWrite(pin, values[index]);

  // next LED
  index = (index + 1) % LED_PINS_WiringPi.length;
  pin = LED_PINS_WiringPi[index];
  wpi.digitalWrite(pin, values[index]);
}, 50);
```

