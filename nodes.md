# nodes.toml

This file is used to describe and configure the nodes present in the simulation. 

## Contents 
1. [File format](#file-format)
2. [Configuring a Node](#configuring-a-node)
    1. [Configuring a Host](#configuring-a-host)
    2. [Configuring a Switch](#configuring-a-switch)
        1. [Energy Models](#energy-models)
            1. [Chassis](#chassis)
            2. [Interfaces](#interfaces)

---

### File format
**Each node name has to be unique.**
```toml
[node1]
# <node1 configurations>

[node2]
# <node2 configurations>
# ...
```

---

### Configuring a Node

```toml
[node]
type = "<type>"
rank = 0
```

- `type`: The type of this node. The available types are `host` and `switch`

- `rank`: The MPI rank where this node will be created. The default value is `0`. This parameter is only required when running distributed simulations.

---

#### Configuring a Host
```toml
[node]
type = "host"
```

**At the moment, hosts have no other extra parameters.**

---

#### Configuring a Switch
```toml
[node]
type = "switch"
cpuCapacity = "1Gbps"
mibs = ["energy"]
```

- `cpuCapacity`: The CPU capacity of this switch. The default value is `"100Gb/s"`.  

- `mibs`: A list containing the MIBS to be available in this switch. This parameter is only required when `MibLogger` is enabled. The default value is `[]`. 

---

##### Energy Models
 
With ECOFEN, different energy models can be assigned to switches. 
<!-- A switch can have just a `Chassis` energy model, or both `Chassis` and `Interface` energy models.  -->

Any type of energy model can be defined exclusively for a particular switch, or a previously defined template in [energy-templates.toml](energy-templates.md) can be referenced.

---

###### Chassis
Reference a template:
```toml
[node.chassis]
template = "<template name>"
```

Define model:
```toml
[node.chassis]
model = "<model>"
#<model configs>
```

---

###### Interfaces
Reference a template:
```toml
[node.interfaces]
template = "<template name>"
```

Define model:
```toml
[node.interfaces]
model = "<model>"
#<model configs>
```

---

For more details on how to define an energy model refer to [energy-templates.toml](energy-templates.md).
