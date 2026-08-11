# Exploring Your First Network

## 0. List Network Interfaces

### Objective

Display a concise overview of every network interface and its assigned addresses.

### Description

`list_interfaces.sh` is a Bash script that displays a brief summary of all network interfaces available in the current environment.

For each interface, the output shows:

- the interface name
- its operational state
- its assigned IPv4 and IPv6 addresses, when present

The script relies on `ip -br a`, which prints the brief address table in the format and ordering provided by the `iproute2` utility. This includes the loopback interface and interfaces that have no assigned address, without any additional sorting or reformatting.

### Usage

```
./list_interfaces.sh
```

### Input

Not applicable.

### Output

The complete brief interface-address table produced for the current environment, with no extra text before or after it.

Example:

```
lo               UNKNOWN        127.0.0.1/8 ::1/128
eth0@if5         UP             169.254.172.2/22 fe60::f003:dcaf:fe31:3e72/64
eth1             UP             10.42.125.74/16 fe80::46b:71ff:fafd:a2a5/64
```

This is an example only. Interface names, states, addresses, and the number of lines will differ between environments.

### Constraints

- No interface names, states, or addresses are hardcoded.
- The loopback interface is preserved.
- The utility's output is not sorted or reformatted.

### Files

- `list_interfaces.sh`: displays the brief network interface and address table using `ip -br a`.
