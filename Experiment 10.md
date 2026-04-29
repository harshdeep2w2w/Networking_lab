# Experiment 10

## Aim:
Simulating & analyzing the performance of network security protocols such as IPsec.

---

## Theory:

IPsec provides security by encrypting data packets. However, this process adds extra bytes (overhead) to each packet due to security headers (AH/ESP).  
In this experiment, we simulate this effect by increasing the packet size of the secure flow to observe how the extra overhead affects network performance.

---

## Procedure:

### Step 1: Script Creation

- Open the Ubuntu terminal & create the file:

gedit ex4.tcl

Enter the following script:
set ns [new Simulator]

set tf [open ipsec.tr w]
$ns trace-all $tf

set nf [open ipsec.nam w]
$ns namtrace-all $nf

proc finish {} {
    global ns tf nf
    $ns flush-trace
    close $tf
    close $nf
    exec nam ipsec.nam &
    exit 0
}

set n0 [$ns node]
set n1 [$ns node]
set n2 [$ns node]

$ns duplex-link $n0 $n1 1Mb 10ms DropTail
$ns duplex-link $n1 $n2 1Mb 10ms DropTail

$ns queue-limit $n1 $n2 20

# Normal Traffic (Green)
set udp1 [new Agent/UDP]
$ns attach-agent $n0 $udp1

set null1 [new Agent/Null]
$ns attach-agent $n2 $null1

$ns connect $udp1 $null1

set cbr1 [new Application/Traffic/CBR]
$cbr1 set packet_size_ 500
$cbr1 set interval_ 0.02
$cbr1 attach-agent $udp1

# IPsec-like Traffic (Orange - larger packets)
set udp2 [new Agent/UDP]
$ns attach-agent $n0 $udp2

set null2 [new Agent/Null]
$ns attach-agent $n2 $null2

$ns connect $udp2 $null2

set cbr2 [new Application/Traffic/CBR]
$cbr2 set packet_size_ 600
$cbr2 set interval_ 0.02
$cbr2 attach-agent $udp2

# Colors
$ns color 1 Green
$ns color 2 Orange
$udp1 set fid_ 1
$udp2 set fid_ 2

# Scheduling
$ns at 1.0 "$cbr1 start"
$ns at 3.0 "$cbr1 stop"

$ns at 3.0 "$cbr2 start"
$ns at 5.0 "$cbr2 stop"

$ns at 5.5 "finish"

$ns run

*Step 2: Topology Setup
Three nodes created:
n0 (Sender)
n1 (Router)
n2 (Receiver)
Nodes connected using duplex links
Queue limit of 20 set on bottleneck link

*Step 3: Traffic Configuration
Normal Traffic (Green):
UDP agent with packet size = 500 bytes
IPsec-like Traffic (Orange):
UDP agent with larger packet size = 600 bytes
Simulates security overhead
Both flows use same interval (0.02) for fair comparison

*Step 4: Execution
ns ex4.tcl
Normal traffic runs from 1.0s to 3.0s
IPsec traffic runs from 3.0s to 5.0s

*Observations:
Normal flow: Green packets move smoothly across the link
IPsec flow: Orange packets are larger due to added overhead
Larger packets consume more bandwidth even at same rate
Trace file shows higher byte count for IPsec traffic

*Result:
The simulation successfully demonstrated the impact of IPsec overhead on network traffic.
Although security is improved, larger packet size leads to higher bandwidth consumption compared to standard unencrypted data.
