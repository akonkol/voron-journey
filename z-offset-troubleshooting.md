## Printer Info
- [Voron 2.4r2 300mm x 300mm (LDO Motors Kit)](https://www.fabreeko.com/collections/v2-4/products/ldo-voron-v2-4-kit)
- [Afterburner Toolhead](https://github.com/VoronDesign/Voron-Afterburner)
- [Klicky NG probe](https://github.com/jlas1/Klicky-Probe/tree/main/Probes/KlickyNG)
- [Kinematic Bed Mount](https://github.com/tanaes/whopping_Voron_mods/tree/main/kinematic_bed)
- [Honeybadger PEI Build Plate](https://www.fabreeko.com/collections/fabreeko/products/honeybadger-v2-4-single-sided-black-pei-textured)
- [Bondtech LGX Lite Extruder](https://www.bondtech.se/product/lgx-lite-extruder-no-motor/)
- [LDO 20mm Pancake Extruder motor](https://www.bondtech.se/product/ldo-nema14-round-20mm-pancake-motor/)
- PIF Provided functional parts


## 1.0 Determing position_endstop drift between prints
### Problem statement:
When probing the bed with Klicky when the printer is hot I recieve "probe inaccuracy" like messages meaning that the
distance measured by the probe to the bed is steadily increasing (for example 1.1 => 1.2 => 1.4) or the margin of error in the readings is too high (for example: 1.1 => 2.0 => 1.0 => 1.4). 

### Hypothesis:
When performing *movement* of the gantry some component within the motion system becomes loose impacting the z motion system which is impacting klicky probe readings. 

### Procedure:
This first experiment runs qgl procedures *cold*, observing if there are any "retry" errors. Run this procedure 10 times, if there is not a significant amount of retries then move on to the next experiment.
I think for this test 2 individual retries is acceptable.

Run the following code:
```
[gcode_macro QRUN]
gcode:
  {% set count = params.TEST_COUNT|int %}
  M118 Performing QGL obsevation procedure {count} times....
  {% for i in range(count) %}
  G28
  QUAD_GANTRY_LEVEL
  G28
  {% endfor %}
  M118 Finished...
```

### Results:
There were a total of 7 `probe samples exceed samples_tolerance` within 7 runs before QGL gives up and throws an exception. We were not able to get a full set of 10 QGL runs.
 
3 Probes per corner, order of corner probing:
2     3
1     4

```
By run #7 probe samples exceed samples_tolerance

20:50:18
Probe samples exceed samples_tolerance
20:50:18
Probe samples exceed samples_tolerance
20:50:18
Probe samples exceed samples_tolerance
20:50:18
probe at 50.000,25.000 is z=7.432500
20:50:17
probe at 50.000,25.000 is z=7.406250
20:50:16
probe at 50.000,25.000 is z=7.406250
20:50:15
Probe samples exceed tolerance. Retrying... #strike 7 corner 1
20:50:14
probe at 50.000,25.000 is z=7.431250
20:50:13
probe at 50.000,25.000 is z=7.406250
20:50:12
probe at 50.000,25.000 is z=7.406250
20:50:11
Probe samples exceed tolerance. Retrying... #strike 6 corner 1
20:50:11
probe at 50.000,25.000 is z=7.433750
20:50:10
probe at 50.000,25.000 is z=7.406250
20:50:09
Probe samples exceed tolerance. Retrying... #strike 5 corner 1
20:50:09
probe at 50.000,25.000 is z=7.433750
20:50:07
probe at 50.000,25.000 is z=7.407500
20:50:03
probe: open
20:49:58
moving to a safe Z distance
20:49:58
Attaching Probe
20:49:57
probe: TRIGGERED
20:49:56
QG Level
20:49:33
probe: TRIGGERED
20:49:25
Docking Probe
20:49:25
Retries: 0/5 Probed points range: 0.005000 tolerance: 0.007500
20:49:25
Making the following Z adjustments:
stepper_z = -0.007750
stepper_z1 = 0.006975
stepper_z2 = -0.002225
stepper_z3 = 0.003000
20:49:25
Average: 2.583934
20:49:25
Actuator Positions:
z: 2.591684 z1: 2.576959 z2: 2.586160 z3: 2.580935
20:49:25
Gantry-relative probe points:
0: 2.587500 1: 2.582500 2: 2.583750 3: 2.583750
20:49:25
probe at 250.000,25.000 is z=7.417500
20:49:24
probe at 250.000,25.000 is z=7.416250
20:49:22
probe at 250.000,25.000 is z=7.413750
20:49:19
probe at 250.000,225.000 is z=7.416250
20:49:18
probe at 250.000,225.000 is z=7.417500
20:49:17
probe at 250.000,225.000 is z=7.416250
20:49:13
probe at 50.000,225.000 is z=7.417500
20:49:12
probe at 50.000,225.000 is z=7.417500
20:49:11
probe at 50.000,225.000 is z=7.416250
20:49:07
probe at 50.000,25.000 is z=7.413750
20:49:06
probe at 50.000,25.000 is z=7.412500
20:49:05
probe at 50.000,25.000 is z=7.412500
20:49:04
Probe samples exceed tolerance. Retrying... # strike 4 corner 1
20:49:04
probe at 50.000,25.000 is z=7.436250
20:49:03
probe at 50.000,25.000 is z=7.408750
20:48:58
probe: open
20:48:53
moving to a safe Z distance
20:48:53
Attaching Probe
20:48:53
probe: TRIGGERED
20:48:21
Retries: 0/5 Probed points range: 0.003750 tolerance: 0.007500
20:48:21
Making the following Z adjustments:
stepper_z = 0.000594
stepper_z1 = 0.005581
stepper_z2 = -0.005581
stepper_z3 = -0.000594
20:48:21
Average: 2.583125
20:48:21
Actuator Positions:
z: 2.582531 z1: 2.577544 z2: 2.588706 z3: 2.583719
20:48:21
Gantry-relative probe points:
0: 2.582500 1: 2.581250 2: 2.585000 3: 2.583750
20:48:21
probe at 250.000,25.000 is z=7.417500
20:48:19
probe at 250.000,25.000 is z=7.416250
20:48:18
probe at 250.000,25.000 is z=7.415000
20:48:15
probe at 250.000,225.000 is z=7.415000
20:48:14
probe at 250.000,225.000 is z=7.415000
20:48:12
probe at 250.000,225.000 is z=7.415000
20:48:09
probe at 50.000,225.000 is z=7.420000
20:48:08
probe at 50.000,225.000 is z=7.418750
20:48:07
probe at 50.000,225.000 is z=7.418750
20:48:03
probe at 50.000,25.000 is z=7.416250
20:48:02
probe at 50.000,25.000 is z=7.417500
20:48:01
probe at 50.000,25.000 is z=7.422500
20:48:00
Probe samples exceed tolerance. Retrying... # strike 3 corner 1
20:48:00
probe at 50.000,25.000 is z=7.415000
20:47:59
probe at 50.000,25.000 is z=7.431250
20:47:57
probe at 50.000,25.000 is z=7.423750
20:47:53
probe: open
20:47:48
moving to a safe Z distance
20:47:47
Attaching Probe
20:47:47
probe: TRIGGERED
20:47:46
QG Level
20:47:15
Docking Probe
20:47:15
Retries: 0/5 Probed points range: 0.006250 tolerance: 0.007500
20:47:15
Making the following Z adjustments:
stepper_z = 0.001312
stepper_z1 = -0.011038
stepper_z2 = 0.008663
stepper_z3 = 0.001063
20:47:15
Average: 2.586470
20:47:15
Actuator Positions:
z: 2.585158 z1: 2.597508 z2: 2.577808 z3: 2.585408
20:47:15
Gantry-relative probe points:
0: 2.586250 1: 2.590000 2: 2.583750 3: 2.585000
20:47:15
probe at 250.000,25.000 is z=7.415000
20:47:14
probe at 250.000,25.000 is z=7.415000
20:47:13
probe at 250.000,25.000 is z=7.412500
20:47:09
probe at 250.000,225.000 is z=7.416250
20:47:08
probe at 250.000,225.000 is z=7.416250
20:47:07
probe at 250.000,225.000 is z=7.416250
20:47:04
probe at 50.000,225.000 is z=7.410000
20:47:02
probe at 50.000,225.000 is z=7.411250
20:47:01
probe at 50.000,225.000 is z=7.410000
20:46:58
probe at 50.000,25.000 is z=7.412500
20:46:57
probe at 50.000,25.000 is z=7.413750
20:46:56
probe at 50.000,25.000 is z=7.417500
20:46:51
probe: open
20:46:46
moving to a safe Z distance
20:46:46
Attaching Probe
20:46:46
probe: TRIGGERED
20:46:44
QG Level
20:46:21
probe: TRIGGERED
20:46:13
Docking Probe
20:46:13
Retries: 0/5 Probed points range: 0.002500 tolerance: 0.007500
20:46:13
Making the following Z adjustments:
stepper_z = 0.003578
stepper_z1 = -0.000103
stepper_z2 = -0.001084
stepper_z3 = -0.002391
20:46:13
Average: 2.582923
20:46:13
Actuator Positions:
z: 2.579345 z1: 2.583026 z2: 2.584007 z3: 2.585313
20:46:13
Gantry-relative probe points:
0: 2.581250 1: 2.582500 2: 2.583750 3: 2.583750
20:46:13
probe at 250.000,25.000 is z=7.416250
20:46:12
probe at 250.000,25.000 is z=7.416250
20:46:11
probe at 250.000,25.000 is z=7.415000
20:46:07
probe at 250.000,225.000 is z=7.416250
20:46:06
probe at 250.000,225.000 is z=7.416250
20:46:05
probe at 250.000,225.000 is z=7.416250
20:46:02
probe at 50.000,225.000 is z=7.417500
20:46:01
probe at 50.000,225.000 is z=7.417500
20:45:59
probe at 50.000,225.000 is z=7.416250
20:45:56
probe at 50.000,25.000 is z=7.417500
20:45:55
probe at 50.000,25.000 is z=7.418750
20:45:54
probe at 50.000,25.000 is z=7.422500
20:45:49
probe: open
20:45:44
moving to a safe Z distance
20:45:44
Attaching Probe
20:45:44
probe: TRIGGERED
20:45:42
QG Level
20:45:20
probe: TRIGGERED
20:45:11
Docking Probe
20:45:11
Retries: 0/5 Probed points range: 0.002500 tolerance: 0.007500
20:45:11
Making the following Z adjustments:
stepper_z = 0.003578
stepper_z1 = -0.000103
stepper_z2 = -0.001084
stepper_z3 = -0.002391
20:45:11
Average: 2.579173
20:45:11
Actuator Positions:
z: 2.575595 z1: 2.579276 z2: 2.580257 z3: 2.581563
20:45:11
Gantry-relative probe points:
0: 2.577500 1: 2.578750 2: 2.580000 3: 2.580000
20:45:11
probe at 250.000,25.000 is z=7.420000
20:45:10
probe at 250.000,25.000 is z=7.421250
20:45:09
probe at 250.000,25.000 is z=7.418750
20:45:06
probe at 250.000,225.000 is z=7.420000
20:45:04
probe at 250.000,225.000 is z=7.420000
20:45:03
probe at 250.000,225.000 is z=7.420000
20:45:00
probe at 50.000,225.000 is z=7.421250
20:44:59
probe at 50.000,225.000 is z=7.421250
20:44:58
probe at 50.000,225.000 is z=7.418750
20:44:54
probe at 50.000,25.000 is z=7.422500
20:44:53
probe at 50.000,25.000 is z=7.422500
20:44:52
probe at 50.000,25.000 is z=7.418750
20:44:47
probe: open
20:44:42
moving to a safe Z distance
20:44:42
Attaching Probe
20:44:42
probe: TRIGGERED
20:44:41
QG Level
20:44:18
probe: TRIGGERED
20:44:10
Docking Probe
20:44:10
Retries: 1/5 Probed points range: 0.002500 tolerance: 0.007500
20:44:10
Making the following Z adjustments:
stepper_z = -0.002500
stepper_z1 = -0.000125
stepper_z2 = 0.002500
stepper_z3 = 0.000125
20:44:10
Average: 2.588764
20:44:10
Actuator Positions:
z: 2.591264 z1: 2.588889 z2: 2.586264 z3: 2.588639
20:44:10
Gantry-relative probe points:
0: 2.590234 1: 2.588984 2: 2.587734 3: 2.588984
20:44:09
probe at 250.000,25.000 is z=7.412266
20:44:08
probe at 250.000,25.000 is z=7.411016
20:44:07
probe at 250.000,25.000 is z=7.409766
20:44:04
probe at 250.000,225.000 is z=7.411016
20:44:03
probe at 250.000,225.000 is z=7.412266
20:44:01
probe at 250.000,225.000 is z=7.419766
20:43:58
probe at 50.000,225.000 is z=7.411016
20:43:57
probe at 50.000,225.000 is z=7.411016
20:43:56
probe at 50.000,225.000 is z=7.412266
20:43:52
probe at 50.000,25.000 is z=7.409766
20:43:51
probe at 50.000,25.000 is z=7.409766
20:43:50
probe at 50.000,25.000 is z=7.408516
20:43:46
Retries: 0/5 Probed points range: 0.021250 tolerance: 0.007500
20:43:46
Making the following Z adjustments:
stepper_z = -0.022266
stepper_z1 = 0.000416
stepper_z2 = 0.019772
stepper_z3 = 0.002078
20:43:46
Average: 2.589690
20:43:46
Actuator Positions:
z: 2.611955 z1: 2.589274 z2: 2.569918 z3: 2.587612
20:43:46
Gantry-relative probe points:
0: 2.602500 1: 2.591250 2: 2.581250 3: 2.591250
20:43:46
probe at 250.000,25.000 is z=7.408750
20:43:45
probe at 250.000,25.000 is z=7.408750
20:43:44
probe at 250.000,25.000 is z=7.406250
20:43:41
probe at 250.000,225.000 is z=7.418750
20:43:39
probe at 250.000,225.000 is z=7.418750
20:43:38
probe at 250.000,225.000 is z=7.418750
20:43:35
probe at 50.000,225.000 is z=7.410000
20:43:34
probe at 50.000,225.000 is z=7.408750
20:43:32
probe at 50.000,225.000 is z=7.407500
20:43:29
probe at 50.000,25.000 is z=7.397500
20:43:28
probe at 50.000,25.000 is z=7.401250
20:43:27
probe at 50.000,25.000 is z=7.396250
20:43:22
probe: open
20:43:17
moving to a safe Z distance
20:43:17
Attaching Probe
20:43:17
probe: TRIGGERED
20:43:16
QG Level
20:42:53
probe: TRIGGERED
20:42:44
Docking Probe
20:42:44
Retries: 2/5 Probed points range: 0.002500 tolerance: 0.007500
20:42:44
Making the following Z adjustments:
stepper_z = -0.000719
stepper_z1 = 0.004269
stepper_z2 = -0.004269
stepper_z3 = 0.000719
20:42:44
Average: 2.816030
20:42:44
Actuator Positions:
z: 2.816749 z1: 2.811762 z2: 2.820299 z3: 2.815312
20:42:44
Gantry-relative probe points:
0: 2.816030 1: 2.814780 2: 2.817280 3: 2.816030
20:42:44
probe at 250.000,25.000 is z=7.183970
20:42:43
probe at 250.000,25.000 is z=7.183970
20:42:42
probe at 250.000,25.000 is z=7.178970
20:42:39
probe at 250.000,225.000 is z=7.182720
20:42:37
probe at 250.000,225.000 is z=7.182720
20:42:36
probe at 250.000,225.000 is z=7.182720
20:42:35
Probe samples exceed tolerance. Retrying... # strike 2 corner 3
20:42:35
probe at 250.000,225.000 is z=7.182720
20:42:34
probe at 250.000,225.000 is z=7.200220
20:42:33
Probe samples exceed tolerance. Retrying... # strike 1 corner 3
20:42:33
probe at 250.000,225.000 is z=7.182720
20:42:32
probe at 250.000,225.000 is z=7.198970
20:42:30
probe at 250.000,225.000 is z=7.198970
20:42:27
probe at 50.000,225.000 is z=7.185220
20:42:26
probe at 50.000,225.000 is z=7.185220
20:42:25
probe at 50.000,225.000 is z=7.183970
20:42:21
probe at 50.000,25.000 is z=7.185220
20:42:20
probe at 50.000,25.000 is z=7.182720
20:42:19
probe at 50.000,25.000 is z=7.183970
20:42:15
Retries: 1/5 Probed points range: 0.016250 tolerance: 0.007500
20:42:15
Making the following Z adjustments:
stepper_z = 0.026468
stepper_z1 = -0.012720
stepper_z2 = -0.001530
stepper_z3 = -0.012218
20:42:15
Average: 2.815875
20:42:15
Actuator Positions:
z: 2.789407 z1: 2.828594 z2: 2.817405 z3: 2.828093
20:42:15
Gantry-relative probe points:
0: 2.803303 1: 2.817053 2: 2.819553 3: 2.818303
20:42:15
probe at 250.000,25.000 is z=7.181697
20:42:14
probe at 250.000,25.000 is z=7.181697
20:42:13
probe at 250.000,25.000 is z=7.176697
20:42:09
probe at 250.000,225.000 is z=7.180447
20:42:08
probe at 250.000,225.000 is z=7.180447
20:42:07
probe at 250.000,225.000 is z=7.179197
20:42:03
probe at 50.000,225.000 is z=7.182947
20:42:02
probe at 50.000,225.000 is z=7.182947
20:42:01
probe at 50.000,225.000 is z=7.181697
20:41:57
probe at 50.000,25.000 is z=7.196697
20:41:56
probe at 50.000,25.000 is z=7.196697
20:41:55
probe at 50.000,25.000 is z=7.196697
20:41:51
Retries: 0/5 Probed points range: 0.300000 tolerance: 0.007500
20:41:51
Making the following Z adjustments:
stepper_z = -0.171697
stepper_z1 = -0.150797
stepper_z2 = 0.480922
stepper_z3 = -0.158428
20:41:51
Average: 2.815627
20:41:51
Actuator Positions:
z: 2.987324 z1: 2.966424 z2: 2.334704 z3: 2.974054
20:41:51
Gantry-relative probe points:
0: 2.957500 1: 2.861250 2: 2.657500 3: 2.908750
20:41:51
probe at 250.000,25.000 is z=7.091250
20:41:50
probe at 250.000,25.000 is z=7.091250
20:41:49
probe at 250.000,25.000 is z=7.088750
20:41:45
probe at 250.000,225.000 is z=7.342500
20:41:44
probe at 250.000,225.000 is z=7.342500
20:41:43
probe at 250.000,225.000 is z=7.342500
20:41:40
probe at 50.000,225.000 is z=7.138750
20:41:38
probe at 50.000,225.000 is z=7.138750
20:41:37
probe at 50.000,225.000 is z=7.137500
20:41:34
probe at 50.000,25.000 is z=7.047500
20:41:32
probe at 50.000,25.000 is z=7.042500
20:41:31
probe at 50.000,25.000 is z=7.037500
20:41:26
probe: open
20:41:21
moving to a safe Z distance
20:41:21
Attaching Probe
20:41:21
probe: TRIGGERED
20:41:20
QG Level
```
### Conclusion:
During run 7 I heard a "crackle" when moving in the Z direction on corner 1. I suspect there is a broken bearing, plastic part or something! Once that crackle was heard there was an increase in probe retries.


## 2.0 Determing position_endstop drift between qgl and firmware restarts#

### Problem statement:
When probing the bed with Klicky when the printer is hot I recieve "probe inaccuracy" like messages meaning that the
distance measured by the probe to the bed is steadily increasing (for example 1.1 => 1.2 => 1.4) or the margin of error in the readings is too high (for example: 1.1 => 2.0 => 1.0 => 1.4). 

### Hypothesis:
When performing *movement* of the gantry some component within the motion system becomes loose impacting the z motion system which is impacting klicky probe readings. 

### Procedure:
This experiment runs homing/qgl procedures *cold* and then requires me to test the z-offset using the traditional paper test. While moving a piece of paper underneath the nozzle I adjust the z-offset using the mainsail UI until I feel the first hint of resistance between
the nozzle and paper. That z-offset value is then saved to the endstop and the firmware is restarted.

[gcode_macro ESRUN]
gcode:
  M118 Performing endstop observation procedure...
  G28
  QUAD_GANTRY_LEVEL
  G28 
  PARKCENTERLOW
  M118 Place piece of paper on bed
  M118 Lower Z position to 0
  M118 Adjust Z-Offset until nozzle grazes paper
  M118 Save Z-Offset to Endstop (SAVE_CONFIG)

##Results:##

```
Run #1
stepper_z: position_endstop: 1.420
```

```
Run #2
stepper_z: position_endstop: 1.395
```

```
Run #3
stepper_z: position_endstop: 1.395
```

```
Run #4
stepper_z: position_endstop: 1.405
```

```
Run #5
stepper_z: position_endstop: 1.415
```

```
Run #6
stepper_z: position_endstop: 1.425
```

```
Run #7
stepper_z: position_endstop: 1.425
```

```
Run #8
stepper_z: position_endstop: 1.425
```

```
Run #9
stepper_z: position_endstop: 1.425
```

```
Run #10
stepper_z: position_endstop: 1.425
```

### Conclusion:
As the results show there was no significant difference detected in position_endstop values while running the home/qgl/home paper-test *cold*


## 3.0 Try to figure out what that noise was
During experiment number 1 there was a "crackle" noise observed in corner 1 and an increase in failed probe measurements. My intiuition is that that sound is associated with a mechanical failure or friction that is throwing off z motion when performing readings.  To help force the issue I have written a macro to lower and raise the gantry in the Z direction n times which should keep the gantry moving while I inspect the machine.

```
[gcode_macro ZCHAOS]
gcode:
  {% set count = params.TEST_COUNT|int %}
  G28
  M118 Abusing z motion {count} times....
  {% for i in range(count) %}
  G1 X50 Y25 Z5 F6000
  G1 X50 Y25 Z250 F6000
  {% endfor %}
  M118 Finished...
```

### Hypothesis:
There is a bearing that is not lubricated or there is a plastic part thats crack that is providing for inconsistent movement in the z-axis.



### Results:
After running `ZCHAOS` for a few iterations I confirmed that the noise was coming from corner 2 of the printer.  My intuition was that the hole that holds the screw on the Z idler was cracked. 
- Upon inspection all plastic parts were intact.
- I removed the Z belt from that corner with the z joint and gantry assembly intact and there was still a noise.
- I removed the Z motor from that corner and there was still a noise.
- I unscrewed the m5 screw holding the z joint and linear bearing to the gantry and the noise dissappeared.

Based on the info above I had a gut feeling that there was something wrong with the linear bearing. I removed the bearing, and lubricated it.  I re-ran `ZCHAOS` once and there was still a noise! I re-ran the `ZCHAOS` for 5 more iteration and the noise subsided. Once the noise went away I re-ran `ZCHAOS` for a total of 10 more times and got way more accurate results with a few probe failures but no total `QGL` failures. 

```
22:30:43
echo: Finished...
22:30:28
probe: TRIGGERED
22:30:20
Docking Probe
22:30:20
Retries: 0/5 Probed points range: 0.002500 tolerance: 0.007500
22:30:20
Making the following Z adjustments:
stepper_z = -0.004062
stepper_z1 = 0.005913
stepper_z2 = -0.005913
stepper_z3 = 0.004062
22:30:20
Average: 2.548750
22:30:20
Actuator Positions:
z: 2.552812 z1: 2.542837 z2: 2.554663 z3: 2.544688
22:30:20
Gantry-relative probe points:
0: 2.550000 1: 2.547500 2: 2.550000 3: 2.547500
22:30:20
probe at 250.000,25.000 is z=7.452500
22:30:19
probe at 250.000,25.000 is z=7.452500
22:30:17
probe at 250.000,25.000 is z=7.450000
22:30:14
probe at 250.000,225.000 is z=7.451250
22:30:13
probe at 250.000,225.000 is z=7.450000
22:30:12
probe at 250.000,225.000 is z=7.448750
22:30:08
probe at 50.000,225.000 is z=7.452500
22:30:07
probe at 50.000,225.000 is z=7.452500
22:30:06
probe at 50.000,225.000 is z=7.451250
22:30:02
probe at 50.000,25.000 is z=7.450000
22:30:01
probe at 50.000,25.000 is z=7.447500
22:30:00
probe at 50.000,25.000 is z=7.450000
22:29:55
probe: open
22:29:50
moving to a safe Z distance
22:29:50
Attaching Probe
22:29:50
probe: TRIGGERED
22:29:49
QG Level
22:29:41
echo: Run 2 --------------------------
22:29:26
probe: TRIGGERED
22:29:18
Docking Probe
22:29:18
Retries: 0/5 Probed points range: 0.002500 tolerance: 0.007500
22:29:18
Making the following Z adjustments:
stepper_z = 0.004297
stepper_z1 = -0.004372
stepper_z2 = 0.003185
stepper_z3 = -0.003109
22:29:18
Average: 2.550423
22:29:18
Actuator Positions:
z: 2.546126 z1: 2.554795 z2: 2.547238 z3: 2.553532
22:29:18
Gantry-relative probe points:
0: 2.548750 1: 2.551250 2: 2.550000 3: 2.551250
22:29:18
probe at 250.000,25.000 is z=7.448750
22:29:17
probe at 250.000,25.000 is z=7.448750
22:29:16
probe at 250.000,25.000 is z=7.446250
22:29:12
probe at 250.000,225.000 is z=7.450000
22:29:11
probe at 250.000,225.000 is z=7.450000
22:29:10
probe at 250.000,225.000 is z=7.450000
22:29:09
Probe samples exceed tolerance. Retrying... #Strike 8
22:29:09
probe at 250.000,225.000 is z=7.450000
22:29:08
probe at 250.000,225.000 is z=7.477500
22:29:06
Probe samples exceed tolerance. Retrying... #Strike 7
22:29:06
probe at 250.000,225.000 is z=7.448750
22:29:05
probe at 250.000,225.000 is z=7.465000
22:29:02
probe at 50.000,225.000 is z=7.448750
22:29:01
probe at 50.000,225.000 is z=7.448750
22:28:59
probe at 50.000,225.000 is z=7.448750
22:28:56
probe at 50.000,25.000 is z=7.453750
22:28:55
probe at 50.000,25.000 is z=7.446250
22:28:54
probe at 50.000,25.000 is z=7.451250
22:28:49
probe: open
22:28:44
moving to a safe Z distance
22:28:44
Attaching Probe
22:28:44
probe: TRIGGERED
22:28:43
QG Level
22:28:35
echo: Run 1 --------------------------
22:28:20
probe: TRIGGERED
22:28:12
Docking Probe
22:28:12
Retries: 0/5 Probed points range: 0.003750 tolerance: 0.007500
22:28:12
Making the following Z adjustments:
stepper_z = -0.006797
stepper_z1 = 0.004247
stepper_z2 = -0.000685
stepper_z3 = 0.003234
22:28:12
Average: 2.551857
22:28:12
Actuator Positions:
z: 2.558654 z1: 2.547610 z2: 2.552542 z3: 2.548623
22:28:11
Gantry-relative probe points:
0: 2.555000 1: 2.551250 2: 2.551250 3: 2.551250
22:28:11
probe at 250.000,25.000 is z=7.448750
22:28:10
probe at 250.000,25.000 is z=7.448750
22:28:09
probe at 250.000,25.000 is z=7.446250
22:28:06
probe at 250.000,225.000 is z=7.450000
22:28:05
probe at 250.000,225.000 is z=7.448750
22:28:03
probe at 250.000,225.000 is z=7.447500
22:28:00
probe at 50.000,225.000 is z=7.448750
22:27:59
probe at 50.000,225.000 is z=7.448750
22:27:58
probe at 50.000,225.000 is z=7.447500
22:27:54
probe at 50.000,25.000 is z=7.445000
22:27:53
probe at 50.000,25.000 is z=7.445000
22:27:52
probe at 50.000,25.000 is z=7.448750
22:27:47
probe: open
22:27:42
moving to a safe Z distance
22:27:42
Attaching Probe
22:27:42
probe: TRIGGERED
22:27:41
QG Level
22:27:34
echo: Run 0 --------------------------
22:27:34
echo: Performing QGL obsevation procedure 3 times....
22:27:34
QRUN TEST_COUNT=3
22:27:22
echo: Finished...
22:27:07
probe: TRIGGERED
22:26:59
Docking Probe
22:26:59
Retries: 1/5 Probed points range: 0.006250 tolerance: 0.007500
22:26:59
Making the following Z adjustments:
stepper_z = 0.011328
stepper_z1 = -0.007079
stepper_z2 = 0.001141
stepper_z3 = -0.005390
22:26:59
Average: 2.553614
22:26:59
Actuator Positions:
z: 2.542286 z1: 2.560692 z2: 2.552473 z3: 2.559004
22:26:59
Gantry-relative probe points:
0: 2.548375 1: 2.554625 2: 2.554625 3: 2.554625
22:26:59
probe at 250.000,25.000 is z=7.445375
22:26:58
probe at 250.000,25.000 is z=7.445375
22:26:56
probe at 250.000,25.000 is z=7.442875
22:26:53
probe at 250.000,225.000 is z=7.445375
22:26:52
probe at 250.000,225.000 is z=7.445375
22:26:51
probe at 250.000,225.000 is z=7.442875
22:26:47
probe at 50.000,225.000 is z=7.445375
22:26:46
probe at 50.000,225.000 is z=7.445375
22:26:45
probe at 50.000,225.000 is z=7.442875
22:26:42
probe at 50.000,25.000 is z=7.452875
22:26:40
probe at 50.000,25.000 is z=7.451625
22:26:39
probe at 50.000,25.000 is z=7.445375
22:26:36
Retries: 0/5 Probed points range: 0.011250 tolerance: 0.007500
22:26:36
Making the following Z adjustments:
stepper_z = 0.005875
stepper_z1 = 0.005400
stepper_z2 = 0.004100
stepper_z3 = -0.015375
22:26:36
Average: 2.554744
22:26:36
Actuator Positions:
z: 2.548869 z1: 2.549344 z2: 2.550643 z3: 2.570118
22:26:36
Gantry-relative probe points:
0: 2.553750 1: 2.551250 2: 2.555000 3: 2.562500
22:26:36
probe at 250.000,25.000 is z=7.437500
22:26:35
probe at 250.000,25.000 is z=7.437500
22:26:33
probe at 250.000,25.000 is z=7.435000
22:26:30
probe at 250.000,225.000 is z=7.446250
22:26:29
probe at 250.000,225.000 is z=7.445000
22:26:28
probe at 250.000,225.000 is z=7.443750
22:26:24
probe at 50.000,225.000 is z=7.448750
22:26:23
probe at 50.000,225.000 is z=7.448750
22:26:22
probe at 50.000,225.000 is z=7.448750
22:26:18
probe at 50.000,25.000 is z=7.448750
22:26:17
probe at 50.000,25.000 is z=7.446250
22:26:16
probe at 50.000,25.000 is z=7.445000
22:26:11
probe: open
22:26:06
moving to a safe Z distance
22:26:06
Attaching Probe
22:26:06
probe: TRIGGERED
22:26:05
QG Level
22:25:57
echo: Run 6 --------------------------
22:25:42
probe: TRIGGERED
22:25:34
Docking Probe
22:25:34
Retries: 0/5 Probed points range: 0.007500 tolerance: 0.007500
22:25:34
Making the following Z adjustments:
stepper_z = -0.004672
stepper_z1 = -0.003128
stepper_z2 = -0.002810
stepper_z3 = 0.010609
22:25:34
Average: 2.553988
22:25:34
Actuator Positions:
z: 2.558660 z1: 2.557116 z2: 2.556798 z3: 2.543379
22:25:34
Gantry-relative probe points:
0: 2.555000 1: 2.556250 2: 2.553750 3: 2.548750
22:25:34
probe at 250.000,25.000 is z=7.453750
22:25:33
probe at 250.000,25.000 is z=7.443750
22:25:32
probe at 250.000,25.000 is z=7.451250
22:25:28
probe at 250.000,225.000 is z=7.446250
22:25:27
probe at 250.000,225.000 is z=7.446250
22:25:26
probe at 250.000,225.000 is z=7.445000
22:25:23
probe at 50.000,225.000 is z=7.443750
22:25:21
probe at 50.000,225.000 is z=7.443750
22:25:20
probe at 50.000,225.000 is z=7.442500
22:25:17
probe at 50.000,25.000 is z=7.445000
22:25:16
probe at 50.000,25.000 is z=7.445000
22:25:14
probe at 50.000,25.000 is z=7.442500
22:25:10
probe: open
22:25:05
moving to a safe Z distance
22:25:04
Attaching Probe
22:25:04
probe: TRIGGERED
22:25:03
QG Level
22:24:55
echo: Run 5 --------------------------
22:24:40
probe: TRIGGERED
22:24:32
Docking Probe
22:24:32
Retries: 1/5 Probed points range: 0.002500 tolerance: 0.007500
22:24:32
Making the following Z adjustments:
stepper_z = -0.000844
stepper_z1 = 0.001769
stepper_z2 = -0.004144
stepper_z3 = 0.003219
22:24:32
Average: 2.551268
22:24:32
Actuator Positions:
z: 2.552111 z1: 2.549499 z2: 2.555411 z3: 2.548049
22:24:32
Gantry-relative probe points:
0: 2.551047 1: 2.551047 2: 2.552297 3: 2.549797
22:24:32
probe at 250.000,25.000 is z=7.451453
22:24:31
probe at 250.000,25.000 is z=7.450203
22:24:30
probe at 250.000,25.000 is z=7.445203
22:24:26
probe at 250.000,225.000 is z=7.448953
22:24:25
probe at 250.000,225.000 is z=7.447703
22:24:24
probe at 250.000,225.000 is z=7.445203
22:24:21
probe at 50.000,225.000 is z=7.448953
22:24:19
probe at 50.000,225.000 is z=7.450203
22:24:18
probe at 50.000,225.000 is z=7.446453
22:24:15
probe at 50.000,25.000 is z=7.450203
22:24:14
probe at 50.000,25.000 is z=7.448953
22:24:12
probe at 50.000,25.000 is z=7.446453
22:24:09
Retries: 0/5 Probed points range: 0.008750 tolerance: 0.007500
22:24:09
Making the following Z adjustments:
stepper_z = -0.013953
stepper_z1 = 0.004454
stepper_z2 = 0.001484
stepper_z3 = 0.008015
22:24:09
Average: 2.552262
22:24:09
Actuator Positions:
z: 2.566214 z1: 2.547808 z2: 2.550778 z3: 2.544246
22:24:09
Gantry-relative probe points:
0: 2.558750 1: 2.552500 2: 2.550000 3: 2.550000
22:24:09
probe at 250.000,25.000 is z=7.451250
22:24:08
probe at 250.000,25.000 is z=7.450000
22:24:07
probe at 250.000,25.000 is z=7.446250
22:24:03
probe at 250.000,225.000 is z=7.450000
22:24:02
probe at 250.000,225.000 is z=7.450000
22:24:01
probe at 250.000,225.000 is z=7.447500
22:23:57
probe at 50.000,225.000 is z=7.447500
22:23:56
probe at 50.000,225.000 is z=7.447500
22:23:55
probe at 50.000,225.000 is z=7.446250
22:23:51
probe at 50.000,25.000 is z=7.443750
22:23:50
probe at 50.000,25.000 is z=7.441250
22:23:49
probe at 50.000,25.000 is z=7.440000
22:23:44
probe: open
22:23:39
moving to a safe Z distance
22:23:39
Attaching Probe
22:23:39
probe: TRIGGERED
22:23:38
QG Level
22:23:30
echo: Run 4 --------------------------
22:23:15
probe: TRIGGERED
22:23:07
Docking Probe
22:23:07
Retries: 0/5 Probed points range: 0.002500 tolerance: 0.007500
22:23:07
Making the following Z adjustments:
stepper_z = 0.000359
stepper_z1 = 0.004041
stepper_z2 = -0.002853
stepper_z3 = -0.001547
22:23:07
Average: 2.548952
22:23:07
Actuator Positions:
z: 2.548593 z1: 2.544912 z2: 2.551806 z3: 2.550499
22:23:07
Gantry-relative probe points:
0: 2.548750 1: 2.547500 2: 2.550000 3: 2.550000
22:23:07
probe at 250.000,25.000 is z=7.450000
22:23:05
probe at 250.000,25.000 is z=7.450000
22:23:04
probe at 250.000,25.000 is z=7.448750
22:23:01
probe at 250.000,225.000 is z=7.451250
22:23:00
probe at 250.000,225.000 is z=7.450000
22:22:59
probe at 250.000,225.000 is z=7.450000
22:22:55
probe at 50.000,225.000 is z=7.453750
22:22:54
probe at 50.000,225.000 is z=7.452500
22:22:53
probe at 50.000,225.000 is z=7.452500
22:22:52
Probe samples exceed tolerance. Retrying... #strike 6
22:22:52
probe at 50.000,225.000 is z=7.470000
22:22:50
probe at 50.000,225.000 is z=7.452500
22:22:47
probe at 50.000,25.000 is z=7.451250
22:22:46
probe at 50.000,25.000 is z=7.451250
22:22:45
probe at 50.000,25.000 is z=7.450000
22:22:40
probe: open
22:22:35
moving to a safe Z distance
22:22:35
Attaching Probe
22:22:35
probe: TRIGGERED
22:22:34
QG Level
22:22:26
echo: Run 3 --------------------------
22:22:11
probe: TRIGGERED
22:22:03
Docking Probe
22:22:03
Retries: 2/5 Probed points range: 0.002500 tolerance: 0.007500
22:22:03
Making the following Z adjustments:
stepper_z = 0.000844
stepper_z1 = -0.001769
stepper_z2 = 0.004144
stepper_z3 = -0.003219
22:22:03
Average: 2.526985
22:22:03
Actuator Positions:
z: 2.526141 z1: 2.528753 z2: 2.522841 z3: 2.530203
22:22:03
Gantry-relative probe points:
0: 2.527205 1: 2.527205 2: 2.525955 3: 2.528455
22:22:02
probe at 250.000,25.000 is z=7.471545
22:22:01
probe at 250.000,25.000 is z=7.472795
22:22:00
probe at 250.000,25.000 is z=7.470295
22:21:57
probe at 250.000,225.000 is z=7.474045
22:21:56
probe at 250.000,225.000 is z=7.474045
22:21:54
probe at 250.000,225.000 is z=7.472795
22:21:53
Probe samples exceed tolerance. Retrying... #Strike 5
22:21:53
probe at 250.000,225.000 is z=7.490295
22:21:52
probe at 250.000,225.000 is z=7.472795
22:21:51
probe at 250.000,225.000 is z=7.469045
22:21:47
probe at 50.000,225.000 is z=7.472795
22:21:46
probe at 50.000,225.000 is z=7.472795
22:21:45
probe at 50.000,225.000 is z=7.470295
22:21:42
probe at 50.000,25.000 is z=7.472795
22:21:41
probe at 50.000,25.000 is z=7.474045
22:21:39
probe at 50.000,25.000 is z=7.472795
22:21:36
Retries: 1/5 Probed points range: 0.016250 tolerance: 0.007500
22:21:36
Making the following Z adjustments:
stepper_z = -0.009592
stepper_z1 = 0.025795
stepper_z2 = -0.035295
stepper_z3 = 0.019092
22:21:36
Average: 2.526800
22:21:36
Actuator Positions:
z: 2.536392 z1: 2.501005 z2: 2.562095 z3: 2.507708
22:21:36
Gantry-relative probe points:
0: 2.527169 1: 2.520919 2: 2.535919 3: 2.519669
22:21:36
probe at 250.000,25.000 is z=7.480331
22:21:35
probe at 250.000,25.000 is z=7.480331
22:21:33
probe at 250.000,25.000 is z=7.477831
22:21:30
probe at 250.000,225.000 is z=7.464081
22:21:29
probe at 250.000,225.000 is z=7.464081
22:21:28
probe at 250.000,225.000 is z=7.461581
22:21:24
probe at 50.000,225.000 is z=7.479081
22:21:23
probe at 50.000,225.000 is z=7.479081
22:21:22
probe at 50.000,225.000 is z=7.477831
22:21:19
probe at 50.000,25.000 is z=7.472831
22:21:17
probe at 50.000,25.000 is z=7.471581
22:21:16
probe at 50.000,25.000 is z=7.476581
22:21:13
Retries: 0/5 Probed points range: 0.012500 tolerance: 0.007500
22:21:13
Making the following Z adjustments:
stepper_z = 0.010344
stepper_z1 = 0.001081
stepper_z2 = -0.015331
stepper_z3 = 0.003906
22:21:13
Average: 2.523197
22:21:13
Actuator Positions:
z: 2.512853 z1: 2.522115 z2: 2.538528 z3: 2.519291
22:21:13
Gantry-relative probe points:
0: 2.516250 1: 2.522500 2: 2.528750 3: 2.520000
22:21:13
probe at 250.000,25.000 is z=7.480000
22:21:12
probe at 250.000,25.000 is z=7.480000
22:21:10
probe at 250.000,25.000 is z=7.477500
22:21:07
probe at 250.000,225.000 is z=7.471250
22:21:06
probe at 250.000,225.000 is z=7.471250
22:21:05
probe at 250.000,225.000 is z=7.468750
22:21:01
probe at 50.000,225.000 is z=7.477500
22:21:00
probe at 50.000,225.000 is z=7.477500
22:20:59
probe at 50.000,225.000 is z=7.477500
22:20:55
probe at 50.000,25.000 is z=7.477500
22:20:54
probe at 50.000,25.000 is z=7.483750
22:20:53
probe at 50.000,25.000 is z=7.485000
22:20:52
Probe samples exceed tolerance. Retrying... #Strike 4
22:20:52
probe at 50.000,25.000 is z=7.483750
22:20:50
probe at 50.000,25.000 is z=7.478750
22:20:49
probe at 50.000,25.000 is z=7.472500
22:20:44
probe: open
22:20:39
moving to a safe Z distance
22:20:39
Attaching Probe
22:20:39
probe: TRIGGERED
22:20:38
QG Level
22:20:30
echo: Run 2 --------------------------
22:20:15
probe: TRIGGERED
22:20:07
Docking Probe
22:20:07
Retries: 1/5 Probed points range: 0.007500 tolerance: 0.007500
22:20:07
Making the following Z adjustments:
stepper_z = 0.005499
stepper_z1 = -0.014451
stepper_z2 = 0.014451
stepper_z3 = -0.005499
22:20:07
Average: 2.543843
22:20:07
Actuator Positions:
z: 2.538344 z1: 2.558294 z2: 2.529393 z3: 2.549343
22:20:07
Gantry-relative probe points:
0: 2.542593 1: 2.547593 2: 2.540093 3: 2.545093
22:20:07
probe at 250.000,25.000 is z=7.454907
22:20:06
probe at 250.000,25.000 is z=7.456157
22:20:05
probe at 250.000,25.000 is z=7.454907
22:20:01
probe at 250.000,225.000 is z=7.459907
22:20:00
probe at 250.000,225.000 is z=7.459907
22:19:59
probe at 250.000,225.000 is z=7.449907
22:19:55
probe at 50.000,225.000 is z=7.452407
22:19:54
probe at 50.000,225.000 is z=7.452407
22:19:53
probe at 50.000,225.000 is z=7.449907
22:19:50
probe at 50.000,25.000 is z=7.457407
22:19:49
probe at 50.000,25.000 is z=7.457407
22:19:47
probe at 50.000,25.000 is z=7.458657
22:19:46
Probe samples exceed tolerance. Retrying... #Strike 3
22:19:46
probe at 50.000,25.000 is z=7.471157
22:19:45
probe at 50.000,25.000 is z=7.458657
22:19:42
Retries: 0/5 Probed points range: 0.027500 tolerance: 0.007500
22:19:42
Making the following Z adjustments:
stepper_z = -0.021157
stepper_z1 = -0.005244
stepper_z2 = 0.036119
stepper_z3 = -0.009718
22:19:42
Average: 2.545261
22:19:42
Actuator Positions:
z: 2.566418 z1: 2.550505 z2: 2.509142 z3: 2.554979
22:19:42
Gantry-relative probe points:
0: 2.560000 1: 2.547500 2: 2.532500 3: 2.552500
22:19:41
probe at 250.000,25.000 is z=7.447500
22:19:40
probe at 250.000,25.000 is z=7.448750
22:19:39
probe at 250.000,25.000 is z=7.447500
22:19:36
probe at 250.000,225.000 is z=7.467500
22:19:35
probe at 250.000,225.000 is z=7.467500
22:19:33
probe at 250.000,225.000 is z=7.467500
22:19:32
Probe samples exceed tolerance. Retrying... #Strike 2
22:19:32
probe at 250.000,225.000 is z=7.468750
22:19:31
probe at 250.000,225.000 is z=7.451250
22:19:30
probe at 250.000,225.000 is z=7.450000
22:19:26
probe at 50.000,225.000 is z=7.453750
22:19:25
probe at 50.000,225.000 is z=7.452500
22:19:24
probe at 50.000,225.000 is z=7.451250
22:19:21
probe at 50.000,25.000 is z=7.441250
22:19:20
probe at 50.000,25.000 is z=7.440000
22:19:18
probe at 50.000,25.000 is z=7.437500
22:19:14
probe: open
22:19:09
moving to a safe Z distance
22:19:09
Attaching Probe
22:19:08
probe: TRIGGERED
22:19:07
QG Level
22:18:59
echo: Run 1 --------------------------
22:18:44
probe: TRIGGERED
22:18:36
Docking Probe
22:18:36
Retries: 3/5 Probed points range: 0.003750 tolerance: 0.007500
22:18:36
Making the following Z adjustments:
stepper_z = 0.006797
stepper_z1 = -0.004247
stepper_z2 = 0.000685
stepper_z3 = -0.003234
22:18:36
Average: 3.696133
22:18:36
Actuator Positions:
z: 3.689336 z1: 3.700380 z2: 3.695448 z3: 3.699367
22:18:36
Gantry-relative probe points:
0: 3.692990 1: 3.696740 2: 3.696740 3: 3.696740
22:18:36
probe at 250.000,25.000 is z=6.303260
22:18:35
probe at 250.000,25.000 is z=6.303260
22:18:34
probe at 250.000,25.000 is z=6.300760
22:18:30
probe at 250.000,225.000 is z=6.303260
22:18:28
probe at 250.000,225.000 is z=6.303260
22:18:27
probe at 250.000,225.000 is z=6.300760
22:18:23
probe at 50.000,225.000 is z=6.303260
22:18:22
probe at 50.000,225.000 is z=6.303260
22:18:21
probe at 50.000,225.000 is z=6.302010
22:18:17
probe at 50.000,25.000 is z=6.307010
22:18:16
probe at 50.000,25.000 is z=6.307010
22:18:15
probe at 50.000,25.000 is z=6.312010
22:18:11
Retries: 2/5 Probed points range: 0.016250 tolerance: 0.007500
22:18:11
Making the following Z adjustments:
stepper_z = 0.020734
stepper_z1 = -0.024510
stepper_z2 = 0.006698
stepper_z3 = -0.002921
22:18:11
Average: 3.698706
22:18:11
Actuator Positions:
z: 3.677973 z1: 3.723216 z2: 3.692008 z3: 3.701627
22:18:11
Gantry-relative probe points:
0: 3.688616 1: 3.704866 2: 3.698616 3: 3.696116
22:18:11
probe at 250.000,25.000 is z=6.305134
22:18:10
probe at 250.000,25.000 is z=6.303884
22:18:08
probe at 250.000,25.000 is z=6.301384
22:18:05
probe at 250.000,225.000 is z=6.302634
22:18:03
probe at 250.000,225.000 is z=6.301384
22:18:02
probe at 250.000,225.000 is z=6.300134
22:17:58
probe at 50.000,225.000 is z=6.295134
22:17:57
probe at 50.000,225.000 is z=6.295134
22:17:56
probe at 50.000,225.000 is z=6.293884
22:17:52
probe at 50.000,25.000 is z=6.311384
22:17:51
probe at 50.000,25.000 is z=6.311384
22:17:50
probe at 50.000,25.000 is z=6.311384
22:17:46
Retries: 1/5 Probed points range: 0.042500 tolerance: 0.007500
22:17:46
Making the following Z adjustments:
stepper_z = 0.052577
stepper_z1 = 0.007571
stepper_z2 = -0.030134
stepper_z3 = -0.030015
22:17:46
Average: 3.696004
22:17:46
Actuator Positions:
z: 3.643426 z1: 3.688433 z2: 3.726138 z3: 3.726019
22:17:46
Gantry-relative probe points:
0: 3.669848 1: 3.687348 2: 3.712348 3: 3.706098
22:17:46
probe at 250.000,25.000 is z=6.295152
22:17:45
probe at 250.000,25.000 is z=6.293902
22:17:44
probe at 250.000,25.000 is z=6.291402
22:17:40
probe at 250.000,225.000 is z=6.287652
22:17:39
probe at 250.000,225.000 is z=6.287652
22:17:37
probe at 250.000,225.000 is z=6.285152
22:17:33
probe at 50.000,225.000 is z=6.303902
22:17:32
probe at 50.000,225.000 is z=6.312652
22:17:31
probe at 50.000,225.000 is z=6.313902
22:17:27
probe at 50.000,25.000 is z=6.330152
22:17:26
probe at 50.000,25.000 is z=6.331402
22:17:25
probe at 50.000,25.000 is z=6.330152
22:17:24
Probe samples exceed tolerance. Retrying... #strike 1
22:17:24
probe at 50.000,25.000 is z=6.327652
22:17:23
probe at 50.000,25.000 is z=6.340152
22:17:18
Retries: 0/5 Probed points range: 1.796250 tolerance: 0.007500
22:17:18
Making the following Z adjustments:
stepper_z = -1.396402
stepper_z1 = 0.498848
stepper_z2 = 2.244277
stepper_z3 = -1.346723
22:17:18
Average: 3.676164
22:17:17
Actuator Positions:
z: 5.072566 z1: 3.177316 z2: 1.431888 z3: 5.022888
22:17:17
Gantry-relative probe points:
0: 4.722500 1: 3.491250 2: 2.926250 3: 4.582500
22:17:17
probe at 250.000,25.000 is z=5.417500
22:17:16
probe at 250.000,25.000 is z=5.418750
22:17:15
probe at 250.000,25.000 is z=5.417500
22:17:11
probe at 250.000,225.000 is z=7.075000
22:17:10
probe at 250.000,225.000 is z=7.073750
22:17:09
probe at 250.000,225.000 is z=7.073750
22:17:05
probe at 50.000,225.000 is z=6.510000
22:17:04
probe at 50.000,225.000 is z=6.508750
22:17:03
probe at 50.000,225.000 is z=6.506250
22:16:59
probe at 50.000,25.000 is z=5.277500
22:16:58
probe at 50.000,25.000 is z=5.278750
22:16:56
probe at 50.000,25.000 is z=5.275000
22:16:51
probe: open
22:16:46
moving to a safe Z distance
22:16:46
Attaching Probe
22:16:46
probe: TRIGGERED
22:16:45
QG Level
22:16:21
echo: Run 0 --------------------------
22:16:21
echo: Performing QGL obsevation procedure 7 times....
22:16:21
QRUN TEST_COUNT=7
```

### Conclusion
Greasing the linear bearing in the corner that had the most probe failures removed `QGL` failures during the test. There were 8 probe failures in 10 `QGL` runs as compared to 7 probe failures in 7 `QGL` runs  and zero total `QGL` failures. I suspect that lubricating the other 3 linear rails will impact readings in a positive way.
