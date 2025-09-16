# Sensor Hardware V3

I'm working on the next hardware layout, which will let us eliminate the USB battery packs and monitor the battery charge.

The following diagrams are generated from [Mermaid](https://mermaid.js.org/), a syntax which Claude often uses to define simple diagrams.  I gave it a list of the connections I'd made and let Claude reformat my connections as Mermaid syntax, then let the Mermaid interpreter render the chart.  You can use Mermaid to embed text-only diagram descriptions in your markdown pages in GitHub and they are automatically rendered in the web view of your repository.

[Mermaid does not yet support circuit diagrams](https://github.com/mermaid-js/mermaid/issues/2112), so I am using their flowchart type to represent a graph with edges representing current paths.  I decided to use this because I'm interested in learning more about the usefulness of Mermaid.  Is it a good accessibility tool for users with vision problems, since it has a clear, textual representation?  What kinds of diagrams is it good at?

This isn't a standard circuit diagram, but this format helps me focus on what connections to make.  Some of the diagram nodes represent pins rather than boards.

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
    outgnd[Buck Boost OUT GND]
    ingnd[Buck Boost IN GND]
    gnd2[⏚]
    gnd3[⏚]
    gnd4[⏚]

    %% Battery connections
    bat -->|B+| bms
    bms -->|B-| bat
    
    %% BMS to Buck Boost
    bms -->|P+ to VIN| bbvin
    bbvin -->|VIN| bb
    bb -->|GND to P-| bms
    
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
    bb -->|OUT GND| outgnd
    outgnd -->|Buck Boost internal routing| ingnd
    ingnd -->|IN GND| bms
    esp32 --> gnd2
    ls --> gnd3
    r2 --> gnd4
    gnd2 --> outgnd
    gnd3 --> outgnd
    gnd4 --> outgnd

    %% Semantic styling for different connection types
    linkStyle 0,2,3,5,6,7 stroke:#ff0000,stroke-width:3px
    linkStyle 1,4,14,15,16,17,18,19,20,21,22 stroke:#000000,stroke-width:3px
    linkStyle 8,11,12,13 stroke:#ffa500,stroke-width:2px
    linkStyle 9 stroke:#cccccc,stroke-width:2px
    linkStyle 10 stroke:#8b4513,stroke-width:2px
```

## Electrical connections with estimated voltages and currents

To better understand how current flows through the circuit, I asked Claude AI to give estimates of voltage and current. 


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
    outgnd[Buck Boost OUT GND]
    ingnd[Buck Boost IN GND]
    gnd2[⏚]
    gnd3[⏚]
    gnd4[⏚]

    %% Battery connections
    bat -->|3.7V / 300mA B+| bms
    bms -->|0V / 300mA B-| bat
    
    %% BMS to Buck Boost
    bms -->|3.7V / 270mA P+ to VIN| bbvin
    bbvin -->|3.7V / 270mA VIN| bb
    bb -->|0V / 270mA GND to P-| bms
    
    %% Buck Boost output
    bb -->|3.3V / 300mA VOUT to +| pwr
    
    %% Power distribution
    pwr -->|3.3V / 280mA| esp32
    pwr -->|3.3V / 20mA VCC| ls
    
    %% ESP32 connections
    vsplit -->|1.85V / 0.185µA GPIO2| esp32
    ls -->|3.3V / <1mA DAT to GPIO6| esp32
    ls -->|3.3V / <1mA SCL to GPIO7| esp32
    
    %% Voltage divider
    r1 -->|3.7V→1.85V / 0.37mA| vsplit
    vsplit -->|1.85V→0V / 0.185mA| r2
    bbvin -->|3.7V / 0.37mA| r1
    
    %% Ground connections
    bb -->|0V / 300mA OUT GND| outgnd
    outgnd -->|0V / 300mA Buck Boost internal routing| ingnd
    ingnd -->|0V / 300mA IN GND| bms
    esp32 -->|0V / 280mA| gnd2
    ls -->|0V / 20mA| gnd3
    r2 -->|0V / 0.185mA| gnd4
    gnd2 -->|0V / 280mA| outgnd
    gnd3 -->|0V / 20mA| outgnd
    gnd4 -->|0V / 0.185mA| outgnd

    %% Semantic styling for different connection types
    linkStyle 0,2,3,5,6,7 stroke:#ff0000,stroke-width:3px
    linkStyle 1,4,14,15,16,17,18,19,20,21,22 stroke:#000000,stroke-width:3px
    linkStyle 8,11,12,13 stroke:#ffa500,stroke-width:2px
    linkStyle 9 stroke:#cccccc,stroke-width:2px
    linkStyle 10 stroke:#8b4513,stroke-width:2px
```

Claude's assumptions:

* ESP32-C3 active current: ~280mA (WiFi enabled, active operation)
* BH1750 sensor current: ~20mA (typical for ambient light sensing)
* Voltage divider current: 0.37mA (3.7V across 20kΩ total resistance)
* Buck-boost efficiency: ~90% (so input current slightly lower than output)
* Battery discharge current: Total system load