## Findings & Recommendations

A review of the lab setup against security best practices surfaced several gaps between a functional honeypot and a properly hardened deployment. These are documented here as lessons learned, not just shortcomings of the original build.

### Findings

1. **Bridged networking without segmentation**
   The VM network adapter was switched from NAT to Bridged so the honeypot could be reached directly on the LAN. Bridged mode provides connectivity, not isolation — without a VLAN, host-only adapter, or firewall ACL, the honeypot VM and other LAN devices can reach each other.

2. **No documented network isolation layer**
   The lab environment did not implement VLAN segmentation, a dedicated virtual switch, or ACLs to block lateral movement between the honeypot and personal devices.

3. **Single-rule iptables redirect, not persisted**
   SSH traffic on port 22 is redirected to Cowrie on port 2222 via a single `iptables` PREROUTING rule. The rule is not saved (e.g., via `iptables-save` / `netfilter-persistent`), so it would not survive a reboot, and no outbound traffic restrictions were configured.

4. **No centralized logging or alerting**
   Logs are reviewed locally with `tail -f` and replayed with `playlog`. There is no log forwarding to a SIEM and no automated alerting — detection depends on manually checking the terminal or log files.

5. **Default Cowrie configuration**
   The honeypot runs on the stock `cowrie.cfg.dist` configuration without customizing the fake hostname, filesystem, or banners. Default configurations are sometimes fingerprinted by automated scanners as known honeypots, which can reduce the realism of captured attacker behavior.

6. **Limited attack surface coverage**
   Only SSH (port 22/2222) is monitored. Other commonly targeted services such as HTTP and FTP are not covered, so the lab demonstrates SSH brute-force and credential-harvesting detection specifically, rather than broad-spectrum exposure.

### Recommendations

- Implement VLAN segmentation or a dedicated virtual switch with no route to personal devices, rather than relying on bridged mode alone.
- Persist firewall rules across reboots and add egress filtering to restrict outbound traffic from the honeypot VM.
- Forward logs to a SIEM (e.g., Splunk) for centralized monitoring and alerting instead of manual log review.
- Customize the Cowrie configuration (hostname, fake filesystem, banners) to reduce fingerprinting risk and improve realism.
- Expand monitored services beyond SSH (e.g., via a multi-honeypot stack like T-Pot) to capture a wider range of attacker behavior.

These changes would move the lab from a functional proof-of-concept toward a deployment that more closely reflects production honeypot practices.
