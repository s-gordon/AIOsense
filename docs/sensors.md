# Sensors

The AIOsense PCB support and hand full of modular sensors. Chose any sensor you
want but notice the compatibility.

<hr>

## 🌡️Temperature, Humidity, Pressure, VOC & Air Quality

For these measurements we recommend the industrial sensors BME680/BME688 or the
BME280 (cheaper but without air
quality).

Please notice that you can either use the BME280 **or** the BME680 out of the
box (there have the same I²C address).

If you can solder SMD components you can solder them directly onto the PCB.
Please notice that you will need and solder
these three more components: 2x 4K7 resistor (THT), 1x 100nF capacitor (also
SMD). Also important to know: It's not
recommended to solder the BME680 with hot air because this can damage the air
quality sensor. Use a hotplate instead.

Related to [#7](https://github.com/Schluggi/AIOsense/issues/7) sometimes SMD
soldering can be inaccessible or difficult.
You can use a BME PCB module instead.

The default offset is -6 °C. 

### Where to buy?

| Part                | Shops                                                                                                                                                                                                                                                                                                   |
|---------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| BME280 (PCB module) | [Amazon DE³](https://amzn.to/3KXklmd), [AliExpress](https://de.aliexpress.com/wholesale?catId=0&initiative_id=SB_20221127130157&SearchText=BME280+&spm=a2g0o.home.1000002.0&dida=y)                                                                                                                     |
| BME280 (SMD)        | [Reichelt](https://www.reichelt.de/kombo-sensor-luftdruck-luftfeuchtigkeit-temp--bme-280-p159825.html), [Mouser](https://www.mouser.de/ProductDetail/Bosch-Sensortec/BME280?qs=2OnyuXx6vpj2fK9HX7qb3g%3D%3D), [Digi-Key](https://www.digikey.de/de/products/detail/bosch-sensortec/BME280/6136306)      |
| BME680 (PCB module) | [Amazon DE³](https://amzn.to/41y0XmS), [AliExpress](https://de.aliexpress.com/wholesale?catId=0&initiative_id=SB_20221127130036&SearchText=BME680+&spm=a2g0o.home.1000002.0&dida=y)                                                                                                                     |
| BME680 (SMD)        | [Reichelt](https://www.reichelt.de/kombo-sensor-luftdruck-luftfeuchtigkeit-temp-gas-bme-680-p159835.html), [Mouser](https://www.mouser.de/ProductDetail/Bosch-Sensortec/BME680?qs=v271MhAjFHjo0yA%2FC4OnDQ%3D%3D), [Digi-Key](https://www.digikey.de/de/products/detail/bosch-sensortec/BME680/7401317) |

### Compatibility

This sensor is fully compatible with any other sensor or module. You can only
choose _one_ BME module and loses one
generic I²C port if you choose an PCB module.


<hr>

## 💡Illuminance (Light)

You want to turn the light on by motion but only when it's necessary? So you
want to know, how bright it is? We
recommend the really cheap GY-302 BH1750 PCB.

### Where to buy?

| Part   | Shops                                                                                                                                                                                     |
|--------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| BH1750 | [Amazon DE³](https://amzn.to/3oAc6Vy), [AliExpress](https://de.aliexpress.com/wholesale?catId=0&initiative_id=SB_20221127121631&SearchText=BH1750&spm=a2g0o.productlist.1000002.0&dida=y) |

### Compatibility

This sensor is fully compatible with any other sensor or module but uses one of
the two generic I²C slots. If you want
to use the official 3D printed case you have to connect it to the lower slot.

<hr>

## 🎙️Microphone

> **Note:** The mic shares pins with the SPI header. If you fit a microphone you
> **cannot** use any generic device on the CS, SCK or MISO IO-header pins, nor
> any SPI based sensor.

A microphone gives the board voice assistant support, and together with a
speaker lets it answer. See [Audio & Voice Assistant](audio.md) for what each
ESPHome package provides, which boards support audio, and how to enable it.

Audio requires an ESP32-S2 mini or ESP32-S3 mini. The ESP32-C3 mini does not
break out the I2S data pins and cannot drive a microphone or speaker.

The AIOsense PCB is designed to work only with INMP441-modules which are known
for their high quality digital audio.

> **Warning:** On PCB v3.0.0-rc1 the microphone footprint has GND and VCC
> swapped ([issue #333](https://github.com/Schluggi/AIOsense/issues/333)). Check
> your wiring before debugging audio in software.

### Where to buy?

| Part    | Shops                                                                                                                                                                                      |
|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| INMP441 | [Amazon DE³](https://amzn.to/40y8twJ), [AliExpress](https://de.aliexpress.com/wholesale?catId=0&initiative_id=SB_20221127114658&SearchText=inmp441&spm=a2g0o.tm800107193.1000002.0&dida=y) |

### Compatibility
> **AIOsense v2.1.0**: This sensor is fully compatible with any other sensor or module besides the buzzer. Please disconnect the buzzer first. 

<hr>

## 🧍Presence (mmWave/radar)

Never heard before? It's a fancy technology to recognize the smallest
movements (like a motion sensor/PIR but really
sensitive). This is useful in situations you want to track if someone is present
even if the person is not really moving
for example while watching TV or sleeping.

The board is mainly made for the
SEN0395 [datasheet](https://wiki.dfrobot.com/mmWave_Radar_Human_Presence_Detection_SKU_SEN0395).
There are two sockets
for this sensor. Feel free to choose yourself which one do you prefer. It's just
the orientation. There is no difference
between these two, and you can't use both sockets at the same time.

Shortly before release of V2.0 someone asked to support the much cheaper module
HLK-LD2410 [datasheet](https://drive.google.com/drive/folders/1p4dhbEJA3YubyIjIIC7wwVsSo8x29Fq-?spm=a2g0o.detail.1000023.17.6dfa18b2xYoafU).
So I added a socket. This module is also supported in ESPHome but currently
there is no demo config available from our
side. So [build it yourself](https://esphome.io/components/sensor/ld2410.html).

### Where to buy?

| Part                  | Shops                                                                                                                                                                                                                                                                                                                 |
|-----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| SEN0395 (recommended) | [Mouser](https://www.mouser.de/ProductDetail/DFRobot/SEN0395?qs=ljCeji4nMDmvEgq75EdCVA%3D%3D), [Farnell](https://de.farnell.com/en-DE/dfrobot/sen0395/mmwave-radar-board-arduino-board/dp/3879712), [Digi-Key](https://www.digikey.de/de/products/detail/dfrobot/SEN0395/14322660?s=N4IgTCBcDaIMoFEByAGAzATgKwgLoF8g) |
| HLK-LD2410            | [AliExpress](https://de.aliexpress.com/item/1005004351593073.html)                                                                                                                                                                                                                                                    |
| HLK-LD2410C           | [AliExpress](https://de.aliexpress.com/item/1005005047841163.html)                                                                                                                                                                                                                                                    |

### Compatibility

This sensor is fully compatible with any other sensor or module, but you can
only use one of them so choose wisely.

<hr>

## 🚶Motion Sensor (PIR)

You want a classic motion sensor? No problem. We support an industrial standard
EKMC1603111 ([Datasheet](https://www3.panasonic.biz/ac/ae/search_num/index.jsp?c=detail&part_no=EKMC1603111)).
Every
other 3V PIR sensor with an TO-5 3-pin socket should work as well (but is
untested).
Please notice that you will need an TO-5 3-pin socket as well (in any case)!

### Where to buy?

| Part              | Shops                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| EKMC1603111       | [Voelkner](https://www.voelkner.de/products/994091/Panasonic-PIR-Bewegungssensor-EKMC1603111-3-6V-1St..html), [Mouser](https://www.mouser.de/ProductDetail/Panasonic-Industrial-Devices/EKMC1603111?qs=7jYh1P364wm%252bee2n5xwlWg%3D%3D), [Farnell](https://de.farnell.com/en-DE/panasonic-electric-works/ekmc1603111/sensor-motion-12m-white/dp/2095731?st=ekmc1603111), [Digi-Key](https://www.digikey.de/de/products/detail/panasonic-electric-works/EKMC1603111/2601880), [RS-Online](https://de.rs-online.com/web/p/naherungssensoren-ics/1357081) |
| TO-5 3-pin socket | [Mouser](https://www.mouser.de/ProductDetail/Mill-Max/917-43-103-41-005000?qs=teNCaa%2FZ3auQVxVMs%2F5ihg%3D%3D), [Digi-Key](https://www.digikey.com/en/products/detail/mill-max-manufacturing-corp/917-43-103-41-005000/1212170?s=N4IgTCBcDaIJwEYDsBaALAZhQgDFtCKOOArMTiALoC%2BQA)                                                                                                                                                                                                                                                                      |

### Compatibility

This sensor is fully compatible with any other sensor or module.

## 🔊 Piezo Buzzers

This is a passive buzzer which can create music in different frequencies.

### Where to buy?

| Part         | Shops                                                                                                                                       |
|--------------|---------------------------------------------------------------------------------------------------------------------------------------------|
| Piezo Buzzer | [Amazon DE³](https://amzn.to/3n0aFza), [Mouser](https://www.mouser.de/ProductDetail/CUI-Devices/CEM-120342?qs=WyjlAZoYn50KzVdoYfaqpA%3D%3D) |

> Please notice the pinout of 6.5mm and a max height of 8.5mm. Otherwise, the
> buzzer will not fit

### Compatibility

This module is fully compatible with any other sensor or module besides
HLK-LD2410. The HLK-LD2410C however will work fine.

<hr>
³ Affiliate link
