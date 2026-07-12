## Week 1 Architecture

#### 1. The three layers of the campus model and what lives at each.

**Access**.
    Switches that uses to connect of all endpoint devices like PC, wireless AP, printers, IP phones. Usually works as pure L2, but in modern networks often L3. There are better no to connect access switches with each other, only to Distribution level switches(usually to more than 1).
    
**Distribution(aggregation)**.
    Laying between Access and Core.  Responsible for fast and reliable transmission of traffic between all the other levels(or blocks), ACL, route summarization towards the Core). Usually uses L3(especially in the modern networks). The best practice is to connect to other distribution and core with a LAG. To this level connects all other blocks, like a WAN, server aggregation, etc.
    
**Core**.
    Uses to manage traffic between the uplinks and our network as fast as possible. All the functions of other layers should be excluded from Core.
    
### 2.  NSF/SSO vs StackWise vs VSS — what failure scenario each solves.
**SSO - Stateful Switchover**. 
    Hot‑standby mechanism that keeps the control plane in sync between an active and a backup RP (or between two redundant switches/routers). The control‑plane state is replicated from the active to the backup RP. 
    It solves: If the active RP goes down, the backup takes over in a few seconds, without resetting the line cards.
    
   **NSF - Non Stop Forwarding**.
    Works together with SSO and protects routing sessions during a switchover. Even if the outage is resolved very quickly by SSO, routing protocols could still reset peering. 
    It solves: With NSF, the line cards keep forwarding traffic using their existing CEF/FIB entries while the new RP rebuilds the routing table from neighbors, so routing sessions stay up instead of resetting.
    
   **StackWise**.
    Connecting few switches by a special stack cables to turn them into 1 stacked switch. Can be used to combine up to 8 switches(model depended). 
    Cons: all control plane managed by a Master(active) switch, so there is the one point of failure. Also often this means that management can be as laggy as the the stack getting larger(but this is not affect the data plane). Cables are very short so all the stack have to be in 1 place.
    It solves: we can made redundant topology without loops, or to extend interface budget.
    
    
   **VSS – Virtual Switching System**.
    Precursor to StackWise Virtual — roughly the same concept, but one is legacy and the other is modern.        
    Pros:
    -  Uses standard Ethernet cables (copper or fiber).    
    Cons:
    - Only 2 devices per stack.    
    - Unlike regular StackWise, the data plane is separate — so to build a LAG using interfaces from different switches in the "stack," a mechanism called Multichassis EtherChannel (MEC) is required.
    It solves: the same as StackWise.
   
### 3. On-prem vs cloud — what you trade for what (control, latency, cost, elasticity).

**On-prem** 
Pros: 
 - Total control - the physical infrastructure itself and all the software running on this hardware. 
 - 
Drawback:
 - Complexity - we have to think about our resources and scalability, backups, updates, about anything and also we need specialists in every topic to maintain it.

**Cloud**
Pros: 
- elasticity is one of the requirements of clouds and especially when we use some public cloud, not out private cloud, we shouldn't think about such things as storage or CPU upgrade
Cons:
- usually there should be cost, but this is not that simple and if we use some public cloud, we can dynamically change the resource pool based on our current need, so in the real life it actually can be cheaper, especially if we need a small bunch of resources(less stuff to maintain, no expensive hardware)
- In most scenarios latency will be higher, due to the added scheme complexity and the physical distance between the user and the cloud provider. However, there are exceptions — such as CDNs and cached content — where latency can actually be lower.