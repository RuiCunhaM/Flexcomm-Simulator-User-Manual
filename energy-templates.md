# energy-templates.toml

This file is used to describe and configure the templates for energy models present in the simulation.  

## Contents
1. [File format](#file-format)
2. [Configuring Chassis Energy Model](#configuring-chassis-energy-model)
    1. [basic](#basic)
    2. [cpuLoad](#cpuload)
3. [Configuring Interface Energy Model](#configuring-interface-energy-model)
    1. [dataRate](#datarate)
<!--     2. [linear](#linear) -->
<!--     3. [complete](#complete) -->

---

### File format
```toml
[chassis]

[chassis.template1]
#<energy model configs>

[chassis.template2]
#<energy model configs>
#...

[interfaces]

[interfaces.template1]
#<energy model configs>

[interfaces.template2]
#<energy model configs>
#...
```
---

#### NOTE: At the moment, the `OffConso` values described in the following models are not used, since turning off devices is not yet supported by the simulator.

---

### Configuring Chassis Energy Model
```toml
[...]
model = "<model>"
#<model configs>
```

- `model`: The base model for this Energy model. The available models are `basic`, `cpuLoad`, `cpuLoadDiscrete` and `enabledPorts`. 

---

##### basic 
This energy model has a constant consumption depending on the device state.

```toml
[...]
model = "basic"
onConso = 50 
offConso = 20 
```

- `OnConso`: The switch's power consumption in Watts when it is on. 

- `OffConso`: The switch's power consumption in Watts when it is off.

---

##### cpuLoad | cpuLoadDiscrete
These energy models calculate the estimated energy consumption based on the CPU usage percentage of the switch. It's possible to define different consumption values for different usage percentages. In the normal `cpuLoad` model, actual consumption is calculated using linear interpolation between the defined values. In the `cpuLoadDiscrete` model, the consumption returned depends on the interval where the CPU usage fits.

```toml
[...]
model = "cpuLoad | cpuLoadDiscrete"
percentages = [0.0, 0.5, 1.0]
consumptions = [20.0, 40.0, 90.0]
```

- `percentages`: The discrete percentages steps **(From 0.0 to 1.0)**

- `consumptions`: The correspondent energy consumptions for each percentage step.

---

##### enabledPorts 
This energy model has a base/idle energy consumption plus a static consumption value per each connected interface depending on its speed. 

```toml
[...]
model = "enabledPorts"
idleConso = 108
dataRates = ["10Gbps", "25Gbps", "100Gbps"]
consumptions = [0.31, 0.52, 1.57]
```

- `idleConso`: The base/idle consumption of the chassis. 

- `dataRates`: The list of ports data rates.

- `consumptions`: The correspondent energy consumption for each port data rate.

---

### Configuring Interface Energy Model

```toml
[...]
model = "<model>"
#<model configs>
```

- `model`: The base model for this Energy model. Currently, only one model is available: `dataRate`. 

---

##### dataRate

This energy model calculates the estimate energy consumption from an interface based on the amount of traffic being processed. Depending on the interface configured speed, the consumption is calculated per each unit of traffic.

```toml
[...]
model = "dataRate"
dataRates = ["10Gbps", "25Gbps", "100Gbps"]
consumptions = [0.014, 0.013, 0.011]
unit = "1Gbps"
```

- `dataRates`: The list of data rates.

- `consumptions`: The correspondent energy consumption per unit depending on the data rate.

- `unit`: The traffic unit.
