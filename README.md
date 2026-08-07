# Pathfinder

<img width="733" height="611" alt="image" src="https://github.com/user-attachments/assets/fc7a2fe0-a7a5-4bca-8948-eb34f9dd7004" />

This is a USB-C powered 2-layer flight controller built around the STM32F722. It's 2S LiPo/Li-ion battery, a 6-axis IMU, a barometer, microSD logging, and two servo outputs. Lots of the sensors I used on this haven't been seen on much other flight controllers including the one I used the guide from. Feel free to use this for hobbyist and competition rocketry!

## List of Materials

- MCU: STM32F722RET6
- IMU: ICM-20948 (SPI) — barometer: BME280 (I2C)
- Charger: BQ25887
- Power: TPS63070 buck-boost, LMR51430 buck
- microSD: 4-bit SD card slot
- J1/J3: servo/PWM headers
- J2: 2S battery screw terminal
- SW1 reset, SW2 boot, D1 status LED indicator
- 25MHz HSE + 32.768kHz LSE crystals
- Board: 50x55mm, 2-layer, KiCad 
- the hardware folder has the KiCad files. Run lcsc.py first to get parts KiCAD didn't have. This came from the hack club guide.

I decided to implement most of these parts myself to differentiate this project with the guide I followed. This, in turn, made it a little more difficult but I'm extremely excited to get it done!

## Power architecture

```
2S Battery ----> TPS63070 ----> VSYS ----> LMR51430 ----> 3.3V rail

USB-C VBUS ----> BQ25887 Charger
```

## Bill of Materials

