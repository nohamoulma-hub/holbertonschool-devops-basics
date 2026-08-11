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

## 1. Inspect Network Links

### Objective

Display link-layer information and identify the hardware address associated with each network interface.

### Description

`show_links.sh` is a Bash script that displays brief link-layer information for every network interface available in the current environment.

For each interface, the output shows:

- the interface name
- its operational state
- its link-layer address, when available
- its interface flags

The script relies on `ip -br link`, which prints the brief link table in the format and ordering provided by the `iproute2` utility. This includes the loopback interface, without any additional sorting or reformatting.

### Usage

```
./show_links.sh
```

### Input

Not applicable.

### Output

The complete brief link table produced for the current environment, with no labels or explanations added.

Example:

```
lo               UNKNOWN        00:00:00:00:00:00 <LOOPBACK,UP,LOWER_UP>
eth0@if5         UP             f2:03:da:51:3e:72 <BROADCAST,MULTICAST,UP,LOWER_UP>
eth1             UP             06:7b:70:fa:a2:c5 <BROADCAST,MULTICAST,UP,LOWER_UP>
```

This is an example only. Link-layer addresses and flags will differ between environments.

### Constraints

- No interface names, states, link-layer addresses, or flags are hardcoded.
- No interface is removed from the result.
- The utility's output is not sorted or reformatted.

### Files

- `show_links.sh`: displays the brief network link table using `ip -br link`.

## 2. Test the IPv4 Loopback Interface

### Objective

Discover an IPv4 loopback address from the current system configuration and use it to test the local network stack.

### Description

`test_loopback.sh` is a Bash script that:

- lists the IPv4 addresses whose scope is limited to the local host (`ip -4 -br addr show scope host`);
- reduces repeated spaces in the brief listing and selects the address column (`tr -s ' '` and `cut -d ' ' -f 3`);
- selects the first address shown (`head -n 1`);
- removes its CIDR prefix (`cut -d '/' -f 1`);
- sends exactly four ICMP echo requests to the resulting IPv4 address via command substitution (`ping -c 4 "$(...)"`).

The loopback interface name and address are discovered from the current system configuration and are never hardcoded.

### Usage

```
./test_loopback.sh
```

### Input

The IPv4 interface configuration of the execution environment. No command-line arguments are required.

### Output

The complete output of four ICMP echo requests sent to the discovered host-scoped IPv4 address.

Example:

```
PING 127.0.0.1 (127.0.0.1) 56(84) bytes of data.
64 bytes from 127.0.0.1: icmp_seq=1 ttl=64 time=0.030 ms
64 bytes from 127.0.0.1: icmp_seq=2 ttl=64 time=0.035 ms
64 bytes from 127.0.0.1: icmp_seq=3 ttl=64 time=0.033 ms
64 bytes from 127.0.0.1: icmp_seq=4 ttl=64 time=0.029 ms

--- 127.0.0.1 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3070ms
rtt min/avg/max/mdev = 0.029/0.031/0.035/0.002 ms
```

This is an example only. The selected address and timing values will differ between environments.

### Constraints

- The address is discovered from the current system configuration, not hardcoded.
- The first IPv4 address with host scope is selected.
- The interface is never assumed to be named `lo`.
- Neither `127.0.0.1` nor another loopback address is hardcoded.
- The CIDR prefix is removed before using the address.
- Exactly four ICMP echo requests are sent.
- The complete output of the connectivity-testing utility is preserved.
- `awk` and `sed` are not used.
- The script contains exactly one command line after the shebang.

### Files

- `test_loopback.sh`: discovers the host-scope IPv4 address and pings it four times using `ip`, `tr`, `cut`, `head`, and `ping`.
