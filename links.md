# links.toml

This file is used to describe and configure the links present in the simulation. 

## Contents

1. [File format](#file-format)
2. [Configuring a Link](#configuring-a-link)

---

### File format
**Each link name has to be unique.**

```toml
[link1]
#<link configs>

[link2]
#<link configs>
#...
```

---

### Configuring a link
```toml
[link]
edges = ["node1", "node2"]
dataRate = "1Gbps"
delay = "1ms"
pcap = true
```

- `edges`: List containing the two nodes connected by this link.

- `dataRate`: The data rate of this link. The default value is `"1Gbps"`.

- `delay`: The delay of this link. The default value is `"1ns"`.

- `pcap`: When `true`, a capture is created for each interface of this link. The default value is `false`.

---

