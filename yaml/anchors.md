# YAML Anchors

Anchors are a powerful YAML feature that help you populate values multiple times without having to retype anything. They allow you to define reusable content once and reference it throughout your YAML document.

## Basic Example

Let's say we have a YAML file for some servers:

```yaml
---
server1:
    make: Lenovo
    model: ThinkSystem SR650 V3
    max_cpu_cores: 64
    max_memory: 1000
    location: Poole
    county: Dorset
```

This is simple and easy, but let's say we have many SR650 servers with different specs but the same location. Repeating that information for 5 or more servers is tedious - this is where anchors come in.

## How Anchors Work

YAML anchors use two key symbols:
- `&anchor_name` - defines an anchor (creates a reference point)
- `*anchor_name` - references an anchor (uses the defined content)
- `<<:` - merge key operator (merges the referenced content into the current map)

## Improved Example with Anchors

```yaml
---
lenovo_thinksystem_sr650_v3: &lenovo_sr650
    make: Lenovo
    model: ThinkSystem SR650 V3

dorset_west_1: &dorset_location
    location: Poole
    county: Dorset

server1:
    <<: *lenovo_sr650
    max_cpu_cores: 128
    max_memory: 2500
    <<: *dorset_location

server2:
    <<: *lenovo_sr650
    max_cpu_cores: 64
    max_memory: 1000
    <<: *dorset_location
```

This way we can write some boilerplate once but reuse it easily throughout our configuration. The `<<:` merge key takes all the key-value pairs from the referenced anchor and merges them into the current mapping.

## Benefits

- **Reduces repetition**: Define common configurations once
- **Easier maintenance**: Update shared values in one place
- **Cleaner files**: Less clutter and duplication
- **Fewer errors**: Less chance of typos in repeated sections

Nice!