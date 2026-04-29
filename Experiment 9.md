# Experiment 9

## Aim:
Stimulate common network attack like DoS (Denial of Service) in NS2.

---

## Theory:

- **DoS Attack:**  
An attempt to make a network resource unavailable to its intended users by flooding the target with superfluous traffic.

- **Congestion:**  
When the volume of traffic exceeds the capacity of a network link, causing overload (in this case, link between router n2 & destination n3).

- **Packet Drop:**  
Occurs when the router’s queue limit is reached, causing incoming packets to be discarded.

---

## Procedure:

### Step 1: Network Topology & Script Creation

- Open terminal & create a new file:

gedit ex3.tcl

Enter the following script:
set ns [new Simulator]

set tf [open dos-attack.tr w]
$ns trace-all $tf

set nf [open dos-attack.nam w]
$ns namtrace-all $nf

proc finish {} {
    global ns tf nf
    $ns flush-trace
    close $tf
    close $nf
    exec nam dos-attack.nam &
    exit 0
}

set n0 [$ns node]
set n1 [$ns node]
set n2 [$ns node]
set n3 [$ns node]

$ns duplex-link $n0 $n2 1Mb 10ms DropTail
$ns duplex-link $n1 $n2 1Mb 10ms DropTail
$ns duplex-link $n2 $n3 1Mb 10ms DropTail

$ns queue-limit $n2 $n3 10

# Normal Traffic
set udp0 [new Agent/UDP]
$ns attach-agent $n0 $udp0

set null0 [new Agent/Null]
$ns attach-agent $n3 $null0

$ns connect $udp0 $null0

set cbr0 [new Application/Traffic/CBR]
$cbr0 set packet_size_ 500
$cbr0 set interval_ 0.5
$cbr0 attach-agent $udp0

# Attack Traffic
set udp1 [new Agent/UDP]
$ns attach-agent $n1 $udp1

set null1 [new Agent/Null]
$ns attach-agent $n3 $null1

$ns connect $udp1 $null1

set attack [new Application/Traffic/CBR]
$attack set packet_size_ 1000
$attack set interval_ 0.001
$attack attach-agent $udp1

# Colors
$ns color 1 Blue
$ns color 2 Red
$udp0 set fid_ 1
$udp1 set fid_ 2

# Scheduling
$ns at 0.5 "$cbr0 start"
$ns at 1.0 "$attack start"
$ns at 4.0 "$attack stop"
$ns at 4.0 "$cbr0 stop"
$ns at 5.0 "finish"

$ns run

*Step 2: Topology & Queue Setup
Four nodes created:
n0 (Sender)
n1 (Attacker)
n2 (Router)
n3 (Destination)
Queue limit set to 10 on bottleneck link (n2 → n3) to observe congestion

*Step 3: Traffic Configuration
Normal Traffic:
UDP agent from n0 (Blue)
Packet interval = 0.5
Attack Traffic:
High-speed UDP from n1 (Red)
Interval = 0.001
Larger packet size

*Step 4: Execution
ns ex3.tcl
Normal traffic starts at 0.5s
Attack starts at 1.0s & stops at 4.0s

*Observations:
Before 1.0s, blue packets reach destination smoothly
After 1.0s, red packets from attacker flood the router
Link between n2 & n3 becomes congested
Both red & blue packets start dropping due to queue overflow

*Result:
The experiment successfully demonstrated how a DoS attack disrupts a network by consuming bandwidth & causing significant packet loss for legitimate users.
