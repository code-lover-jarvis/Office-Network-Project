# IP Addressing & VLSM Plan

## 1. Parent Network

The office network uses the private IPv4 address block:

**10.0.0.0/24**

The /24 network provides 256 total addresses, from 10.0.0.0 to 10.0.0.255.

VLSM (Variable Length Subnet Masking) is used to divide the parent network into subnets of different sizes according to the host requirements of each department.

## 2. Host Requirements

| VLAN | Department  | Required Hosts | Subnet |
| ---: | ----------- | -------------: | -----: |
|   10 | Development |             60 |    /26 |
|   20 | Guest       |             50 |    /26 |
|   30 | Sales       |             35 |    /26 |
|   40 | Finance     |             12 |    /28 |
|   50 | Servers     |             10 |    /28 |
|   60 | HR          |             10 |    /28 |
|   99 | Management  |             10 |    /28 |

## 3. VLSM Allocation

| VLAN | Department  | Network    | CIDR | First Usable | Last Usable | Broadcast  | Gateway    |
| ---: | ----------- | ---------- | ---: | ------------ | ----------- | ---------- | ---------- |
|   10 | Development | 10.0.0.0   |  /26 | 10.0.0.1     | 10.0.0.62   | 10.0.0.63  | 10.0.0.1   |
|   20 | Guest       | 10.0.0.64  |  /26 | 10.0.0.65    | 10.0.0.126  | 10.0.0.127 | 10.0.0.65  |
|   30 | Sales       | 10.0.0.128 |  /26 | 10.0.0.129   | 10.0.0.190  | 10.0.0.191 | 10.0.0.129 |
|   40 | Finance     | 10.0.0.192 |  /28 | 10.0.0.193   | 10.0.0.206  | 10.0.0.207 | 10.0.0.193 |
|   50 | Servers     | 10.0.0.208 |  /28 | 10.0.0.209   | 10.0.0.222  | 10.0.0.223 | 10.0.0.209 |
|   60 | HR          | 10.0.0.224 |  /28 | 10.0.0.225   | 10.0.0.238  | 10.0.0.239 | 10.0.0.225 |
|   99 | Management  | 10.0.0.240 |  /28 | 10.0.0.241   | 10.0.0.254  | 10.0.0.255 | 10.0.0.241 |

## 4. Address Allocation Strategy

The first usable address in each subnet is reserved for the default gateway.

DHCP clients will receive addresses from the remaining usable address space, while infrastructure devices such as servers, network management interfaces, and other devices requiring predictable addresses will use statically assigned or reserved addresses.

## 5. VLSM Efficiency

The subnet sizes were selected according to the number of hosts required by each department rather than assigning every department the same subnet size.

The total allocation is:

* 3 × /26 = 192 addresses
* 4 × /28 = 64 addresses
* Total = 256 addresses

Therefore, the complete 10.0.0.0/24 address space is allocated without unused address blocks between the departmental subnets.

## 6. Design Rationale

VLSM provides more efficient utilization of the available IPv4 address space. Larger departments receive /26 networks providing 62 usable host addresses, while smaller departments receive /28 networks providing 14 usable host addresses.

The VLAN ID and subnet assignment are intentionally aligned to make the network easier to understand and troubleshoot.
