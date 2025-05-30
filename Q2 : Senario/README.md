## 1. Verify DNS Resolution
### (a) nslookup internal.example.com

What you're checking:

Does internal.example.com return a valid IP address?
Or does it fail (SERVFAIL, NXDOMAIN, timeout)?

### (b) Check DNS Resolution Using Google's Public DNS 8.8.8.8:
nslookup internal.example.com 8.8.8.8

What you're checking:

Does 8.8.8.8 know about your internal domain? (likely no, because it's internal)
Expected: failure or no answer.

### Conclusion:
If /etc/resolv.conf DNS fails, but 8.8.8.8 also fails, your local DNS might be misconfigured or down.

## 2. Diagnose Service Reachability

### (a) Find Resolved IP:
If the domain resolved, take the IP address (e.g., 192.168.1.50).
If not, use the IP you expect for internal.example.com.

### (b) Test Port 80/443 (HTTP/HTTPS):
Using curl:
curl -I http://internal.example.com
curl -I https://internal.example.com


## 3. Trace the Issue – List All Possible Causes

Here are all possible causes:


Category	     Possible Cause
DNS	           DNS server down
DNS	           Wrong /etc/resolv.conf entries
DNS	           Internal DNS zone corrupted
DNS	           Firewall blocking DNS queries (port 53 UDP/TCP)
Networking	   Server IP changed but DNS not updated
Networking	   Routing issues between client and server
Networking	   Firewall blocking HTTP/HTTPS ports (80, 443)
Web Service	   Web server (nginx/apache) not running
Web Service	   Web server running but misconfigured (wrong server_name)

## 4. Propose and Apply Fixes
Now for each cause, here’s how to:

Confirm it's the issue
Fix it with exact Linux commands

### 1. DNS Server Down
Confirm:
sudo systemctl restart named

### 2. Wrong /etc/resolv.conf Entries
confirm:
cat /etc/resolv.conf

### 3. Firewall Blocking DNS
confirm:
telnet <dns-server-ip> 53

### 4. Web Server Not Running
Confirm: SSH into dashboard server:
sudo systemctl status nginx
Fix:
sudo systemctl start nginx
sudo systemctl enable nginx

### 5. IP Address Mismatch
Confirm: Compare DNS-resolved IP vs server IP:
ip addr show
Fix:
Update DNS record (if you have access)
Or temporarily fix using /etc/hosts (bonus below)
