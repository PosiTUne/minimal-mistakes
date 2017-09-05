---
permalink: /docs/arduino/wemos-bmp180-shield/
title: "Drucksensor BMP180"
# tags: Arduino Sketch Programmierung ESP8266 Wemos PlatformIO BMP180
author_profile: true
author: Diego
comments: true
---
{% include toc %}

Das BMP180-Shield für den Wemos D1 Mini nutzt die Pins D1 (SCL) und D2 (SDA) für die Kommunikation via I<sup>2</sup>C. Praktischerweise ist bereits eine Bibliothek namens [*Adafruit_BMP085_Unified*](https://github.com/adafruit/Adafruit_BMP085_Unified) vorhanden, mit der sich der Drucksensor sehr einfach auslesen lässt. Zusammen mit der Temperatur, die der Sensor auch liefert, lässt sich die Höhe bestimmen, in der sich der Sensor befindet.

<figure>
    <a href="/assets/images/docs/wemos-d1-mini_bmp180-shield.jpg"><img src="/assets/images/docs/wemos-d1-mini_bmp180-shield.jpg" alt="Foto des BMP180-Shields."></a>
    <figcaption>
        Das BMP180-Shield für den Wemos D1 Mini. Man erkennt gut die Leiterbahnen, die vom Sensor-Chip zu den I<sup>2</sup>C-Pins D1 (SCL) und D2 (SDA) führen. <i>(<a href="https://creativecommons.org/licenses/by/4.0/">CC BY 4.0</a> Diego Jahn)</i>
    </figcaption>
</figure>

## Bibliothek

Die Adafruit-Bibliothek wird im PlatformIO-Projekt-Ordner wie folgt installiert:

    $ platformio lib install "Adafruit BMP085 Unified"

## Code

Druck und Temperatur werden folgendermaßen abgefragt und angezeigt:

{% gist jdieg0/f297627497c1f151df4fe3ad8f9e2966 %}

{% capture notice-fehler %}
Leider funktioniert die Umrechnung von Druck und Temperatur in eine Höhe mit Hilfe der Funktion ```pressureToAltitude()``` nicht auf dem Wemos-Board, da es ein Problem mit der Mathe-Bibliothek *e_pow.c* gibt:

```
/home/pi/xtensa/esp-open-sdk/crosstool-NG/.build/src/newlib-2.0.0/newlib/libm/math/e_pow.c:164: undefined reference to `__ieee754_sqrt'
```

Mit der dieser Funktion zugrundeliegenden [baromentrischen Höhenformel](https://de.wikipedia.org/wiki/Barometrische_Höhenformel) kann man allerdings die Höhe auch selbst ausrechnen.
{% endcapture %}
<div class="notice">
    {{ notice-fehler | markdownify }}
</div>

## Anzeigen der Werte

Über den seriellen Monitor werden die Messwerte angezeigt:

    $ screen /dev/ttyUSB0 9600

{% capture notice-ausgabe %}
So sieht die Ausgabe beispielsweise aus:

```
Druck: 996.88 hPa, Temperatur: 29.30 *C
Druck: 996.84 hPa, Temperatur: 29.30 *C
Druck: 996.93 hPa, Temperatur: 29.40 *C
Druck: 996.87 hPa, Temperatur: 29.40 *C
Druck: 996.91 hPa, Temperatur: 29.40 *C
```
{% endcapture %}
<div class="notice--success">
	{{ notice-ausgabe | markdownify }}
</div>

{% capture notice-screen %}
Das Screen-Fenster kann man mit ```Strg + a``` ```k``` schließen (mit ```y``` bestätigen).
{% endcapture %}
<div class="notice--info">
	{{ notice-screen | markdownify }}
</div>

{% capture notice-waerme %}
Die sonst praktische Steckmöglichkeit der Wemos-Platinen erweist sich hier eher als Nachteil. Die Wärmeabstrahlung des Wemos D1 Mini ist nicht ganz unbeträchtlich, vor allem über die eng beieinander liegenden Masse-Pins (GND) beider Boards wird der BMP180-Temperatursensor stark erwärmt.

<figure>
    <a href="/assets/images/docs/wemos-d1-mini_bmp180-shield_gesteckt.jpg"><img src="/assets/images/docs/wemos-d1-mini_bmp180-shield_gesteckt.jpg" alt="Foto des BMP180-Shields zusammengesteckt mit einem Wemos D1 Mini"></a>
    <figcaption>
       Wemos D1 Mini und das BMP180-Shield übereinander gesteckt <i>(<a href="https://creativecommons.org/licenses/by/4.0/">CC BY 4.0</a> Diego Jahn)</i>
    </figcaption>
</figure>

Falls zuverlässigere Temperaturwerte gebraucht werden, sollte man das [Dual-Base-Shield](https://wiki.wemos.cc/products:d1_mini_shields:dual_base) verwenden oder das BMP180-Shield mit Kabeln mit dem Wemos D1 Mini verbinden.
{% endcapture %}
<div class="notice--warning">
	{{ notice-waerme | markdownify }}
</div>

## Weiterführende Links

- [Adafruit-Tutorial „Using the BMP (API v2)“](https://learn.adafruit.com/bmp085/using-the-bmp085-api-v2)
- [Bibliothek *Adafruit BMP085 Unified* bei PlatformIO](http://platformio.org/lib/show/16/Adafruit%20BMP085%20Unified)
- [Arduino-Befehlsreferenz für die serielle Kommunikation mit *Serial*](https://www.arduino.cc/en/Reference/Serial)
- [BMP180-Shield bei *ESP8266 Learning*](http://www.esp8266learning.com/wemos-mini-bmp180-shield.php)
