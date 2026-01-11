Move the administrative SSH service off the default port to reduce automated scanning and allow safe honeypot deployment.
It reduces attack surface while maintaining key based authentication and monitoring set up earlier.

---

## IMPLEMENTATION
Do this after initial system hardening.

Edit SSH config
```
sudo nano /etc/ssh/sshd_config
```
Change Port 22 to a new port number, must be above 1024 or else root needed.

Update firewall:
```
sudo ufw allow 52222/tcp
sudo ufw reload
```

Restart SSH:
```
sudo systemctl restart ssh
```
Run to verify:
```
sudo ss -tlnp | grep ssh
```
If this doesn't work you may need to run:
```
sudo systemctl disable --now ssh.socket
```
This will disable ssh.socket, which hard codes port 22.

Test access:
```
ssh user@ip -p 52222
```

---
Changing the SSH port doesn’t meaningfully improve security on its own. It’s not a defensive control that stops a capable or targeted attacker, and it doesn’t replace proper measures like key-based authentication, disabling passwords, limiting users, or firewalling access. What it does do is reduce internet-wide noise. Port 22 is constantly hammered by automated scanners and brute-force bots, especially on exposed devices. Moving SSH to a high, non-default port dramatically cuts that background traffic, which results in cleaner logs, fewer pointless auth attempts, and less load on small systems like a Raspberry Pi. For our honeypot-focused setup, this is useful because it increases signal-to-noise ratio. Any SSH activity you now see is more likely to be intentional and therefore worth alerting on or investigating. In that sense, SSH port relocation is operational hygiene and observability improvement, not real security by itself.




