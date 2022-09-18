# canbus sniffing a 2013 victory hammer

## Overview

I had a 2013 Victory Hammer 8-Ball - excellent bike.  Unfortunately, it died and I parted it out.  I was able to sell the vast majority of the parts except for the ECU and Speedometer.  I decided I wanted to use the Speedometer for my own purposes, but found the interface used Canbus to trigger the majority of the features of the speedometer.  This led me to try and figure out exactly how it worked.  My research and experimentation is documented here.

Please note: everything on here is from my research.  It may be wrong, it likely will have errors, and it probably will result in breaking my ECU and/or speedometer.  Anything you do with this is a risk to your motorcycle and will likely result in bad things happening.  Use this information at your own risk.

## Hardware

The power supply is a PC power supply which provides various voltage outputs: +5v +12v as well as a couple others (which I'm not currently using).

The speedometer has a 16 pin connector on the back, which appears to have the following pinouts:

1. CAN high [yellow]
2. CAN low [green]
3. Switched power +12v [pink]
4. Constant power +12v [orange]
5. Ground [black/white]
6. Flasher light [blue/purple]
	* floating => light off
	* +5v => light on
7. No connection
8. High beam light [dark green]
	* floating => light off
	* +5v => light on
9. Neutral light [black/pink]
	* floating => light off
	* Ground => light on
10. Oil pressure sensor [black/brown]
	* floating => oil pressure ok
	* Ground => display shows low oil
11. No connection
12. Mode switch [white/black]
	* floating => button not pressed
	* Ground => button pressed
13. Fuel light [black/light green]
	* unknown.   I think it needs to be a variable resistor to ground, but haven't figured it out yet 
14. Cruise control light [white/brown]
	* floating => light off
	* Ground => light on
15. Air temperature [orange/light blue]
	* unknown.   I think it needs to be a variable resistor to ground, but haven't figured it out yet 
16. No connection

The ECU has a 60-pin connector, which appears to have the following pinouts:

1. Power +12v
2. No connection
3. Ignition coil A ?
4. Ignition coil B ? 
5. unknown
6. Idle air control 1 ?
7. Idle air control 2 ?
8. Idle air control 3 ? 
9. Idle air control 4 ?
10. unknown
11. Power +12v
12. Ground
13. unknown
14. unknown
15. unknown
16. unknown
17. unknown
18. FEPS ?
19. Purge valve ?
20. unknown
21. Speedometer switched power ?
22. Tachometer switched power ?
23. neutral indicator ?
24. overdrive indicator ?
25. fuel injector 1 ?
26. fuel injector 2 ?
27. unknown
28. Fuel pump ?
29. unknown
30. unknown
31. Power +12v
32. unknown
33. Ground
34. Power +12v
35. unknown
36. O2 sensor 1 ?
37. O2 sensor 2 ?
38. IAT ?
39. CHT ?
40. TOS ?
41. Ground
42. VSSH ?
43. unknown
44. unknown
45. unknown
46. SIGRTN ?
47. Canbus high
48. Canbus low
49. unknown
50. unknown
51. NCS ?
52. GSS ? 
53. TPS ?
54. MAP ?
55. Power +5v
56. unknown
57. Crank Position + ?
58. Crank Position - ?
59. unknown
60. KAPWR ?

At this time, most wires are floating, with the exception of the following:

* +12v Power
	* ECU-1, ECU-11, ECU-31, ECU-34, S-3, S-4
* +5v Power
	* ECU-55
* Ground
	* ECU-12, ECU-33, ECU-41, S-5
* Canbus
	* ECU-47, ECU-48, S-1, S-2

The canbus connections also feed into a canbus-shield for an arduino.  No other arduino connections are currently used.

The arduino code will be added to this repo at a later time.

## General settings

canbus speed: 250 kbps
canbus clock: 16 MHz

Note: I was also able to get similar (identical?) data by sniffing at 125 kbps / 8MHz. 

## Sniffed frames at startup
```
14:21:22.574 -> > 0x18EEFF00 => 00 00 00 00 00 00 00 00
14:21:22.606 -> > 0x18FEF200 => 00 00 FF FF FF FF 00 FF
14:21:22.672 -> > 0x18FEF100 => 3F 00 00 FF FF FF FF FF
14:21:22.704 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:22.737 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:22.803 -> > 0x18FEF100 => 3F 00 00 FF FF FF FF FF
14:21:22.835 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:22.868 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:22.901 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:22.966 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:22.999 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:23.032 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:23.098 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:23.130 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:23.163 -> > 0x18FEF100 => 3F 00 00 FF FF FF FF FF
14:21:23.229 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:23.261 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:23.294 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:23.327 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:23.393 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:23.425 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:23.458 -> > 0x18F00500 => FF FF FF FF 20 2D FF FF
14:21:23.524 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:23.556 -> > 0x18FEF100 => 3F 00 00 FF FF FF FF FF
14:21:23.589 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:23.655 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:23.687 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:23.720 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:23.753 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:23.818 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:23.851 -> > 0x18F00500 => FF FF FF FF 20 2D FF FF
14:21:23.884 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:23.949 -> > 0x18FEF100 => 3F 00 00 FF FF FF FF FF
14:21:23.982 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:24.015 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:24.081 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:24.113 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:24.146 -> > 0x18FEF600 => FF FF FF 27 FF FF FF FF
14:21:24.212 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:24.244 -> > 0x18F00500 => FF FF FF FF 20 2D FF FF
14:21:24.277 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:24.310 -> > 0x18FEF100 => 3F 00 00 FF FF FF FF FF
14:21:24.375 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:24.409 -> > 0x1CEB1700 => 04 57 56 31 46 47 48 31
14:21:24.441 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:24.507 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:24.539 -> > 0x1CEB1700 => 08 32 34 2A 34 30 31 33
14:21:24.572 -> > 0x1CEB1700 => 09 38 39 35 00 20 20 2A
14:21:24.637 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:24.670 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:24.703 -> > 0x18FEF100 => 3F 00 00 FF FF FF FF FF
14:21:24.736 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:24.801 -> > 0x1CECFF00 => 20 0E 00 02 FF CA FE 00
14:21:24.834 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:24.867 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:24.932 -> > 0x1CEBFF00 => 02 02 05 01 01 F0 E7 2D
14:21:24.965 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:24.998 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:25.064 -> > 0x1CECFF00 => 20 12 00 03 FF CA FE 00
14:21:25.095 -> > 0x18FEF100 => 3F 00 00 FF FF FF FF FF
14:21:25.129 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:25.162 -> > 0x1CEBFF00 => 02 02 05 01 8C 02 05 01
14:21:25.227 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:25.260 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:25.293 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:25.358 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:25.391 -> > 0x18F00500 => FF FF FF FF 20 2D FF FF
14:21:25.424 -> > 0x1CEBFF00 => 02 00 04 01 8B 02 05 01
14:21:25.489 -> > 0x18FEF100 => 3F 00 00 FF FF FF FF FF
14:21:25.522 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:25.555 -> > 0x1CEBFF00 => 04 2D FF FF FF FF FF FF
14:21:25.588 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:25.653 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:25.686 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:25.719 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:25.785 -> > 0x18F00500 => FF FF FF FF 20 2D FF FF
14:21:25.817 -> > 0x1CECFF00 => 20 1E 00 05 FF CA FE 00
14:21:25.850 -> > 0x18FEF100 => 3F 00 00 FF FF FF FF FF
14:21:25.916 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:25.948 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:25.981 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:26.014 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:26.079 -> > 0x1CEBFF00 => 04 01 8C 02 05 01 01 F0
14:21:26.112 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:26.145 -> > 0x18F00500 => FF FF FF FF 20 2D FF FF
14:21:26.210 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:26.243 -> > 0x18FEF100 => 3F 00 00 FF FF FF FF FF
14:21:26.276 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:26.342 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:26.374 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:26.407 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:26.440 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:26.505 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:26.538 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:26.571 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:26.637 -> > 0x18FEF100 => 3F 00 00 FF FF FF FF FF
14:21:26.669 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:26.702 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:26.767 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:26.800 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:26.833 -> > 0x1CECFF00 => 20 1E 00 05 FF CA FE 00
14:21:26.898 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:26.931 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:26.964 -> > 0x1CEBFF00 => 02 00 03 01 33 00 04 01
14:21:26.996 -> > 0x18FEF100 => 3F 00 00 FF FF FF FF FF
14:21:27.062 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:27.095 -> > 0x1CEBFF00 => 04 01 8C 02 05 01 01 F0
14:21:27.128 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:27.193 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:27.226 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:27.259 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:27.324 -> > 0x18F00500 => FF FF FF FF 20 2D FF FF
14:21:27.357 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:27.390 -> > 0x18FEF100 => 3F 00 00 FF FF FF FF FF
14:21:27.423 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:27.488 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:27.520 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:27.554 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:27.619 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:27.652 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:27.685 -> > 0x18F00500 => FF FF FF FF 20 2D FF FF
14:21:27.750 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:27.783 -> > 0x18FEF100 => 3F 00 00 FF FF FF FF FF
14:21:27.816 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:27.849 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:27.914 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:27.947 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:27.980 -> > 0x1CEBFF00 => 02 00 03 01 33 00 04 01
14:21:28.045 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:28.078 -> > 0x18F00500 => FF FF FF FF 20 2D FF FF
14:21:28.111 -> > 0x1CEBFF00 => 04 01 8C 02 05 01 01 F0
14:21:28.176 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:28.209 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:28.242 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:28.275 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:28.340 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:28.373 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:28.406 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:28.471 -> > 0x18F00500 => FF FF FF FF 20 2D FF FF
14:21:28.504 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:28.537 -> > 0x18FEF100 => 3F 00 00 FF FF FF FF FF
14:21:28.602 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:28.635 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:28.668 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:28.701 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:28.766 -> > 0x18F00500 => FF FF FF FF 20 2D FF FF
14:21:28.799 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:28.832 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:28.897 -> > 0x1CECFF00 => 20 1E 00 05 FF CA FE 00
14:21:28.930 -> > 0x18FEF100 => 3F 00 00 FF FF FF FF FF
14:21:28.963 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:29.028 -> > 0x1CEBFF00 => 02 00 03 01 33 00 04 01
14:21:29.060 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:29.094 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:29.127 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:29.192 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:29.225 -> > 0x18F00500 => FF FF FF FF 20 2D FF FF
14:21:29.258 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:29.321 -> > 0x18FEF200 => 00 00 FF FF FF FF 00 FF
14:21:29.356 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:29.389 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:29.454 -> > 0x18FEEE00 => 46 FF FF FF FF FF FF FF
14:21:29.487 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:29.520 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:29.585 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:29.618 -> > 0x18F00500 => FF FF FF FF 20 2D FF FF
14:21:29.651 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:29.684 -> > 0x18FEF200 => 00 00 FF FF FF FF 00 FF
14:21:29.749 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:29.782 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:29.815 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:29.880 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:29.913 -> > 0x1CECFF00 => 20 1E 00 05 FF CA FE 00
14:21:29.946 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:30.011 -> > 0x18F00500 => FF FF FF FF 20 2D FF FF
14:21:30.044 -> > 0x1CEBFF00 => 02 00 03 01 33 00 04 01
14:21:30.077 -> > 0x18FEF200 => 00 00 FF FF FF FF 00 FF
14:21:30.109 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:30.175 -> > 0x1CEBFF00 => 04 01 8C 02 05 01 01 F0
14:21:30.208 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:30.241 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:30.306 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:30.339 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:30.372 -> > 0x18F00500 => FF FF FF FF 20 2D FF FF
14:21:30.437 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:30.470 -> > 0x18FEF200 => 00 00 FF FF FF FF 00 FF
14:21:30.503 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:30.535 -> > 0x0CF00400 => FF FF FF 00 00 FF FF FF
14:21:30.601 -> > 0x18F00500 => FF FF FF FF 20 2D FF FF
14:21:30.634 -> > 0x18F00500 => FF FF FF FF 20 2D FF FF
```

## Message Testing
  Key: 
    * Is the sniffed base
    + is my specific guesses
    - is my resulting analysis based on test frames sent
    
  Note: often I will cause some sort of error state which breaks the responsiveness of the ECU/Speedometer.  Therefore, all analysis are done at startup as well as injected dynamically.

### 0x 0CF0 0400 - tachometer

```
* FF FF FF 00 00 FF FF FF - ?
+ 00 00 00 00 00 00 00 00 - ?
+ 12 34 56 78 87 65 43 21
	all values are decimal (not hex), tach reads 2750 rpm
+ 0c 22 38 4e 57 41 2b 15
	hex (from above), tach reads 2750 rpm
+ fe dc ba 98 76 54 32 10
	tach reads 3750 rpm

- xx 00 00 00 00 00 00 00 - ?
- 00 xx 00 00 00 00 00 00 - ?
- 00 00 xx 00 00 00 00 00 - ?
- 00 00 00 xx 00 00 00 00 - ?
- 00 00 00 00 xx 00 00 00 - tachometer RPMs 
	tach in 32 rpm intervals.  xx*32 truncated to 50 rpm.
	example 1: 87 (0x57) => floor50(87 * 32) = floor50(2785) => 2750 rpm
	example 2: 118 (0x76) => floor50(118 * 32) = floor50(3776) => 3750 rpm
	example 3: 99 => floor50(99 * 32) = floor50(3168) => 3150 rpm
- 00 00 00 00 00 xx 00 00 - ?
- 00 00 00 00 00 00 xx 00 - ?
- 00 00 00 00 00 00 00 xx - ?
```

### 0x 18F0 0500 - engine codes?  I got several messages to make the engine light stay off
```
* FF FF FF FF 20 2D FF FF - ?
+ 00 00 00 00 00 00 00 00 - check engine light is off.

- xx 00 00 00 00 00 00 00 - ?
- 00 xx 00 00 00 00 00 00 - ?
- 00 00 xx 00 00 00 00 00 - ?
- 00 00 00 xx 00 00 00 00 - ?
- 00 00 00 00 xx 00 00 00 - ?
- 00 00 00 00 00 xx 00 00 - ?
- 00 00 00 00 00 00 xx 00 - ?
- 00 00 00 00 00 00 00 xx - ?
```

### * 0x 18FE F100 - speedometer + engine codes?
```
* 3F 00 00 FF FF FF FF FF - check engine light is on.
+ 3F 00 00 00 00 00 00 00 - check engine light is off.

- xx 00 00 00 00 00 00 00 - ?
- 00 xx 00 00 00 00 00 00 - ?
- 00 00 xx 00 00 00 00 00 - speedometer speed
	set speed to xx km/h.  0x00-0xFA (0xFB-0xFF rendered as 0 km/h)
- 00 00 00 xx 00 00 00 00 - ?
- 00 00 00 00 xx 00 00 00 - ?
- 00 00 00 00 00 xx 00 00 - ?
- 00 00 00 00 00 00 xx 00 - ?
- 00 00 00 00 00 00 00 xx - ?
```

### * 0x 18FE F200 - ?
```
* 00 00 FF FF FF FF 00 FF - ?
+ 00 00 00 00 00 00 00 00 - ?

- xx 00 00 00 00 00 00 00 - ?
- 00 xx 00 00 00 00 00 00 - ?
- 00 00 xx 00 00 00 00 00 - ?
- 00 00 00 xx 00 00 00 00 - ?
- 00 00 00 00 xx 00 00 00 - ?
- 00 00 00 00 00 xx 00 00 - ?
- 00 00 00 00 00 00 xx 00 - ?
- 00 00 00 00 00 00 00 xx - ?
```

## Notes:

Mileage is 10461 (0x28DD).   I'm not seeing these values in the sniffed frames. I'm not sure the mileage / other settings are stored on the speedometer or the ECU. 

The check-engine lamp appears to be stateful.  Once it's on, it seems that only a power-cycle turns it off. 