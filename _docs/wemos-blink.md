---
permalink: /docs/arduino/wemos-blink/
title: "Blinkende LED"
# tags: Arduino Sketch Programmierung ESP8266 Wemos PlatformIO
author_profile: true
author: Diego
comments: true
---
Der Wemos D1 mini besitzt eine blaue LED in der Nähe des TX-Pins.

<figure>
    <a href="/assets/images/docs/wemos-d1-mini_led.jpg"><img src="/assets/images/docs/wemos-d1-mini_led.jpg" alt="Die blaue Onboard-LED des Wemos D1 Mini leuchtet."></a>
    <figcaption>
        Blaue Onboard-LED des Wemos D1 Mini <i>(<a href="https://creativecommons.org/licenses/by/4.0/">CC BY 4.0</a> Diego Jahn)</i>
    </figcaption>
</figure>

Die Onboard-LED ist mit dem [digitalen Ein- und Ausgang D4](http://escapequotes.net/wemos-d1mini-arduinoide/) verbunden. Statt ```D4``` zu schreiben kann man den LED-Pin auch mit der vordefinierten Konstante ```BUILTIN_LED``` ansteuern.

{% gist jdieg0/b9b9907df9251bbfe9527bf33f4cada1 %}

## Weiterführende Links

- [Konstanten definieren](https://www.arduino.cc/en/Reference/Const)
- [Blink-Beispiel aus den Arduino-Examples](https://www.arduino.cc/en/Tutorial/Blink)
