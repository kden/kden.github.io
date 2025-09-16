# Sensor Hardware V3

I'm working on the next hardware layout, which will let us eliminate the USB battery packs and monitor the battery charge.


## Electrical connections
```mermaid
flowchart TD
    bat[Battery]
    bms[Battery Management System]
    bbvin[Buck Boost VIN]
    bb[Buck Boost]
    pwr[Power Rail]
    esp32[Seeed XIAO ESP32C3]
    ls[Light Sensor]
    vsplit[Voltage Split]
    r1[R1 10kΩ]
    r2[R2 10kΩ]
    gnd1[⏚]
    gnd2[⏚]
    gnd3[⏚]
    gnd4[⏚]

    %% Battery connections
    bat -->|B+| bms
    bat -->|B-| bms
    
    %% BMS to Buck Boost
    bms -->|P+ to VIN| bbvin
    bbvin -->|VIN| bb
    bms -->|P- to GND| bb
    
    %% Buck Boost output
    bb -->|VOUT to +| pwr
    
    %% Power distribution
    pwr -->|3V3| esp32
    pwr -->|VCC| ls
    
    %% ESP32 connections
    vsplit -->|GPIO2| esp32
    ls -->|DAT to GPIO6| esp32
    ls -->|SCL to GPIO7| esp32
    
    %% Voltage divider
    r1 --> vsplit
    vsplit --> r2
    bbvin --> r1
    
    %% Ground connections
    bb -->|GND rail| gnd1
    esp32 -->|GND rail| gnd2
    ls -->|GND rail| gnd3
    r2 -->|GND rail| gnd4

    %% Styling for different connection types
    linkStyle 0 stroke:#ff0000,stroke-width:3px
    linkStyle 1 stroke:#000000,stroke-width:3px
    linkStyle 2 stroke:#ff0000,stroke-width:3px
    linkStyle 3 stroke:#ff0000,stroke-width:3px
    linkStyle 4 stroke:#000000,stroke-width:3px
    linkStyle 5 stroke:#ff0000,stroke-width:3px
    linkStyle 6 stroke:#ff0000,stroke-width:3px
    linkStyle 7 stroke:#ff0000,stroke-width:3px
    linkStyle 8 stroke:#ffa500,stroke-width:2px
    linkStyle 9 stroke:#cccccc,stroke-width:2px
    linkStyle 10 stroke:#8b4513,stroke-width:2px
    linkStyle 11 stroke:#ffa500,stroke-width:2px
    linkStyle 12 stroke:#ffa500,stroke-width:2px
    linkStyle 13 stroke:#ffa500,stroke-width:2px
    linkStyle 14 stroke:#000000,stroke-width:3px
    linkStyle 15 stroke:#000000,stroke-width:3px
    linkStyle 16 stroke:#000000,stroke-width:3px
    linkStyle 17 stroke:#000000,stroke-width:3px
```

## Electrical connections with estimated voltages
```mermaid
flowchart TD
    bat[Battery<br/>3.7V nominal]
    bms[Battery Management System]
    bbvin[Buck Boost VIN]
    bb[Buck Boost]
    pwr[Power Rail<br/>3.3V]
    esp32[Seeed XIAO ESP32C3]
    ls[Light Sensor]
    vsplit[Voltage Split<br/>~1.85V]
    r1[R1 10kΩ]
    r2[R2 10kΩ]
    gnd1[⏚<br/>0V]
    gnd2[⏚<br/>0V]
    gnd3[⏚<br/>0V]
    gnd4[⏚<br/>0V]

    %% Battery connections
    bat -->|B+ 3.7V| bms
    bat -->|B- 0V| bms
    
    %% BMS to Buck Boost
    bms -->|P+ to VIN 3.7V| bbvin
    bbvin -->|VIN 3.7V| bb
    bms -->|P- to GND 0V| bb
    
    %% Buck Boost output
    bb -->|VOUT 3.3V| pwr
    
    %% Power distribution
    pwr -->|3V3 3.3V| esp32
    pwr -->|VCC 3.3V| ls
    
    %% ESP32 connections
    vsplit -->|GPIO2 1.85V| esp32
    ls -->|DAT 0-3.3V| esp32
    ls -->|SCL 0-3.3V| esp32
    
    %% Voltage divider
    r1 -->|1.85V| vsplit
    vsplit -->|1.85V| r2
    bbvin -->|3.7V| r1
    
    %% Ground connections
    bb -->|GND 0V| gnd1
    esp32 -->|GND 0V| gnd2
    ls -->|GND 0V| gnd3
    r2 -->|GND 0V| gnd4

    %% Styling for different connection types
    linkStyle 0 stroke:#ff0000,stroke-width:3px
    linkStyle 1 stroke:#000000,stroke-width:3px
    linkStyle 2 stroke:#ff0000,stroke-width:3px
    linkStyle 3 stroke:#ff0000,stroke-width:3px
    linkStyle 4 stroke:#000000,stroke-width:3px
    linkStyle 5 stroke:#ff0000,stroke-width:3px
    linkStyle 6 stroke:#ff0000,stroke-width:3px
    linkStyle 7 stroke:#ff0000,stroke-width:3px
    linkStyle 8 stroke:#ffa500,stroke-width:2px
    linkStyle 9 stroke:#cccccc,stroke-width:2px
    linkStyle 10 stroke:#8b4513,stroke-width:2px
    linkStyle 11 stroke:#ffa500,stroke-width:2px
    linkStyle 12 stroke:#ffa500,stroke-width:2px
    linkStyle 13 stroke:#ffa500,stroke-width:2px
    linkStyle 14 stroke:#000000,stroke-width:3px
    linkStyle 15 stroke:#000000,stroke-width:3px
    linkStyle 16 stroke:#000000,stroke-width:3px
    linkStyle 17 stroke:#000000,stroke-width:3px
```

