---
permalink: /docs/raspberrypi/neopixel/
title: "Neopixel (WS2812)"
# tags: Programmierung, Neopixel, WS2812, LED
author_profile: true
author: Diego
comments: true
---
{% include toc %}
Bei Neopixel-RGB-LEDs ist ein genaues Timing bei der Ansteuerung gefragt. Aus diesem Grund greift man dafür üblicherweise zu Mikrocontroller-basierten Systemen wie zum Beispiel Arduino-Boards. Dementsprechend wird auch die [Adafruit-Neopixel-Bibliothek][adafruit-neopixel] für Arduino entwickelt.

Mit Hilfe der Bibliothek [*rpi_ws281x*][ws281x] lassen sich aber auch mit Python auf dem Raspberry Pi Neopixel zuverlässig und bequem ansteuern.

## Bibliothek

Für die Installation der Python-Bibliothek müssen folgende Pakete installiert sein:

    $ sudo apt-get install build-essential python-dev git scons swig

Die Bibliothek wird installiert mit:

    $ git clone https://github.com/jgarff/rpi_ws281x.git
    $ cd rpi_ws281x
    $ scons
    $ cd python
    $ sudo python setup.py install

## Schaltung

Für die Stromversorgung wird einer der 5V-Pins des Raspberry Pi verwendet. Der *In*-Port der Neopixel muss mit einem der PWM-Ports des Pi verbunden werden, bspw. Pin 12 (BCM 18) wie in den Beispielprogrammen der Bibliothek angegeben.

| Neopixel | Raspberry Pi                                              |
|----------|-----------------------------------------------------------|
| GND/G    | GND                                                       |
| 5V/+     | 5V                                                        |
| In/DI    | [BCM 18](https://pinout.xyz/pinout/pin12_gpio18)/12/13/19 |
| O/DO     | –                                                         |

{% capture notice-pwm %}
Bei anderen PWM-Pins muss eventuell der Kanal (```LED_CHANNEL```) im Code mit angegeben werden. Weitere Infos dazu gibt es in [diesem Blogpost](http://www.detlef-huettemann.com/post/adafruit-neopixel-raspberry/).
{% endcapture %}
<div class="notice--primary">
    {{ notice-pwm | markdownify }}
</div>

## Code

Im Ordner ```python/examples``` finden sich einige Beispiele. Nachdem im entsprechenden Beispiel-Programm die Variablen ```LED_COUNT``` und ggf. ```LED_PIN``` und ```LED_CHANNEL``` angepasst wurden, kann es gestartet werden mit:

     $ sudo python strandtest.py

{% capture notice-sudo %}
Die *rpi_ws281x*-Bibliothek erfordert Root-Zugriff, sodass das Python-Skript mit ```sudo```aufgerufen werden muss.
{% endcapture %}
<div class="notice--info">
    {{ notice-sudo | markdownify }}
</div>

## Weiterführende Links

- [Detmans Blog: „AdaFruit NeoPixel am Raspberry“][detmansblog]
- [Adafruit: „Adafruit NeoPixel Überguide“][adafruit-neopixel]
- [GitHub: Bibliothek *rpi_ws281x* für den Raspberry Pi][ws281x]
- [Adafruit: „NeoPixels on Raspberry Pi“](https://learn.adafruit.com/neopixels-on-raspberry-pi/overview)

[detmansblog]: http://www.detlef-huettemann.com/post/adafruit-neopixel-raspberry/
[adafruit-neopixel]: https://learn.adafruit.com/adafruit-neopixel-uberguide
[ws281x]: https://github.com/jgarff/rpi_ws281x
