# SIEM Lab: Wazuh & Windows Endpoint Integration 

**Target Role:** Security Operations Center (SOC) Analyst / Security Engineer

**Skill Demonstrated:** SIEM Architecture, Endpoint Telemetry (Sysmon), Virtual Network Troubleshooting, and Secure Agent Authentication.

---

### 1. Evidence of Work

**What I Built:**
I deployed a localized Wazuh SIEM manager on Linux and securely connected a Windows 11 enterprise endpoint. To ensure high-quality security telemetry, I configured the Windows endpoint with Sysmon and the SwiftOnSecurity ruleset before deploying the Wazuh agent to forward those logs into the SIEM.

**My Thought Process & Troubleshooting:**
During deployment, the Windows agent failed to appear in the Wazuh dashboard. Instead of reinstalling, I systematically diagnosed the environment:
*   **Network Isolation Diagnosis:** I used `ping` and `ipconfig` to discover both VMs were isolated on VirtualBox's default NAT network, resulting in an IP conflict (`10.0.2.15`). I resolved this by bridging the machines with a Host-only adapter to establish a private virtual switch.
*   **Loopback Routing Fix:** I analyzed the agent's `ossec.conf` file and found it was mistakenly configured to send logs to the `127.0.0.1` loopback address. I manually re-routed this to the server's true Host-only IP.
*   **Cryptographic Authentication:** Because the initial connection failed, the agent lacked its authentication key. I used PowerShell to execute `agent-auth.exe`, forcing a secure handshake with the manager to download the key, and restarted the service to bring the agent online.

**What I Learned:**
This project reinforced the critical difference between host-to-host virtual networking and host-to-guest port forwarding. It also provided hands-on experience with how SIEM agents securely authenticate and route logs across a network.

**What I Would Do Differently:**
In the future, I will map out the virtual network topology and configure the Host-only network adapters *before* running the agent installation scripts. This will prevent authentication timeouts and ensure a seamless initial deployment.

---

### 2. Outcome
A fully operational Wazuh SIEM environment with an actively connected Windows 11 endpoint. The SIEM is successfully receiving baseline system logs and advanced Sysmon event data, ready for threat simulation and rule creation.

**Visual Proof:**
*(Add your screenshot of the Active Wazuh Dashboard here)*

*(Add your screenshot of the PowerShell terminal showing "Ellzy" hostname and the "Valid key received" message here)*
<h2>Program walk-through:</h2>

<p align="center">
Launch the utility: <br/>
Select the disk:  <br/>
<img https://imgur.com/K38F8bP"/>
<br />
<br />
Enter the number of passes: <br/>
<img src="https://i.imgur.com/nCIbXbg.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Confirm your selection:  <br/>
<img src="https://i.imgur.com/cdFHBiU.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Wait for process to complete (may take some time):  <br/>
<img src="https://i.imgur.com/JL945Ga.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Sanitization complete:  <br/>
<img src="https://i.imgur.com/K71yaM2.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Observe the wiped disk:  <br/>
<img src="https://i.imgur.com/AeZkvFQ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
