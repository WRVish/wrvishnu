# Microsoft Connected Cache (MCC) for Intune Devices Without Internet Access

Status: Draft high-level approach and PoC plan

## 1. Context and assumptions
- Some Windows devices have no internet access but can reach the internal network, including a distribution point (DP) / internal server.
- Goal: deliver software and packages to these devices using Intune.
- Assumptions (to validate): devices are Intune-enrolled; an Azure subscription is available; a Windows or Linux host can be provisioned for the cache node.

## 2. Key constraint to validate first
MCC offloads content **downloads** only. Intune management traffic (check-in, policy, app assignment and status reporting) still needs a route to Microsoft cloud services.

- If the devices can reach the internet only through an internal proxy or gateway, MCC is a good fit.
- If the devices are fully isolated, MCC alone will not work. Consider the alternatives in section 8.

Also confirm that the content types are supported: Intune Win32 apps, Windows Update, Store apps, Edge and Office. Custom LOB packages may be limited.

## 3. Proposed architecture
1. A Connected Cache node (Microsoft Connected Cache for Enterprise and Education, deployed on Windows with WSL2 or on Linux) sits on the internal network next to the DP.
2. The node is registered in an Azure resource (Azure subscription required) and pulls content from Microsoft CDN, including Intune Win32 content.
3. Intune policy (Cache Host / DOCacheHost via Delivery Optimization settings) points clients to the cache node. DHCP option 235 is an alternative.
4. Clients download through Delivery Optimization from the cache node and fall back to the CDN if it is unavailable.
5. The cache node needs outbound access to the required Microsoft endpoints. Allow-list only that host through the firewall or proxy.

## 4. Planned activities
| Phase | Activities |
|---|---|
| 0. Discovery | Answer open questions (section 7). Inventory devices, OS versions, sites and subnets. Identify content types and package sizes. Confirm the network path from devices to Intune and the DP. |
| 1. Design | Choose a host (VM or physical, sizing and disk based on content footprint). Plan network, firewall and allow-list requests. Design the Delivery Optimization policy and device groups. Agree on security review and approvals. |
| 2. PoC build | Create the Azure subscription and MCC resource. Provision the cache host. Install and register the node. Open the required firewall rules. |
| 3. PoC test | Run the test cases in section 6. Collect Delivery Optimization logs and metrics. |
| 4. Pilot | Expand to a small pilot group (10-50 devices) across one site. Monitor cache hit ratio and failures. |
| 5. Rollout | Phased deployment by site. Write the operations runbook (monitoring, updates, capacity). Hand over to support. |

## 5. PoC scope and test devices
**Infrastructure**
- 1 cache host: a Windows 11/Server VM with WSL2 or a Linux VM; 4+ vCPU, 8+ GB RAM, and 100+ GB cache disk (size to workload).
- 1 Azure subscription with permissions to create the MCC resource.
- Firewall and proxy rule changes for the cache host.

**Test devices**
- 2-3 Intune-enrolled Windows 10/11 devices on the **same subnet/internal network** as the target offline devices, configured the same way (internet-restricted).
- 1 control device with normal internet access, for baseline comparison.
- A test Entra group for the DO cache host policy.

**Test content**
- 1-2 Win32 apps (one small, one large, at least 500 MB).
- One Windows update or feature update.
- Optionally a Store app or Edge.

## 6. Test cases and success criteria
1. Device receives the DO cache host policy from Intune.
2. Win32 app install succeeds on a restricted device, served from the cache.
3. The first download populates the cache and the second device gets a cache hit.
4. Windows update download works via the cache.
5. Cache node unavailable: confirm fallback behaviour, and whether it fails cleanly when no internet is available.
6. Measure download time, WAN bandwidth saved and cache hit ratio (target e.g. >80% on repeat downloads).
7. Confirm that status reporting and policy sync work for the restricted devices.

Success: the target content is installed on restricted devices, with the cache serving the downloads and no manual intervention.

## 7. Open questions
- Is there a proxy or gateway path from the devices to Intune endpoints?
- Which content types are required (Win32, LOB, Store, updates)?
- Number of devices and sites; join type (Entra joined, hybrid, workgroup)?
- Cache host options and Azure subscription availability.
- Security approval for outbound access from the cache host.
- PoC timeline and success criteria.

## 8. Alternatives if MCC does not fit
- Configuration Manager distribution point, or cloud-attached ConfigMgr with an on-prem DP.
- Intune with a proxy that allow-lists only required Intune and CDN endpoints.
- Offline media or Autopilot with local sources for fully air-gapped devices.
- Delivery Optimization peer-to-peer in addition to the cache.

## 9. Risks
- MCC does not remove the need for Intune cloud connectivity.
- Limited support for custom and LOB content.
- Cache host sizing and maintenance overhead.
- WSL2 and virtualization requirements on the host.
- Product features change; verify against current Microsoft documentation before the PoC.

## 10. Suggested next steps
1. Answer the open questions and confirm the connectivity assumption.
2. Request the Azure subscription, host and firewall changes.
3. Start the PoC (Phases 2-3) with the test devices above.
