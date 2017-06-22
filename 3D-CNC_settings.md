## Settings for my CNC Machine and 3D Printer

# CNC Machine running GRBL

```
> $$
$0=10 (Step pulse time, microseconds)
$1=25 (Step idle delay, milliseconds)
$2=0 (Step pulse invert, mask)
$3=4 (Step direction invert, mask)
$4=0 (Invert step enable pin, boolean)
$5=0 (Invert limit pins, boolean)
$6=0 (Invert probe pin, boolean)
$10=1 (Status report options, mask)
$11=0.010 (Junction deviation, millimeters)
$12=0.002 (Arc tolerance, millimeters)
$13=0 (Report in inches, boolean)
$20=0 (Soft limits enable, boolean)
$21=0 (Hard limits enable, boolean)
$22=0 (Homing cycle enable, boolean)
$23=3 (Homing direction invert, mask)
$24=200.000 (Homing locate feed rate, mm/min)
$25=250.000 (Homing search seek rate, mm/min)
$26=250 (Homing switch debounce delay, milliseconds)
$27=3.000 (Homing switch pull-off distance, millimeters)
$30=1000 (Maximum spindle speed, RPM)
$31=0 (Minimum spindle speed, RPM)
$32=0 (Laser-mode enable, boolean)
$100=1280.000 (X-axis travel resolution, step/mm)
$101=1280.000 (Y-axis travel resolution, step/mm)
$102=640.000 (Z-axis travel resolution, step/mm)
$110=500.000 (X-axis maximum rate, mm/min)
$111=500.000 (Y-axis maximum rate, mm/min)
$112=400.000 (Z-axis maximum rate, mm/min)
$120=1.000 (X-axis acceleration, mm/sec^2)
$121=1.000 (Y-axis acceleration, mm/sec^2)
$122=1.000 (Z-axis acceleration, mm/sec^2)
$130=155.000 (X-axis maximum travel, millimeters)
$131=155.000 (Y-axis maximum travel, millimeters)
$132=30.000 (Z-axis maximum travel, millimeters)
```

# 3D Printer (PrintrBot Simple Metal)


### PrintrBot

**Adjust z height**
```
M212 Z-0.5
M500
M501
```

**Adjust Motor Current**

`X:50.00 Y:50.00 Z:60.00 E:60.00`

`M907`: Sets motor current as a percentage of the full-scale adjustment (ex: M907 X30 Y30 Z30 E30 sets all motor current levels to 30% of the full-scale reference value).

`M909`: Reads the present motor current setpoints (similar to M501).

`M910`: Saves motor current setpoints (similar to M500).


#### M501 Settings
Description | Code
----------- | ------
Steps per mm | `M92 X80 Y80 Z2020 E96.00`
Default Build Dimensions and Sensor Offset | `M211 X152 Y152 Z152`
Maximum Feedrates (mm/s) | `M203 X125.00 Y125.00 Z5.00 E14.00`
Maximum Acceleration (mm/s2) | `M201 X2000 Y2000 Z30 E10000`
Acceleration (s=Acceleration, T=retract acceleration) | `M204 S3000.00 T3000.00`
Advanced variables: S=Min feedrate (mm/s), T=Min travel feedrate (mm/s), B=minimum segment time (ms), X=maximum XY jerk (mm/s), Z=maximum Z jerk (mm/s), E=maximum E jerk (mm/s) | `M205 S0.00 T0.00 B20000 X20.00 Z0.40 E5.00`
Home offset (mm) | `M206 X0.00 Y0.00 Z0.00`
PID Settings | `M301 P22.20 I1.08 D114.00`
Min position (mm) | `M210 X0.00 Y0.00 Z0.00`
Max position | `M211 X152.40 Y152.40 Z152.40`
Bed probe offset (mm) | `M212 X10.00 Y0.00 Z0.00`
