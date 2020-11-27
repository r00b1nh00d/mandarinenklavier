# Mandarinen Klavier
## ~avatar avatar @unplugged
Du hast bereits gelernt den Calliope Musikstücke abspielen zu lassen. 
Du kannst mit dem Calliope aber auch selbst musizieren und ihn in eine Art kleines Klavier umbauen.<br>
![MandarinenKlavier](https://github.com/r00b1nh00d/mandarinenklavier/blob/master/mandarine2.gif?raw=true) <br>
**Hinweis** immer wen sich die LED-Farbe ändert kommt ein unterschiedlicher Ton.

## ~ @unplugged einfache Version
Beginnen wir erstmal mit einer einfachen Version. <br>
Hier hast du aber leider nur vier Tasten zur verfügung. <br>
![MandarinenKlavier](https://github.com/r00b1nh00d/mandarinenklavier/blob/master/Mandarine1.gif?raw=true) <br>
**Hinweis** immer wen sich die LED-Farbe ändert kommt ein unterschiedlicher Ton.


## Einfache Version
Du kannst hierfür die Blocke ``||inputs:wenn Pin gedrückt||`` nutzen.<br>
In diese Blöcke schiebst du einfach den Block ``||music:spiele Note||`` und wählst pro Pin je eine Note aus. <br>
Ich habe hier zusätzlich noch je ein Block ``||basic: setze RGB Farbe||`` verwendet um noch etwas farbe ins spiel zu bringen. <br>
**Um einen Pin zu drücken musst du zusätzlich noch den Minuspol am Calliope berühren** 

```blocks
input.onPinPressed(TouchPin.P0, function () {
    music.playTone(262, music.beat(BeatFraction.Whole))
    basic.setLedColor(0xffff00)
})
input.onPinPressed(TouchPin.P3, function () {
    basic.setLedColor(0xff0000)
    music.playTone(294, music.beat(BeatFraction.Whole))
})
input.onPinPressed(TouchPin.P2, function () {
    basic.setLedColor(0x00ff00)
    music.playTone(294, music.beat(BeatFraction.Whole))
})
input.onPinPressed(TouchPin.P1, function () {
    basic.setLedColor(0x007fff)
    music.playTone(294, music.beat(BeatFraction.Whole))
})

```

## Komplexere Version Teil 1
Um noch mehr Tasten an deinem Klavir zu haben kannst du auch die analogen Werten auslesen. <br>
Hier gibt es nämlich insgesammt 6 Pins die solche Werte auslesen können. <br>
Dieser analoge Wert kann aber auch abhängign von der Leitfähigkeit deiner Finger oder der Mandarinien Stücke sein. <br>
Daher solltest du hier erstmal die gemessenen Werte Auslesen. <br>
Ist der Calliope an deine Computer via USB angeschlossen kannst du dir diese Werte mit dem Block ``||serial: seriell Zeile ausgeben||`` am Computer anzeigen lassen. (diesen Block findest du unten im Fortgeschrittenen Bereich) <br>
**Achtung die folgenden Schritte werden jetzt auch ein ganzes Stück anspruchsvoller** <br>
Mit diesem Programm kannst du dir nach dem Drücken von "A" die Werte aller analogen Pins ausgeben lassen. Da wir die Pin's C4, C5 und C6 nutzen, welche auch für das Display vorgesehen sind macht es sinn dieses beim Start auszuschalten.

```blocks
led.enable(false)
input.onButtonPressed(Button.A, function () {
    serial.writeLine("")
    serial.writeLine("" + (pins.analogReadPin(AnalogPin.P2)))
    serial.writeLine("" + (pins.analogReadPin(AnalogPin.C4)))
    serial.writeLine("" + (pins.analogReadPin(AnalogPin.C5)))
    serial.writeLine("" + (pins.analogReadPin(AnalogPin.C6)))
    serial.writeLine("" + (pins.analogReadPin(AnalogPin.C16)))
    serial.writeLine("" + (pins.analogReadPin(AnalogPin.C17)))
})

```

## ~ @unplugged
Um dir die Werte am Computer ausgeben zu lassen brauchst du ein Programm wie [putty](https://www.putty.org/). <br>
![puttyNutzen](https://github.com/r00b1nh00d/mandarinenklavier/blob/master/Puttynutzen.png?raw=true) <br>
Öffne das Programm Putty. Dort muss du zuerst "Serial" auswählen. Anschließend musst du im Gerätemanager schauen an welchem COM-Anschluss dein Calliope angeschlossen ist. Bei mir war es der Port COM4. Den Gerätemanager erreichst du z.B. mit rechtsklick auf das Windows-Symbol unten links im Bild. Als übertragungsgeschwindigkeit musst du für den Calliope noch die 115220 einstellen. Nachdem du bei putty auf Open geklickt hast öffnet sich ein Fenster mit schwarzem Hintergrund. Dort sollten dir jetzt nach einem Drücken der Taste "A" die Werte angezeigt werden.

## ~ @unplugged
Jetzt kannst du Mandarinenstücke oder andere Dinge über Jumperkabel mit den Pins am Calliope verbinden.
Wenn du die Werte ausließt, sollten sie höher sein sobald du die jeweilige Mandarine berührst. Überlege dir hierzu für jede Mandarine einen sogenannten Schwellenwert. Also einen Wert bei dem du entscheiden ob die Mandarine berührt wird (Messwert > Schwellenwert) oder nicht berührst (Messwert < Schwellenwert). 

## Komplexe Version Teil 2
Wie zuvor solltest du den Bildschirm auch hier beim Start ausschalten. In einer dauerhaft- Schleife kannst du jetzt mit einer Wenn-Dann-Bedingung prüfen ob, die gemessenen Werte an den einzelnen Pins über oder unter deinem festgelegten Schwellwert liegen.

```blocks

led.enable(false)
basic.forever(function () {
    if (pins.analogReadPin(AnalogPin.P1) > 400) {
        basic.setLedColor(0xff0000)
        music.playTone(262, music.beat(BeatFraction.Whole))
    } else if (pins.analogReadPin(AnalogPin.P2) > 400) {
        basic.setLedColor(0xffff00)
        music.playTone(294, music.beat(BeatFraction.Whole))
    } else if (pins.analogReadPin(AnalogPin.C4) > 400) {
        basic.setLedColor(0x00ff00)
        music.playTone(294, music.beat(BeatFraction.Whole))
    } else if (pins.analogReadPin(AnalogPin.C5) > 300) {
        basic.setLedColor(0x007fff)
        music.playTone(330, music.beat(BeatFraction.Whole))
    } else if (pins.analogReadPin(AnalogPin.C6) > 250) {
        basic.setLedColor(0xff8000)
        music.playTone(330, music.beat(BeatFraction.Whole))
    } else if (pins.analogReadPin(AnalogPin.C16) > 250) {
        basic.setLedColor(0x7f00ff)
        music.playTone(349, music.beat(BeatFraction.Whole))
    } else if (pins.analogReadPin(AnalogPin.C17) > 250) {
        basic.setLedColor(0xff9da5)
        music.playTone(392, music.beat(BeatFraction.Whole))
    } else {
        basic.setLedColor(0x000000)
    }
})

```
## ~@unplugged
Dein Klavier hat immer noch nicht genug Tasten? <br>
Du kannst die Pins P0, P1 und P3 zusätzlich noch nutzen wie in der einfachen Variante. Musst für diese aber wieder den Minuspol berühren!
