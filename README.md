# Q2-Scenario

# Your internal web dashboard (hosted on `internal.example.com`) is suddenly unreachable from multiple systems. The service seems up, but users get “host not found” errors. You suspect a DNS or network misconfiguration. Your task is to troubleshoot, verify, and restore connectivity to the internal service.

# ✅ Step 1: Verify DNS Resolution

![WhatsApp Image](https://github.com/YoussefHatem2002/Q2-Scenario/blob/main/WhatsApp%20Image%202025-04-28%20at%2021.43.44_e8cef2d7.jpg?raw=true) 

                                 
![WhatsApp Image](https://github.com/YoussefHatem2002/Q2-Scenario/blob/main/WhatsApp%20Image%202025-04-28%20at%2021.44.00_d91cb5a1.jpg?raw=true)

# ✅ Step 2: Diagnose Service Reachability

![WhatsApp Image](https://github.com/YoussefHatem2002/Q2-Scenario/blob/main/WhatsApp%20Image%202025-04-28%20at%2022.13.06_30416e3f.jpg?raw=true)


# ✅ Step 3 & 4: List All Possible Causes and Apply Fixes


| Layer   | Potential Issue  | 1. How to Confirm  | 2. How to Fix (Commands) |
|:--------|:-----------------|:-------------------|:--------------------------|
| DNS | Wrong entries in `/etc/resolv.conf` | Check current DNS servers: <br>```cat /etc/resolv.conf```<br>Ping a known domain: <br>```ping google.com``` | Edit `/etc/resolv.conf` and set correct DNS, e.g.: <br>```sudo nano /etc/resolv.conf```<br>Add:<br>```nameserver 8.8.8.8``` |
| DNS | Internal DNS server down | Test DNS server: <br>```dig @<internal-DNS-IP> internal.example.com```<br>If no response, it's down. | Restart DNS server (if you control it): <br>```sudo systemctl restart named``` <br>or contact admin. |
| DNS | Internal DNS misconfigured | Use `dig` to check if zone exists: <br>```dig internal.example.com```<br>Look at "AUTHORITY" section for missing records. | Fix zone files on DNS server (requires admin): edit zone files and reload DNS.<br>Example:<br>```sudo nano /etc/bind/named.conf.local```<br>then:<br>```sudo systemctl reload bind9``` |
| Network | Firewall blocking DNS (port 53) | Test if port 53 is open:<br>```nc -zv <dns-server-ip> 53``` | Allow DNS traffic:<br>```sudo ufw allow out 53```<br>or<br>```sudo iptables -A OUTPUT -p udp --dport 53 -j ACCEPT``` |
| Network | Firewall blocking port 80/443 | Test if HTTP/HTTPS ports are open:<br>```nc -zv <server-ip> 80```<br>```nc -zv <server-ip> 443``` | Allow HTTP/HTTPS traffic:<br>```sudo ufw allow 80/tcp```<br>```sudo ufw allow 443/tcp``` |
| Network | Routing issues | Check route to server:<br>```traceroute <server-ip>```<br>or<br>```ip route get <server-ip>``` | Add correct route if missing:<br>```sudo ip route add <network>/<mask> via <gateway>``` |
| Service | Web server down | Test if port is open:<br>```nc -zv <server-ip> 80```<br>If connection refused, web service is down.<br>Or:<br>```curl http://<server-ip>``` | Restart web service:<br>```sudo systemctl restart apache2```<br>or<br>```sudo systemctl restart nginx``` |
| Service | Web server binding to wrong IP/interface | Check listening services:<br>```ss -tuln```<br>or<br>```netstat -tuln```<br>Look if it's only listening on 127.0.0.1 (localhost). | Configure web server to listen on correct interface:<br>Edit web server config:<br>```sudo nano /etc/nginx/sites-available/default```<br>or<br>```sudo nano /etc/apache2/ports.conf```<br>Change `listen 127.0.0.1:80` → `listen 0.0.0.0:80`.<br>Then restart server. |


# 🏆 Bonus
#  Configure a local /etc/hosts entry to bypass DNS for testing
![WhatsApp Image](https://github.com/YoussefHatem2002/Q2-Scenario/blob/main/WhatsApp%20Image%202025-04-28%20at%2023.00.46_1157a366.jpg?raw=true)
![WhatsApp Image](https://github.com/YoussefHatem2002/Q2-Scenario/blob/main/WhatsApp%20Image%202025-04-28%20at%2023.00.31_45806ba2.jpg?raw=true)
# Show how to persist DNS server settings using systemd-resolved or NetworkManager.

![WhatsApp Image](https://github.com/YoussefHatem2002/Q2-Scenario/blob/main/WhatsApp%20Image%202025-04-28%20at%2023.03.40_10194557.jpg?raw=true)
![WhatsApp Image](https://github.com/YoussefHatem2002/Q2-Scenario/blob/main/WhatsApp%20Image%202025-04-28%20at%2023.03.16_8aa265bb.jpg?raw=true)
![WhatsApp Image](https://github.com/YoussefHatem2002/Q2-Scenario/blob/main/WhatsApp%20Image%202025-04-28%20at%2023.04.46_0708ebf3.jpg?raw=true)