## Electrical connections with estimated current

```mermaid
flowchart TD
    bat[Samsung 35E Battery<br/>3.7V, 3500mAh]
    bms[Battery Management System]
    bbvin[Buck Boost VIN]
    bb[XL63802 Buck Boost<br/>~90% efficiency]
    pwr[Power Rail<br/>3.3V]
    esp32[Seeed XIAO ESP32C3<br/>~120mA active]
    ls[Light Sensor<br/>~5mA active]
    vsplit[Voltage Split<br/>~1.85V]
    r1[R1 10kΩ]
    r2[R2 10kΩ]
    gnd1[⏚<br/>0A]
    gnd2[⏚<br/>0A]
    gnd3[⏚<br/>0A]
    gnd4[⏚<br/>0A]

    %% Battery connections
    bat -->|B+ ~150mA| bms
    bat -->|B- ~150mA return| bms
    
    %% BMS to Buck Boost
    bms -->|P+ ~150mA| bbvin
    bbvin -->|VIN ~150mA| bb
    bms -->|P- ~150mA return| bb
    
    %% Buck Boost output
    bb -->|VOUT ~135mA| pwr
    
    %% Power distribution
    pwr -->|3V3 ~120mA| esp32
    pwr -->|VCC ~5mA| ls
    
    %% ESP32 connections
    vsplit -->|GPIO2 ~1µA| esp32
    ls -->|DAT ~1µA| esp32
    ls -->|SCL ~1µA| esp32
    
    %% Voltage divider (very low current)
    r1 -->|~185µA| vsplit
    vsplit -->|~185µA| r2
    bbvin -->|~185µA| r1
    
    %% Ground connections (return currents)
    bb -->|GND ~135mA return| gnd1
    esp32 -->|GND ~120mA return| gnd2
    ls -->|GND ~5mA return| gnd3
    r2 -->|GND ~185µA return| gnd4

    %% Styling for different connection types
    linkStyle 0 stroke:#ff0000,stroke-width:3px
    linkStyle 1 stroke:#000000,stroke-width:3px
    linkStyle 2 stroke:#ff0000,stroke-width:3px
    linkStyle 3 stroke:#ff0000,stroke-width:3px
    linkStyle 4 stroke:#000000,stroke-width:3px
    linkStyle 5 stroke:#ff0000,stroke-width:3px
    linkStyle 6 stroke:#ff0000,stroke-width:3px
    linkStyle 7 stroke:#ff0000,stroke-width:3px
    linkStyle 8 stroke:#ffa500,stroke-width:2px
    linkStyle 9 stroke:#cccccc,stroke-width:2px
    linkStyle 10 stroke:#8b4513,stroke-width:2px
    linkStyle 11 stroke:#ffa500,stroke-width:2px
    linkStyle 12 stroke:#ffa500,stroke-width:2px
    linkStyle 13 stroke:#ffa500,stroke-width:2px
    linkStyle 14 stroke:#000000,stroke-width:3px
    linkStyle 15 stroke:#000000,stroke-width:3px
    linkStyle 16 stroke:#000000,stroke-width:3px
    linkStyle 17 stroke:#000000,stroke-width:3px
```