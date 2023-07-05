# Units 
For more details on Time and Data Rate units refer to [ns-3's documentation](https://www.nsnam.org/doxygen/)

## Contents

1. [Time](#time)
2. [Data Rate](#data-rate)

---

### Time 

Time fields follow the format:
`"<value><unit>"`

- Supported units:

  ```
  s (seconds)
  ms (milliseconds)
  us (microseconds)
  ns (nanoseconds)
  ps (picoseconds)
  fs (femtoseconds)
  min (minutes)
  h (hours)
  d (days)
  y (years)
  ```

- Examples:
  ```
  "60s"
  "30min"
  "24h"
  ```

---

### Data Rate

Data Rate fields follow the format: `"<value><unit>"`

- Supported units:

  ```
  bps, b/s, Bps, B/s
  kbps, kb/s, Kbps, Kb/s, kBps, kB/s, KBps, KB/s, Kib/s, KiB/s
  Mbps, Mb/s, MBps, MB/s, Mib/s, MiB/s
  Gbps, Gb/s, GBps, GB/s, Gib/s, GiB/s
  ```

- Examples:
  ```
  "128kb/s"
  "1Gbps"
  ```
