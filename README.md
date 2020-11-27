# Mandarinen Klavier
## ~avatar avatar @unplugged
gedulde dich noch ein wenig bis es soweit ist :-)

## einfache Version
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
led.enable(false)
```

## komplexere Version
```blocks
input.onButtonPressed(Button.A, function () {
    serial.writeLine("")
    serial.writeLine("" + (pins.analogReadPin(AnalogPin.P2)))
    serial.writeLine("" + (pins.analogReadPin(AnalogPin.C4)))
    serial.writeLine("" + (pins.analogReadPin(AnalogPin.C5)))
    serial.writeLine("" + (pins.analogReadPin(AnalogPin.C6)))
    serial.writeLine("" + (pins.analogReadPin(AnalogPin.C16)))
    serial.writeLine("" + (pins.analogReadPin(AnalogPin.C17)))
})
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
