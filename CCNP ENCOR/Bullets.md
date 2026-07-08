## Week 1 Architecture##
**Bullets to write from memory:** 
1. The three layers of the campus model and what lives at each.
2. NSF/SSO vs StackWise vs VSS — what failure scenario each solves.
3. On-prem vs cloud — what you trade for what (control, latency, cost, elasticity).

Answers:
1.  Access - switches that uses to connect of all endpoint devices like PC, wireless AP, printers, IP phones. Usually works as pure L2, but in modern networks often L3. There are better no to connect access switches with each other, only to Distribution level switches(usually to more than 1).
    Distribution(aggregation) - laying between Access and Core. Responsible for fast and reliable transmission of traffic between all the other levels(or blocks), ACL, route summarization towards the Core). Usually uses L3(especially in the modern networks). The best practice is to connect to other distribution and core with a LAG. To this level connects all other blocks, like a WAN, server aggregation, etc.
     Core - uses to manage traffic between the uplinks and our network as fast as possible. All the functions of other layers should be excluded from Core.
2. SSO - Stateful Switchover. 
    Hot‑standby mechanism that keeps the control plane in sync between an active and a backup RP (or between two redundant switches/routers). The control‑plane state is replicated from the active to the backup RP. If the active RP goes down, the backup takes over in a few seconds, without resetting the line cards.
   NSF - Non Stop Forwarding.
    Works together with SSO and protects routing sessions during a switchover. Even if the outage is resolved very quickly by SSO, routing protocols could still reset peerings. With NSF, the line cards keep forwarding traffic using their existing CEF/FIB entries while the new RP rebuilds the routing table from neighbors, so routing sessions stay up instead of resetting.
   StackWise.
    Connecting few switches by a special stack cables to turn them into 1 stacked switch. Can be used to combine up to 8 switches(model depended). Cons: all control plane managed by a Master(active) switch, so there is the one point of failure. Also often this means that management can be as laggy as the the stack getting larger(but this is not affect the data plane). Cables are very short so all the stack have to be in 1 place.
   VSS – Virtual Switching System
    Precursor to StackWise Virtual — roughly the same concept, but one is legacy and the other is modern.
   **Pros:** Uses standard Ethernet cables (copper or fiber).
**Cons:**
- Only 2 devices per stack.    
- Unlike regular StackWise, the data plane is separate — so to build a LAG using interfaces from different switches in the "stack," a mechanism called Multichassis EtherChannel (MEC) is required.
   
4. 
5.  