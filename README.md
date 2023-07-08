# Flexcomm Simulator Documentation 

## Contents

1. [Configuration Files](#configuration-files)
    1. [Topologies Directory Structure](#directory-structure)
    2. [Files](#files)
2. [Runtime Flags](#runtime-flags)
3. [Internal Controller](#internal-controller)
4. [External Controller](#external-controller)
5. [EnergyAPI](#energyapi)
6. [SNMP](#snmp)

---

## Configuration files

#### Directory Structure

The configurations for each topology should be placed inside the `topologies` folder and follow the following structure:

```
├── topologies
│   ├── energy-templates.toml
│   ├── topology1
│   │   ├── applications.toml
│   │   ├── configs.toml
│   │   ├── links.toml
│   │   ├── nodes.toml
│   │   ├── flex.json
│   │   ├── flex2.json
│   │   └── estimate.json
│   └── topology2
│       ├── applications.toml
│       ├── configs.toml
│       ├── links.toml
│       ├── nodes.toml
│       ├── flex.json
│       ├── estimate.json
│       ├── estimate2.json
│       └── energy-templates.toml
... 
...
```

#### Files
Each topology has its own configuration files:
  - [`nodes.toml`](nodes.md)
  - [`links.toml`](links.md)
  - [`applications.toml`](applications.md)
  - [`configs.toml`](configs.md)
  - [`energy-templates.toml`](energy-templates.md) (optional)
  - [`flex.json`](flexibility.md) (optional)
  - [`estimate.json`](flexibility.md) (optional)

The top-level `energy-templates.toml` file applies to all the configurations. However, each topology can also specify a local templates file. In this case, for templates with equal names, the local file overrides the global configuration. 

**For how to specify different measurement units see [Units](units-fomat.md).**

---

## Runtime Flags

| Flag | Description | Default Value |
| ---- | ----------- | ------------- |
| TOPO | The topology scenario to simulate | example |
| CHECKSUM | If checksums should be calculated | false |
| CONTROLLER | Which controller to use (`ns3::Simple`, `ns3::SimpleFlex`, `External`, `ns3::OFSwitch13LearningController`, etc...) | Simple |
| OUTPUTS | The output path | outputs |
| ESTIFILE | The energy estimate file to load | estimate.json |
| FLEXFILE | The flexibility file to load | flex.json |

---

## Internal Controller
Examples of Internal Controllers are provided with the Flexcomm Simulator (*e.g,:* `ns3::SimpleController`). To develop new Internal Controllers refer to [Working with custom Controllers](controllers.md). 

---

## External Controller
To use an External SDN Controller (e.g.: `ryu`, `onos`, etc) with the simulator, the SDN Controller must be running in the localhost at port 6653 before starting the simulation. The simulation can then be launched with the `CONTROLLER` flag set to `External`

**Note:** You may be required to run the simulation with `sudo`

- Example:
`sudo make run CONTROLLER=External`


##### LLDP Notes
Some SDN controllers, like `onos`, use Link Layer Discovery Protocol (LLDP) by default to survey the network. This is not compatible with the Flexcomm Simulator and such option must be disabled in the Controller to avoid conflicts. To generate a configuration file replacing LLDP and containing the topology characteristics use the `switches-config.py` script in the `utils` folder.

---

## EnergyAPI
The `EnergyAPI` can be used by Internal Controllers to access the energy data provided in the flexibility configuration files using the methods `EnergyAPI::GetFlexArrayConsumption(<id/name>)` and `EnergyAPI::GetEstimateArray(<id/name>)`

For External Controllers, an external HTTP server is started on port 5000. Data can be accessed using the endpoints `localhost:5000/flex?id=<id/name>` and `localhost:5000/estimate?id=<id/name>`

---

## SNMP
The Flexcomm Simulator provides a simple SNMP implementation so that External Controller can access switches' data using this protocol. To achieve this, if the `MibLogger` is enabled and an External Controller is in use, an instance of `snmpsim` is started.

SNMP requests can then be issued to localhost from port 1024 onwards, depending on the number of switches being simulated. 

This implementation is intended only to access data, therefore no updates can be made by the controller. For an example of how to create a `MIB` see [MibLogger](https://github.com/RuiCunhaM/Flexcomm-Simulator/blob/master/ns-3.35/src/snmp/model/energy-mib.cc)
