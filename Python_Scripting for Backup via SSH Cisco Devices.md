
# Cisco Switch Backup Automation using Python and TFTP

This project automates the backup of Cisco SG300/SG350 series switches using Python and TFTP. It connects to multiple switches via SSH, sends the command to copy the startup configuration, and saves it to a local TFTP server.

---

## ✨ Features

- Automates configuration backup from multiple Cisco switches  
- Uses `paramiko` for SSH connection  
- Stores backup configuration files on a local TFTP server  
- Handles CLI prompts and confirmations during backup

---

## 🚀 Prerequisites

Before running this script, make sure:

- You have Python 3.x installed  
- The `paramiko` library is installed (`pip install paramiko`)  
- TFTP server is installed and running on your local PC (e.g., TFTPD32 or TFTPD64)  
- Your PC IP (TFTP server) is reachable from the switches  
- SSH is enabled and password authentication is allowed on the switches

---

## 🔧 How It Works (Step-by-Step)

### 1. **Import Required Modules**
```python
import paramiko
import time
```
- `paramiko` is used to create SSH connections.  
- `time` is used to manage delays between commands.

### 2. **Define the Switch List**
```python
switches = [
    {"ip": "192.168.102.250", "username": "root", "password": "WhqAcc*&^8u7y"},
    {"ip": "192.168.102.6", "username": "root", "password": "WhqAcc*&^8u7y"}
]
```
- Each switch is represented as a dictionary containing its IP, username, and password.  
- The script loops over this list to process each switch.

### 3. **Set TFTP Server and Backup Filename Format**
```python
tftp_server = "172.16.6.180"
backup_filename = "backup_{ip}.cfg"
```
- `tftp_server`: Your local PC's IP address where the TFTP service is running.  
- `backup_filename`: The format of the filename that will be saved. Each switch will have a unique name.

### 4. **Loop Over Each Switch**
```python
for switch in switches:
    # extract IP, username, password
```
- Extracts the details of each switch in every iteration to connect and send commands.

### 5. **Connect via SSH**
```python
ssh = paramiko.SSHClient()
ssh.set_missing_host_key_policy(paramiko.AutoAddPolicy())
ssh.connect(ip, username=username, password=password, look_for_keys=False)
```
- Initializes the SSH connection.  
- Accepts unknown host keys automatically.  
- Connects using provided credentials.

### 6. **Start Shell and Clear Initial Banner**
```python
shell = ssh.invoke_shell()
time.sleep(1)
shell.recv(1000)
```
- Opens an interactive shell session.  
- Waits a second for the CLI to be ready.  
- Clears any login messages.

### 7. **Send Backup Command to the Switch**
```python
command = f"copy startup-config tftp://{tftp_server}/{backup_filename.format(ip=ip)}\n"
shell.send(command)
time.sleep(2)
```
- Builds the backup command dynamically.  
- Sends it to the switch.  
- Waits for the switch to process.

### 8. **Handle CLI Prompts (e.g., Destination Filename)**
```python
output = shell.recv(5000).decode('utf-8')
if "?" in output or "Destination" in output:
    shell.send("\n")
    time.sleep(2)
    output += shell.recv(5000).decode('utf-8')
```
- Cisco switches often prompt for confirmation or filename.  
- The script sends `Enter` to accept the default.  
- Reads the output again.

### 9. **Close SSH Connection**
```python
ssh.close()
```
- Ends the SSH session after the backup command completes.

### 10. **Error Handling**
```python
except Exception as e:
    print(f"Failed to connect to {ip}: {e}")
```
- If SSH fails or any error occurs, it's caught and displayed.

---

## 🔒 Switch-side Requirements

1. SSH must be enabled:
```shell
conf t
ip ssh password-auth
exit
```

2. TFTP server must be reachable:
```shell
ping 172.16.6.180
```

3. Manual command (for testing):
```shell
copy startup-config tftp://172.16.6.180/backup_test.cfg
```

---

## 📁 Sample Output
```bash
Connecting to 192.168.102.250...
Backup completed for 192.168.102.250
Connecting to 192.168.102.6...
Backup completed for 192.168.102.6
```

---

## ✅ To Run This Script

1. Save as `main.py`  
2. Open terminal and run:
```bash
python main.py
```
3. Backup files will appear in the TFTP root directory

---

## ❤️ Author

**Niloy Saha**  

---