| Designator(s)               | Qty. | Value / Part                      | LCSC Part # | Purchase Link                                              | Approx. Cost |
| --------------------------- | ---: | --------------------------------- | ----------- | ---------------------------------------------------------- | -----------: |
| C1, C18–C21, C26–C30, C39   |   11 | 100 nF, 0402                      | C1525       | [LCSC](https://www.lcsc.com/product-detail/C1525.html)     |        $1.00 |
| C9–C11                      |    3 | 22 µF, 0805                       | C129302     | [LCSC](https://www.lcsc.com/product-detail/C129302.html)   |        $2.00 |
| C3, C4, C6, C8, C12–C14     |    7 | 10 µF, 0402                       | C7472949    | [LCSC](https://www.lcsc.com/product-detail/C7472949.html)  |        $3.00 |
| C15                         |    1 | 2.2 µF, 0402                      | C77002      | [LCSC](https://www.lcsc.com/product-detail/C77002.html)    |        $0.50 |
| C16, C17                    |    2 | 22 µF, 0805                       | C129302     | [LCSC](https://www.lcsc.com/product-detail/C129302.html)   |        $1.50 |
| C2                          |    1 | 47 µF, 0805                       | C76636      | [LCSC](https://www.lcsc.com/product-detail/C76636.html)    |        $1.50 |
| C22, C24                    |    2 | 100 µF, 1210                      | C90143      | [LCSC](https://www.lcsc.com/product-detail/C90143.html)    |        $3.00 |
| C25                         |    1 | 2.2 µF, 0402                      | C77002      | [LCSC](https://www.lcsc.com/product-detail/C77002.html)    |        $0.50 |
| C5, C31–C34                 |    5 | 1 µF, 0402                        | C52923      | [LCSC](https://www.lcsc.com/product-detail/C52923.html)    |        $1.50 |
| C35, C36                    |    2 | 6.8 pF, 0402                      | C6376026    | [LCSC](https://www.lcsc.com/product-detail/C6376026.html)  |        $0.50 |
| C37, C38                    |    2 | 22 pF, 0402                       | C1555       | [LCSC](https://www.lcsc.com/product-detail/C1555.html)     |        $0.50 |
| C7                          |    1 | 47 µF, 0805                       | C76636      | [LCSC](https://www.lcsc.com/product-detail/C76636.html)    |        $1.50 |
| CARD1                       |    1 | DM3C-SF microSD socket            | C3177034    | [LCSC](https://www.lcsc.com/product-detail/C3177034.html)  |        $3.00 |
| D1                          |    1 | Green LED, 0402                   | C130723     | [LCSC](https://www.lcsc.com/product-detail/C130723.html)   |        $0.50 |
| J1, J3                      |    2 | PH2.54 2×3-pin headers            | C42431837   | [LCSC](https://www.lcsc.com/product-detail/C42431837.html) |        $2.00 |
| J2                          |    1 | XY308 three-pin terminal block    | C557686     | [LCSC](https://www.lcsc.com/product-detail/C557686.html)   |        $2.00 |
| L1                          |    1 | 1 µH power inductor               | C285919     | [LCSC](https://www.lcsc.com/product-detail/C285919.html)   |        $2.00 |
| L2                          |    1 | 1.5 µH power inductor             | C5289323    | [LCSC](https://www.lcsc.com/product-detail/C5289323.html)  |        $3.00 |
| L3                          |    1 | 5.6 µH inductor                   | C5809216    | [LCSC](https://www.lcsc.com/product-detail/C5809216.html)  |        $2.00 |
| L4                          |    1 | FCM1608KF-601T03 ferrite bead     | C141723     | [LCSC](https://www.lcsc.com/product-detail/C141723.html)   |        $1.00 |
| R1, R2                      |    2 | 5.1 kΩ, 0402                      | C25905      | [LCSC](https://www.lcsc.com/product-detail/C25905.html)    |        $1.00 |
| R10                         |    1 | 5.1 kΩ, 0402                      | C25905      | [LCSC](https://www.lcsc.com/product-detail/C25905.html)    |        $0.50 |
| R11                         |    1 | 390 Ω, 0402                       | C25109      | [LCSC](https://www.lcsc.com/product-detail/C25109.html)    |        $0.50 |
| R12                         |    1 | 22 Ω, 1 W balancing resistor      | C3921908    | [LCSC](https://www.lcsc.com/product-detail/C3921908.html)  |        $2.00 |
| R4–R6, R8, R13–R15, R18–R24 |   14 | 10 kΩ, 0402                       | C25744      | [LCSC](https://www.lcsc.com/product-detail/C25744.html)    |        $3.00 |
| R16                         |    1 | 22 kΩ, 0402                       | C25768      | [LCSC](https://www.lcsc.com/product-detail/C25768.html)    |        $0.50 |
| R17                         |    1 | 100 kΩ, 0402                      | C25741      | [LCSC](https://www.lcsc.com/product-detail/C25741.html)    |        $0.50 |
| R3                          |    1 | 2.2 kΩ, 0402                      | C25879      | [LCSC](https://www.lcsc.com/product-detail/C25879.html)    |        $0.50 |
| R7                          |    1 | 300 Ω, 0402                       | C25102      | [LCSC](https://www.lcsc.com/product-detail/C25102.html)    |        $0.50 |
| R9                          |    1 | 30.1 kΩ precision resistor        | C5842805    | [LCSC](https://www.lcsc.com/product-detail/C5842805.html)  |        $1.00 |
| SW1, SW2                    |    2 | TS-1088-AR02016 buttons           | C720477     | [LCSC](https://www.lcsc.com/product-detail/C720477.html)   |        $2.00 |
| U1                          |    1 | TPS63070RNMR buck-boost converter | C109322     | [LCSC](https://www.lcsc.com/product-detail/C109322.html)   |        $5.00 |
| U3                          |    1 | ICM-20948 motion sensor           | C726001     | [LCSC](https://www.lcsc.com/product-detail/C726001.html)   |       $18.00 |
| U4                          |    1 | BME280 environmental sensor       | C92489      | [LCSC](https://www.lcsc.com/product-detail/C92489.html)    |        $8.00 |
| U5                          |    1 | BQ25887RGET battery charger       | C2862617    | [LCSC](https://www.lcsc.com/product-detail/C2862617.html)  |        $9.00 |
| U6                          |    1 | LMR51430XDDCR buck converter      | C5185863    | [LCSC](https://www.lcsc.com/product-detail/C5185863.html)  |        $3.00 |
| U7                          |    1 | STM32F722RET6 microcontroller     | C118207     | [LCSC](https://www.lcsc.com/product-detail/C118207.html)   |       $18.00 |
| USB1                        |    1 | 16-pin USB Type-C connector       | C2765186    | [LCSC](https://www.lcsc.com/product-detail/C2765186.html)  |        $3.00 |
| X1                          |    1 | X322525MOB4SI crystal             | C9006       | [LCSC](https://www.lcsc.com/product-detail/C9006.html)     |        $2.00 |
| X2                          |    1 | Q13FC1350000400 crystal           | C32346      | [LCSC](https://www.lcsc.com/product-detail/C32346.html)    |        $2.00 |
|                             |      |                                   |             | **Estimated component subtotal**                           |  **$112.50** |
|                             |      |                                   |             | **Shipping, taxes and price-change buffer**                |  **$11.50** |
|                             |      |                                   |             | **Estimated total budget**                                 |  **$124.00** |

> Prices are intentionally conservative estimates and may differ from the final LCSC cart because of stock changes, minimum-order quantities, taxes and shipping.

## How To Make It Yourself

1. Run `lcsc.py` so the libraries resolve.
2. Open the .kicad_pro file in KiCad.
3. Export gerbers/drill/pick-and-place and send to JLCPCB for a quote.
4. Generate a BOM and assemble with a stencil, paste, and reflow.
5. Power up: 2S LIPO pack on the battery pad, `J2`.
6. Flash: hold `SW2`, reset with `SW1`, flash over `USB1`.

Note for shippers: The gerber is SimpleFlightController.zip in the production folder with the bom.csv
