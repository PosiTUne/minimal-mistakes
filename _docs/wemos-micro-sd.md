---
permalink: /docs/arduino/wemos-micro-sd-karten-shield/
title: "Micro-SD-Karten verwenden"
# tags: [Arduino, Sketch, Programmierung, ESP8266, Wemos, PlatformIO, microSD, Micro-SD-Karte]
author_profile: true
author: Diego
comments: true
---
{% include toc %}
Um Daten in größerem Umfang aufzuzeichnen bzw. vom Board bspw. als [CSV-Datei](https://de.wikipedia.org/wiki/CSV_(Dateiformat)) auf einen anderen Computer zu übertragen und dort weiterzuverarbeiten, kann man das Micro-SD-Karten-Shield von Wemos zuhilfe nehmen.

<figure>
    <a href="/assets/images/docs/wemos-d1-mini_micro-sd-card-shield.jpg"><img src="/assets/images/docs/wemos-d1-mini_micro-sd-card-shield.jpg" alt="Foto des Micro-SD-Karten-Shields."></a>
    <figcaption>
        Das Micro-SD-Karten-Shield für den Wemos D1 Mini. <i>(<a href="https://creativecommons.org/licenses/by/4.0/">CC BY 4.0</a> Diego Jahn)</i>
    </figcaption>
</figure>

Das Micro-SD-Karten-Shield kommuniziert mittels [SPI](https://de.wikipedia.org/wiki/Serial_Peripheral_Interface) mit dem Wemos D1 Mini. Hierfür werden die Pins D5-D8 verwendet.

| D1 Mini | Shield |
|---------|--------|
| D5      | SCK    |
| D6      | MISO   |
| D7      | MOSI   |
| D8      | CS     |

## Code
### CardInfo

Arduino bietet selbst einige Beispiel-Programme an, um mit SD-Karten zu interagieren. Eines davon heißt [„CardInfo“](https://www.arduino.cc/en/Tutorial/CardInfo) und liefert einige Informationen über die eingesetzte Micro-SD-Karte. Eine angepasste Version des Programms, in der der korrekte CS-Pin angegeben ist (Zeile 36: ```const int chipSelect = D8;```) ist diese hier:

{% gist jdieg0/68602db4dd2925356ae0c63c7fa91a18 %}

Testweise wurde bei der hier verwendeten SD-Karte die Datei „Test.txt” angelegt. Über die Ausgabe des Programms per seriellem Monitor

    $ screen /dev/ttyUSB0 9600

bekommt man dann beispielsweise Folgendes angezeigt:

```
Initializing SD card...Wiring is correct and a card is present.

Card type: SDHC

Volume type is FAT32

Volume size (bytes): 3094347776
Volume size (Kbytes): 3021824
Volume size (Mbytes): 2951

Files found on the card (name, date and size in bytes):
TEST.TXT      2017-06-10 18:35:52 0
```

Die Testdatei findet sich also wieder.

{% capture notice-speicher %}
Die angezeigte Speicherkapazität ist mit 2951 MB deutlich geringer als die nominellen 16 GB, die die Micro-SD-Karte in diesem Beispiel eigentlich haben sollte. Das soll uns aber vorerst nicht weiter stören.
{% endcapture %}
<div class="notice">
	{{ notice-speicher | markdownify }}
</div>

### Dateien schreiben
#### Eine Zeile Text

Zuerst wollen wir eine Datei mit dem Namen „text.txt“ erstellen und eine Zeile Text reinschreiben. 

{% gist jdieg0/00abb7ab5980b5d027f300045cbce735 %}

{% capture notice-seriell %}
Du kannst über den seriellen Monitor die Meldungen verfolgen.

```
$ screen /dev/ttyUSB0 9600

Initialisiere SD-Karte... fertig.
Schreibe in text.txt... fertig.
```

Starte mal das Programm, nachdem du die Micro-SD-Karte entfernt hast! Du bekommst dann nach einigen Sekunden vergeblicher Versuche des Wemos D1 Mini, die SD-Karte anzusprechen, die Fehlermeldung:

    Initialisiere SD-Karte... fehlgeschlagen!
{% endcapture %}
<div class="notice--info">
	{{ notice-seriell | markdownify }}
</div>

Auf der Micro-SD-Karte findet sich nun die Datei "TEXT.TXT" mit dem Inhalt:

    Test, Test. 1, 2, 3.

{% capture notice-text %}
Je nachdem, wie oft du das Programm ausgeführt hast, kann diese Zeile auch mehrmals in "TEXT.TXT" stehen, denn beim Schreiben wird standardmäßig eine vorhandene Datei nicht überschrieben, sondern der neue Inhalt hinten angehängt.
{% endcapture %}
<div class="notice--info">
	{{ notice-text | markdownify }}
</div>

#### Daten kontinuierlich schreiben

Zuguterletzt werden wir uns mal ansehen, wie man eine ganze Reihe von Daten schreiben kann. Schließlich soll ja die SD-Karte mal dafür genutzt werden, Sensordaten kontinuierlich aufzuzeichnen.

Beispielhaft soll im folgenden Programm pro Sekunde eine Zufallszahl im Bereich von 0 bis 1023 generiert und abgespeichert werden. Das Ganze versehen wir mit einer Zeitmarke.

{% gist jdieg0/ccaf4cc5991088503259a36d53aa3312 %}

Die Zeit seit Start des Programms erhält man mit [```millis()```](https://www.arduino.cc/en/Reference/Millis), die Funktion [```random()```](https://www.arduino.cc/en/Reference/Random) liefert uns die Zufallszahl.

Die Ausgabe über die serielle Schnittstelle verrät uns, dass fleißig Werte gespeichert werden.

```
...
Schreibe in RANDOM.TXT... fertig.
Schreibe in RANDOM.TXT... fertig.
Schreibe in RANDOM.TXT... fertig.
Schreibe in RANDOM.TXT... fertig.
Schreibe in RANDOM.TXT... fertig.
...
```

Und voilà, in der „RANDOM.TXT“ auf der Micro-SD-Karte sind die sekündlichen Werte alle auch tatsächlich gelandet!

```
...
33159   511
34168   5
35177   979
36186   185
37195   652
38204   27
39213   1001
40222   803
41231   580
42240   858
...
```

{% capture notice-hinweise %}
Einige Hinweise:

- Die SD-Karten-Bibliothek unterstützt nur [8.3-Dateinamen](https://de.wikipedia.org/wiki/8.3), d. h. Dateien oder Verzeichnisse dürfen nur aus maximal **acht Buchstaben oder Ziffern** bestehen, optional gefolgt von einer Dateiendung, bestehend aus einem **Punkt** und maximal **drei weiteren Zeichen**. Es wird nicht zwischen Groß- und Kleinschreibung unterschieden. Wie du schon gemerkt hast, wird der Dateiname von der Bibliothek immer in Großbuchstaben umgewandelt. Auch mit der Verwendung von Sonderzeichen solltest du vorsichtig sein!
- Nur durch Schließen der Datei mit der ```close()```-Funktion ist sichergestellt, dass die Daten sicher geschrieben wurden!
{% endcapture %}
<div class="notice--warning">
	{{ notice-hinweise | markdownify }}
</div>

## Weiterführende Links

- [Produktbeschreibung im Wemos-Wiki](https://wiki.wemos.cc/products:d1_mini_shields:micro_sd_card_shield)
- ["Micro SD Card Breakout Board Tutorial" bei Adafruit](https://learn.adafruit.com/adafruit-micro-sd-breakout-board-card-tutorial)
- [Arduino-Befehlsreferenz zu Strings](https://www.arduino.cc/en/Reference/String)
- [Code-Beispiele von Wemos auf Github](https://github.com/wemos/D1_mini_Examples/tree/master/examples/04.Shields/Micro_SD_Shield)
