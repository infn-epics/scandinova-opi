# PV Name Mapping: Display → Database

## Thyratron Section
- HeaterVoltage1 → Thy:HeaterVoltage
- HeaterVoltage2 → (NOT IN DATABASE - remove or map to Thy:ReservoirVoltage?)
- HeaterCurrent → Thy:TotalCurrent (or separate heater current?)
- TotalCurrent → Thy:TotalCurrent
- ReservoirVoltage → Thy:ReservoirVoltage
- TimerPreheatMin → Thy:TimerPreheatMin
- TimerPreheatSec → Thy:TimerPreheatSec

## Klystron Section
- KlystronVoltage → Klys:HeaterVoltage
- KlystronCurrent → Klys:HeaterCurrent

## Water/Temperature
- BodyWaterInTemp → Klys:BodyWaterInTemp
- BodyWaterOutTemp → Klys:BodyWaterOutTemp
- BodyWaterFlow → Klys:BodyWaterFlow

## Focus Magnets
- MagnetVoltageCoil1 → Focus:Coil1Voltage
- MagnetCurrentCoil1 → Focus:Coil1Current
- MagnetVoltageCoil2 → Focus:Coil2Voltage
- MagnetCurrentCoil2 → Focus:Coil2Current
- (Coil 3 missing in display)

## Timers
- TimerPreheat100Min → Klys:TimerPreheat100Min
- TimerPreheat100Sec → (NOT IN DATABASE - only minutes exist)

## Connection Status
- HeaterVoltage1 (for LED) → Thy:HeaterVoltage or Connected
