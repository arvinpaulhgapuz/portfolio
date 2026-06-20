## Findings & Recommendations

A review of the lab setup and supporting documentation surfaced a mix of genuine technical gaps and documentation gaps. These are recorded here as lessons learned from building and running the honeypot.

### Findings

1. **Network isolation was implemented but undocumented**
   The honeypot was deployed on a dedicated VMware/VirtualBox host-only adapter, isolating it from personal devices and the rest of the home network. This isolation step was confirmed as part of the actual build but was missing from the Installation Guide, which only documents switching the VM adapter to Bridged for the Cowrie/iptables steps. The guide and the real setup describe two different stages of the same project, which made the deployment look less isolated than it actually was.

2. **Single-rule iptables redirect, not persisted**
   SSH traffic on port 22 is redirected to Cowrie on port 2222 via a single `iptables` PREROUTING rule. The rule is not saved (e.g., via `iptables-save` / `netfilter-persistent`), so it would not survive a reboot, and no outbound traffic restrictions were documented for the honeypot VM.

3. **Short collection window**
   The honeypot was left running for only a few hours before logs were analyzed. This is enough to capture automated scanning and credential-stuffing traffic, but a longer collection window would surface more variety in source IPs, timing patterns, and any slower, manually-driven attack behavior.

4. **No centralized logging or alerting**
   Logs were reviewed locally with `tail -f` and replayed with `playlog`. There is no log forwarding to a SIEM and no automated alerting — detection depended on manually checking the terminal or log files during the live window.

5. **Default Cowrie configuration**
   The honeypot runs on the stock `cowrie.cfg.dist` configuration without customizing the fake hostname, filesystem, or banners. Default configurations are sometimes fingerprinted by automated scanners as known honeypots, which can reduce the realism of captured attacker behavior.

6. **Limited attack surface coverage**
   Only SSH (port 22/2222) is monitored. Other commonly targeted services such as HTTP and FTP are not covered, so the lab demonstrates SSH brute-force and credential-harvesting detection specifically, rather than broad-spectrum exposure.

### Recommendations

- Update the Installation Guide to include the host-only adapter / network isolation step, so the documentation accurately reflects the full build rather than only the Cowrie installation steps.
- Persist firewall rules across reboots and add egress filtering to restrict outbound traffic from the honeypot VM.
- Extend future runs to a longer collection window (days rather than hours) to capture a broader sample of attacker behavior.
- Forward logs to a SIEM (e.g., Splunk) for centralized monitoring and alerting instead of manual log review.
- Customize the Cowrie configuration (hostname, fake filesystem, banners) to reduce fingerprinting risk and improve realism.
- Expand monitored services beyond SSH (e.g., via a multi-honeypot stack like T-Pot) to capture a wider range of attacker behavior.

These changes would move the lab from a solid, isolated proof-of-concept toward a deployment that more closely reflects production honeypot practices — and would bring the documentation in line with what was actually built.

(Back To [Main](\README.md))
