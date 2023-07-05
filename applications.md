# applications.toml

This file is used to describe and configure the applications present in the simulation. Applications generate traffic in the simulation. 

## Contents
1. [File format](#file-format)
2. [Configuring an Application](#configuring-an-application)
    1. [v4ping](#v4ping)
    2. [bulkSend](#bulksend)
    3. [constSend](#constsend)
    4. [sinSend](#sinsend)
    5. [PPBP](#ppbp)

---

### File format
**Each application name has to be unique.**

```toml
[app1]
#<application configs>

[app2]
#<application configs>
#...
```

---

### Configuring an Application

```toml
[app]
type = "<type>"
startTime = "1s"
stopTime = "60s"
host = "host1"
remote = "host2"
```

- `type`: The type of the application. The available types are: `v4ping`, `bulkSend`, `constSend`, `sinSend` and `PPBP`.

- `startTime`: The time to start the application. The default value is `"1s"`, and this is the lowest acceptable value.

- `stopTime`: The time to stop the application. The default value equals the simulation stop time.

- `host`: The host where to install this application.

- `remote`: The remote host where to send the traffic.

---

##### v4ping

An application that sends ICMP ECHO requests, waits for REPLYs and reports calculated RTT. More details [here](https://www.nsnam.org/docs/release/3.35/doxygen/classns3_1_1_v4_ping.html).

```toml
[app]
type = "v4ping"
interval = "1s"
verbose = true
```

- `verbose`: If `true`, once the application stops, statistics are shown. The default value is `true`.

- `interval`: The interval between each ping. The default value is `"1s"`.

---

##### bulkSend

```toml
[app]
type = "bulkSend"
protocol = "TCP"
sendSize = 512
maxBytes = 0
```

- `protocol`: The protocol used to generate traffic. The default value is `"UDP"`. `"TCP"` is also available. 

- `sendSize`: The size of each packet in bytes. The default value is `512`.

- `maxBytes`: The maximum Bytes to send. The default value is `0` (no limit).

---

##### constSend
This application generates traffic at a continuous Data Rate. See [OnOffApplication](https://www.nsnam.org/docs/release/3.35/doxygen/classns3_1_1_on_off_application.html) for more details.

```toml
[app]
type = "constSend"
protocol = "UDP"
dataRate = "500kb/s"
maxBytes = 0
packetSize = 512
```

- `protocol`: The protocol used to generate traffic. The default value is `"UDP"`. `"TCP"` is also available. 

- `dataRate`: The sustained data rate to generate the traffic. The default value is `"500kb/s"`

- `maxBytes`: The maximum Bytes to send. The default value is `0` (no limit).

- `packetSize`: The size of each packet in bytes. The default value is `512`.

---

##### sinSend

This application generates traffic with a Data Rate that changes in time following a `sin` function. The Data Rate $DR$ at a given time $t$ is given by: 
$$DR=A * sin(B * t + C) + Const$$ 

```toml
[app]
type = "sinSend"
protocol = "UDP"
const = "5Mb/s"
a = "1Mb/s"
b = 0.2
c = 0.0
packetSize = 1024
unit = "min"
```

- `protocol`: The protocol used to generate traffic. The default value is `"UDP"`. `"TCP"` is also available. 

- `const`: The constant Data Rate. The default value is `"3Mb/s"`.

- `a`: The amplitude of the variation in the constant Data Rate. The default value is `"1Mb/s"`.

- `b`: This factor controls the function's frequency. The default value is `0.5`

- `c`: This factor shifts the function left and right. The default value is `0.0`

- `packetSize`: The size of each packet in bytes. The default value is `1470`.

- `unit`: The time unit $t$ used to calculate the Data Rate. The default value is `"min"` (minutes).

---

##### PPBP

This application generates traffic based on a Poisson Pareto Burst Process. [Original implementation](https://github.com/doreidammar/PPBP---network-traffic-generator---ns3) by Doreid Ammar.

```toml
[app]
type = "PPBP"
protocol = "UDP"
burstIntensity = "100Mb/s" 
meanBurstArrivals = 20 
meanBurstTimeLength = "ns3::ConstantRandomVariable[Constant=0.2]"
h = 0.7 
packetSize = 1470
```

- `protocol`: The protocol used to generate traffic. The default value is `"UDP"`. `"TCP"` is also available. 

- `burstIntensity`: The Data Rate of each burst. The default value is `"100Mb/s"`.

- `meanBurstArrivals`: Mean active sources. The default value is `20`

- `meanBurstTimeLength`: Pareto distributed burst durations. The default value is `"ns3::ConstantRandomVariable[Constant=0.2]"`.

- `h`: The Hurst parameter. The default value is `0.7`

- `packetSize`: The size of each packet in bytes. The default value is `1470`.

---
