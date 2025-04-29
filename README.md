# 🧠 Python Scripting for Cisco Switch Backup via SSH

![Project Screenshot](https://github.com/niloy-it/Python_Scritping/blob/main/python.jpg)

## 🔧 Project Objective

This project automates the backup of **Cisco SG300/SG350 switch configurations** using **SSH over Python (Paramiko)**. It connects to multiple switches, executes the `copy startup-config` command, and saves the configuration files to a **TFTP server** with filenames based on each switch's IP address.

---

## 🚀 Key Features

- ✅ Connects to multiple Cisco switches using `SSH`
- 📤 Sends backup commands via CLI: `copy startup-config tftp://<server>/<file>`
- 📁 Saves files locally on TFTP server with structured filenames like `backup_192.168.102.250.cfg`
- ⏳ Handles interactive prompts (e.g., confirmation, destination filenames)
- 🔐 Authenticates securely with username and password
- 🖥️ Designed for **network administrators** managing Cisco switches

---

## 📂 Project Structure

```bash
.
├── backup_script.py          # Main Python script
├── README.md                 # Project documentation
└── python.jpg                # Screenshot for visual reference
