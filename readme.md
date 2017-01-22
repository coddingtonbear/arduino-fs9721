# Arduino interface for your FS9721-based multimeter

Do you have a cheap Chinese multimeter using the FS9721 chip and want
to read the convert the strange serial protocol into something useful?
This project gives you an easy way to read measurements from your multimeter
if you have connected it to your Arduino.

## Compatible Multimeters

Any multimeter using the FS9721 chipset is compatible; more exist than are
below, but known-compatible multimeters include:

* **RECOMMENDED** [TekPower TP4000ZC](https://www.amazon.com/Tekpower-TP4000ZC-Interfaced-Multimeter-Computer/dp/B000OPDFLM)
    * Also available as the Digitek DT-4000ZC.
* [Mastech V&A 18B](https://www.amazon.com/Manual-Ranging-Digital-Multimeter-Interface/dp/B000LQONYM)
* [UNI-T UT60E](https://www.amazon.com/UNI-T-UT60E-Precise-Light-Weight/dp/B0152OJAWC)


## Example

```c
#define MM_RX 2
// TX is never used, so you can set this to anything you'd like
#define MM_TX 3

SoftwareSerial mm = SoftwareSerial(MM_RX, MM_TX, true);
FS9721 fs9721 = FS9721(&mm);

void setup() {
    mm.begin(2400);
    Serial.begin(9600);
}

void loop() {
    while(mm.available()) {
        if(fs9721.update()) {
            // You have a new measurement!
            Serial.println(fs9721.getValue());
        }
    }
}
```
