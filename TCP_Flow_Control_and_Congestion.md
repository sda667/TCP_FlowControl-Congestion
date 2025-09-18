# TCP Flow Control and Congestion  
**Author:** Ali Bahja  
**Date:** 21 November 2024  

---

## Objective
The purpose of this lab is to analyze the mechanisms of **TCP flow control and congestion control** using Wireshark captures.  
The lab investigates:
- How TCP adapts to receiver limitations  
- How it reacts to congestion  
- How it compares with Go-Back-N and Selective Repeat protocols  

---

## Topology and Setup
- Two clients download data from a server.  
- Wireshark captures **TCP segments, ACKs, retransmissions, and congestion window dynamics**.  
- TCP graphs are analyzed to identify **flow control** and **congestion control phases**.  

---

## Procedure and Results

### Flow Control (Client 2)
- 424 packets downloaded at **273 KB/s**.  
- Receiver advertises reduced window sizes → activates flow control.  
- Throughput limited by receiver capacity (not congestion).  

---

### Congestion Control (Client 1)
- 531 packets downloaded at **59 KB/s**.  
- Multiple **timeouts** → congestion threshold drops.  
- TCP’s **AIMD mechanism** gradually increases throughput after retransmission.  

Phases observed:
- Slow Start: rapid growth  
- Congestion Avoidance: linear growth  
- Multiplicative Decrease: after losses  

---

### ACK Number Calculation
- Last correct segment: **SEQ = 417322**  
- MSS = **1453**, 13 packets follow  
- Final ACK number:  

```
ACK_final = SEQ_last + (13 × MSS)  
           = 417322 + (1453 × 13)  
           = 436211
```

This confirms retransmissions are acknowledged and transmission resumes normally.  

---

### Comparison with Go-Back-N and Selective Repeat

**Similarities:**
- Like Go-Back-N → sliding window, retransmissions after timeouts.  
- Like Selective Repeat → retransmits only lost segments.  

**Differences:**
- Unlike Go-Back-N → no retransmission of already received segments (thanks to Fast Retransmit).  
- Unlike Selective Repeat → uses **cumulative ACKs**.  
- TCP integrates **congestion control** (AIMD, slow start), which ARQ protocols lack.  

---

## Analysis
- Flow control limits throughput by **receiver capacity**.  
- Congestion control adapts to **network conditions dynamically**.  
- TCP is more **realistic and efficient** than classical ARQ because it adds congestion management.  

---

## Conclusion
- Client 2: Receiver-limited throughput (flow control).  
- Client 1: Congestion avoidance with slow start, AIMD growth, retransmissions.  
- ACK analysis: Confirms correct delivery after losses.  
- TCP extends ARQ mechanisms by integrating congestion management.  
