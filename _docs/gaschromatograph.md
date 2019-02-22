---
permalink:      /docs/gaschromatograph/
title:          "Gaschromatograph"
author:         Diego
author_profile: true
comments:       true
---
{% include toc %}

In dieser Doku werden Informationen über den Selbstbau-Gaschromatographen *AS656* des [*Arbeitskreises Amateurfunk und Telekommunikation in der Schule e.V.* (AATiS)](https://www.aatis.de) gesammelt.

[comment]: # (## Über den Gaschromatographen)

[comment]: # (Beschreibung des Geräts)

## Anschluss der Sensoren

### Allgemeine Hinweise

- Für die Sensoren muss mit einem roten Jumper der korrekte **Rx-Widerstand** sowie die richtige **Spannungsversorgung** eingestellt sein.

### WLD

- Rx-Widerstand: 82 Ω (i. d. R. Anschluss rechts oben)
- Spannungsversorgung: intern (USB) oder extern (5 V, Minuspol außen am Rundstecker)
- Wert wird i. d. R. im seriellen Monitor an zweiter Stelle angezeigt)

### MQ2

- Rx-Widerstand: 47 kΩ (i. d. R. Anschluss rechts unten)
- Spannungsversorgung: extern (5 V)
- das Sensormodul kann in beiden Orientierungen auf den weißen Sockel aufgesteckt werden, die Verdrahtung/Pin-Belegung ist symmetrisch
- Wert wird i. d. R. im seriellen Monitor an erster Stelle angezeigt)

## Serieller Monitor

Werte ausgeben lassen:

    $ pio device monitor -b 38400

## AS646quant

- Kanal A (blau) / Nullpunkttaster S1: MQ2
- Kanal B (rot) / Nullpunkttaster S2: WLD



[comment]: # (## Weiterführende Links)


