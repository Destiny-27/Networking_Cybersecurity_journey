# Week 1 — Network Discovery

**Course:** Networking and Systems Administration — Cohort 1
**Assignment:** Local Network Discovery & Mapping

## Introduction

Before you can secure or test a network, you first need to learn how to analyze your own environment. This project uses native terminal tools on Windows to explore my local network, map connected devices, and document the findings.

## Step 1: Access the command line

Opened Command Prompt on Windows (`Win + R` → `cmd` → Enter).

## Step 2: Run diagnostic commands and analyze the output

### Command: `ipconfig`

**What it does:** shows my computer's own network settings.

```
Wireless LAN adapter Wi-Fi:

   Connection-specific DNS Suffix  . :
   Link-local IPv6 Address . . . . . : fe80::5f60:8b5c:89c4:5e44%6
   IPv4 Address. . . . . . . . . . . : 172.20.10.2
   Subnet Mask . . . . . . . . . . . : 255.255.255.240
   Default Gateway . . . . . . . . . : 172.20.10.1
```

**What it means:**

| Term | Meaning | Value found |
|---|---|---|
| IPv4 address | My computer's own address on this network | `172.20.10.2` |
| Subnet mask | How large the local network is | `255.255.255.240` |
| Default gateway | The device that connects me to the internet | `172.20.10.1` |

The subnet mask `255.255.255.240` allows for only 16 addresses total, which points to a mobile hotspot network (in this case, an iPhone Personal Hotspot) rather than a full home router.

### Command: `arp -a`

**What it does:** lists other devices my computer has recently exchanged data with on the local network.

```
Interface: 172.20.10.2 --- 0x6
  Internet Address      Physical Address      Type
  172.20.10.1            36-fe-77-a1-94-64     dynamic
  172.20.10.15           ff-ff-ff-ff-ff-ff     static
  224.0.0.22              01-00-5e-00-00-16    static
  224.0.0.251             01-00-5e-00-00-fb    static
  224.0.0.252             01-00-5e-00-00-fc    static
  239.255.255.250         01-00-5e-7f-ff-fa    static
  255.255.255.255         ff-ff-ff-ff-ff-ff    static
```

**What it means:**

| Address | MAC address | What it actually is |
|---|---|---|
| `172.20.10.1` | `36-fe-77-a1-94-64` | Real device — the default gateway (hotspot) |
| `172.20.10.15` | `ff-ff-ff-ff-ff-ff` | Not a device — subnet broadcast address |
| `224.0.0.22` | `01-00-5e-00-00-16` | Not a device — multicast (IGMP) |
| `224.0.0.251` | `01-00-5e-00-00-fb` | Not a device — multicast (mDNS) |
| `224.0.0.252` | `01-00-5e-00-00-fc` | Not a device — multicast (LLMNR) |
| `239.255.255.250` | `01-00-5e-7f-ff-fa` | Not a device — multicast (SSDP) |
| `255.255.255.255` | `ff-ff-ff-ff-ff-ff` | Not a device — limited broadcast |

**Note on device count:** `arp -a` only lists devices a computer has actually communicated with — it doesn't scan the whole network. At the time of testing, no other phones, tablets, or computers were connected to this hotspot, so the gateway was the only real device the ARP cache could show. This is the expected, correct result for a single-client network, not a missing step.

### Command: `getmac`

**What it does:** shows my computer's own MAC address (a unique hardware ID).

```
Physical Address    Transport Name
==================  ==========================================================
C8-B2-9B-82-F5-87   \Device\Tcpip_{41C008CD-5C02-44B8-B30B-0316D340A69B}
C8-B2-9B-82-F5-8B   Media disconnected
```

My active Wi-Fi adapter's MAC address: **`C8-B2-9B-82-F5-87`**

### Command: `ping 8.8.8.8`

**What it does:** sends test messages to Google's public DNS server to check internet connectivity and speed.

```
Pinging 8.8.8.8 with 32 bytes of data:
Reply from 8.8.8.8: bytes=32 time=221ms TTL=113
Reply from 8.8.8.8: bytes=32 time=26ms TTL=113
Reply from 8.8.8.8: bytes=32 time=47ms TTL=113
Reply from 8.8.8.8: bytes=32 time=53ms TTL=113

Ping statistics for 8.8.8.8:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 26ms, Maximum = 221ms, Average = 86ms
```

**What it means:** all 4 test messages made it there and back with 0% loss — the internet connection is healthy. An average response time (latency) of 86ms is fast; anything under ~100ms feels instant to a person.

## Step 3: Network map

A visual diagram of the network — showing my machine, the default gateway, and the external ping target — is included as `network-diagram.html` in this repository.

**Summary of what the map identifies:**

| Required item | Value |
|---|---|
| My machine's IP address | `172.20.10.2` |
| My machine's MAC address | `C8-B2-9B-82-F5-87` |
| Router / default gateway IP | `172.20.10.1` |
| Other connected devices | None discovered — see note below |

**Why no additional devices are shown:** this network was a single mobile hotspot with only my laptop connected to it. No other devices (phone, tablet, secondary computer) were available to join during testing, so the ARP table genuinely had nothing more to discover beyond the gateway. Every other entry in the raw `arp -a` output was a broadcast or multicast address, not a real device, as explained in the table above. This reflects an accurate and complete picture of the network as it existed at the time of testing.

## Files in this repository

- `Week1-Network-Discovery.md` — this report
- `network-diagram.html` — visual network diagram
