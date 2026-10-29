# Windows DNS Troubleshooting Lab

A personal Windows 11 support lab completed in Oracle VirtualBox. This project demonstrates an evidence-based troubleshooting process for a controlled DNS configuration issue.

## Lab Environment

- **Host operating system:** Windows
- **Virtualization software:** Oracle VirtualBox
- **Guest operating system:** Windows 11
- **Virtual machine network mode:** NAT
- **Tools:** Command Prompt, Microsoft Edge, `ipconfig`, `ping`, and `nslookup`

## Scenario

**User symptom:** The user cannot browse websites by domain name, such as `google.com`.

A DNS fault was intentionally created by configuring an invalid IPv4 DNS server on the Windows virtual machine.

## Troubleshooting process

1. Created a clean Windows baseline and verified normal connectivity.
2. Confirmed external IP connectivity with **ping 8.8.8.8**.
3. Confirmed normal DNS resolution with `nslookup google.com`.
4. Changed the IPv4 DNS setting to an invalid test address.
5. Collected evidence using:
   ```bat
   ipconfig /all
   ping 8.8.8.8
   ping google.com
   nslookup google.com
   ```
6.Identified DNS configuration as the cause because IP connectivity worked while hostname resolution failed.
**Evidence collected during troubleshooting:**
- **DNS server before the fault:** 10.0.2.2
- **Invalid DNS server configured:** 192.0.2.99
- **IP connectivity during the fault:** It succeeded with the reply of
  Reply from 8.8.8.8; bytes=32 time=78ms TTL=128
  Reply from 8.8.8.8; bytes=32 time=33ms TTL=128
  Reply from 8.8.8.8; bytes=32 time=50ms TTL=128
  Reply from 8.8.8.8; bytes=32 time=35ms TTL=128
  Ping statistics for 8.8.8.8:
  Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
  Approximate round trip times in milli-seconds:
  Minimum = 33ms, Maximum = 78ms, Average = 49ms
  
- **DNS resolution during the fault:** Ping request could not find host google.com, please check the name and try again.
- **DNS configuration after repair:** 10.0.2.2.
7. **Restored DNS to automatic DHCP, re-enabled IPv6, and cleared the DNS cache:** ```bat
   **ipconfig /flushdns**
   ```
8. Validated the fix with `nslookup`, `ping google.com`, and a browser test.

## Diagnosis

The VM could reach `8.8.8.8`, showing that external IP connectivity remained available. However, it could not resolve `google.com`, and `nslookup` timed out against the manually configured invalid DNS server. This isolated the incident to DNS name resolution rather than a general internet or network-adapter failure.

## Resolution

The DNS server assignment was restored to Automatic (DHCP). DNS cache was cleared, and successful DNS resolution and website access were confirmed.

## Evidence

See the `screenshots` folder for baseline, failure, and validation screenshots.

## Documentation

- [DNS Troubleshooting Report (PDF)](DNS-Troubleshooting-Report.pdf)
- [DNS Troubleshooting Report (Word)](DNS-Troubleshooting-Report.docx)

## Skills demonstrated

- Windows 11 support and troubleshooting
- DNS and TCP/IP fundamentals
- Evidence-based root-cause analysis
- Command Prompt diagnostics
- VirtualBox virtual machine administration
- Technical documentation

## Note

This was a controlled personal lab environment. No production systems, user accounts, or organizational data were affected.
