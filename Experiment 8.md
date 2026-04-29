# Experiment 8

## Aim:
Setup & simulate a basic network topology to understand the fundamental components of NS2.

---

## Theory:

- **UDP (User Datagram Protocol):**  
A connectionless protocol used for data transfer where speed is prioritized over reliability.

- **CBR (Constant Bit Rate):**  
A traffic generator that sends packets at a fixed rate (interval).

- **Agents:**  
In NS2, agents represent endpoints where packets are created or consumed (e.g., UDP agent for sending, NULL agent for receiving).

---

## Procedure:

### Step 1: Script Creation

- Open the terminal in Ubuntu & create a new script file:

gedit ex2.tcl

Enter the following script:
set ns [new Simulator]

set tf [open out.tr w]
$ns trace-all $tf

set nf [open out.nam w]
$ns namtrace-all $nf

set n0 [$ns node]
set n1 [$ns node]

$ns duplex-link $n0 $n1 1Mb 10ms DropTail

set udp [new Agent/UDP]
set null [new Agent/Null]

$ns attach-agent $n0 $udp
$ns attach-agent $n1 $null

$ns connect $udp $null

set cbr [new Application/Traffic/CBR]
$cbr set packet_size_ 500
$cbr set interval_ 0.01
$cbr attach-agent $udp

$ns at 0.5 "$cbr start"
$ns at 4.5 "$cbr stop"

proc finish {} {
    global ns tf nf
    $ns flush-trace
    close $tf
    close $nf
    exec nam out.nam &
    exit 0
}

$ns at 5.0 "finish"

$ns run

*Step 2: Traffic Configuration
Attach a UDP agent to node n0 & a NULL agent to node n1
Connect both agents to establish a path
Attach a CBR application to the UDP agent
Set:
Packet size = 500 bytes
Interval = 0.01

*Step 3: Execution
Schedule traffic:
Start at 0.5s
Stop at 4.5s
Run the simulation: ns ex2.tcl

Observations:
When the NAM window opens, simulation starts
At 0.5s packets begin moving from n0 to n1
The flow remains constant till 4.5s, after which transmission stops
The trace file records every packet as:
"enqueued"
"dequeued"
"received"
Result:

A point-to-point network with UDP traffic was successfully simulated.
The experiment demonstrated how agents & applications work together in NS2 to generate network load.
