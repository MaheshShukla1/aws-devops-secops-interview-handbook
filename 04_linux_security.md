# 4) Linux & Security

**Q124. Finding why a server is slow—first commands?**

**Answer (Natural):**  
Check CPU (`top`), memory (`free -m`), IO (`iostat`), disk (`df -h`), network (`ss -tuna`), and logs. Form a quick hypothesis and drill down.


👉 **Example:** High iowait → investigate storage or noisy neighbor.


---

**Q125. File permissions rwx—explain 640?**

**Answer (Natural):**  
Owner read+write (6), group read (4), others none (0). Use least privilege.


👉 **Example:** `chmod 640 secrets.txt` limits exposure.


---

**Q126. setuid/setgid/sticky bit?**

**Answer (Natural):**  
setuid runs with file owner's privileges; setgid with group's; sticky bit on dirs prevents users from deleting others' files.


👉 **Example:** /tmp uses sticky bit (01777).


---

**Q127. SSH hardening basics?**

**Answer (Natural):**  
Disable root login/password auth, use keys, enable MFA where possible, restrict with `AllowUsers`, use `Fail2ban`, and rotate host keys.


👉 **Example:** Port knocking is optional; focus on keys and MFA.


---

**Q128. Systemd service troubleshooting?**

**Answer (Natural):**  
Use `systemctl status`, `journalctl -u`, check ExecStart, environment, and dependencies. Set Restart policies.


👉 **Example:** Add `Restart=on-failure` and `LimitNOFILE`.


---

**Q129. Disk full—what do you check?**

**Answer (Natural):**  
Largest directories (`du -sh *`), logs rotation, orphaned Docker layers, and inode exhaustion (`df -i`).


👉 **Example:** Rotate logs and prune Docker images.


---

**Q130. TLS vs mTLS?**

**Answer (Natural):**  
TLS authenticates server to client; mTLS authenticates both—great for service-to-service zero trust.


👉 **Example:** Service mesh enforces mTLS by default.


---

**Q131. Firewalls on Linux?**

**Answer (Natural):**  
Use `nftables`/`iptables` or `ufw` for policy. Favor deny-by-default and allow needed ports. Document rules.


👉 **Example:** Allow 22 from bastion SG only.


---
