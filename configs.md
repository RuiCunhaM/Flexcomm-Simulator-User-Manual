# configs.toml

This file is used to set global simulator configurations. 

## Contents
1. [File format](#file-format)
    1. [[simulator]](#[simulator])
    2. [[flowMonitor]](#[flowmonitor])
    4. [[ecofen]](#[ecofen])
    5. [[switchStats]](#[switchstats])
    5. [[linkStats]](#[linkstats])

---

### File format
The configuration file is composed of multiple sections. Each section controls different components of the simulator.

```toml
[simulator]
stopTime = "30min" 

[flowMonitor]
enable = true

[ecofen]
logFile = true
logInterval = "5s"
interval = "5s"

[switchStats]
enable = true
interval = "5s"

[linkStats]
logFile = true
logInterval = "5s"
interval = "5s"
```

---

##### [simulator]

- `stopTime`: When to stop the simulation. This can be considered as the simulation duration.

---

##### [flowMonitor]

- `enable`: When `true`, flow monitor data is captured. The default value is `false`.

---

<!-- ##### [log] -->

<!-- - Log components that when set to true, will output log messages in Debug builds. -->

<!-- **This section can be left empty.** -->

<!-- --- -->

##### [ecofen]

- `logFile`: When `true`, ECOFEN trace files are generated. The default value is `false`.

- `logInterval`: The interval to log energy consumption. The default value is `"5s"`.

- `interval`: The interval between energy consumption calculations. The default value is `"5s"`.

---

##### [switchStats]

- `enable`: When `true`, trace files with switch statistics are generated. The default value is `false`.

- `interval`: The interval to log information. The default value is `"5s"`.

---

##### [linkStats]

- `logFile`: When `true`, trace files are generated. The default value is `false`.

- `logInterval`: The interval to log information. The default value is `"5s"`. 

- `interval`: The interval between calculations. The default value is `"5s"`.

---
