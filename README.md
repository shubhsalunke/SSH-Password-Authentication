# Enable SSH Password Authentication on Ubuntu/Azure VM

## Step 1: Connect to Your Server

```bash
ssh azureuser@YOUR_SERVER_IP
```

---

# Step 2: Set Password for User

```bash
sudo passwd azureuser
```

Example:

```text
New password:
Retype new password:
passwd: password updated successfully
```

---

# Step 3: Open SSH Configuration File

```bash
sudo nano /etc/ssh/sshd_config
```

---

# Step 4: Add or Update These Lines

Go to the bottom of the file and add:

```bash
PasswordAuthentication yes
PubkeyAuthentication yes
PermitRootLogin no
```

---

# Step 5: Save the File

Press:

```text
CTRL + O
```

Then press:

```text
Enter
```

Then exit:

```text
CTRL + X
```

---

# Step 6: Restart SSH Service

```bash
sudo systemctl restart ssh
```

---

# Step 7: Check SSH Service Status

```bash
sudo systemctl status ssh
```

Expected:

```text
active (running)
```

Exit status screen:

```text
Q
```

---

# Step 8: Verify Configuration

```bash
sudo grep -E "PasswordAuthentication|PermitRootLogin|PubkeyAuthentication" /etc/ssh/sshd_config
```

Expected Output:

```bash
PasswordAuthentication yes
PubkeyAuthentication yes
PermitRootLogin no
```

---

# Step 9: Allow SSH in Firewall (If UFW Enabled)

Check firewall:

```bash
sudo ufw status
```

Allow SSH:

```bash
sudo ufw allow 22/tcp
```

Reload firewall:

```bash
sudo ufw reload
```

---

# Step 10: Check SSH Port

```bash
sudo ss -tulpn | grep ssh
```

Expected:

```text
LISTEN 0 128 0.0.0.0:22
```

---

# Step 11: Test SSH Login from Local Machine

From Windows PowerShell / CMD:

```bash
ssh azureuser@YOUR_SERVER_IP
```

Example:

```bash
ssh azureuser@20.7.54.57
```

Enter password when prompted.

---

# Step 12: Troubleshooting

## If Connection Refused

Check SSH service:

```bash
sudo systemctl restart ssh
sudo systemctl status ssh
```

---

## If Timeout Happens

Check Azure NSG rules:

Allow:

| Port | Protocol | Source |
| ---- | -------- | ------ |
| 22   | TCP      | Any    |

---

## If Still Cannot Login

Check logs:

```bash
sudo journalctl -u ssh -n 50
```

or

```bash
sudo tail -f /var/log/auth.log
```

---

# Final Test

Successful login:

```bash
ssh azureuser@YOUR_SERVER_IP
```

Expected:

```text
azureuser@yourvm:~$
```
