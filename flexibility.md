# Flexibility 

## flex.json
The `flex.json` file should include, for each network switch, the associated energy flexibility vector.

#### File Format
```jsonc
{
  "switch1": [
    100,
    -200,
    ...
  ],
  "switch2": [
    300,
    50,
    ...
  ],
  ...
}
```

## estimate.json
The `estimate.json` file should include, for each network switch, the associated energy consumption estimate.

```jsonc
{
  "switch1": [
    1502,
    3000,
    ...
  ],
  "switch2": [
    5123,
    3012,
    ...
  ],
  ...
}
```

