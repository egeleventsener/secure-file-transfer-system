# 🛡️ Secure File Transfer System

A secure client-server file management system written in **C for Linux**.  
This project utilizes **SSH-based encrypted communication**, **multithreaded server architecture**, and **low-level socket programming**.

---

# 🚀 Key Features

- **Secure Communication**  
  Encrypted data transfer using **libssh** and **libssh2**.

- **Multithreaded Server**  
  Handles multiple concurrent clients using **POSIX threads**.

- **Jail Path Security**  
  Restricts clients to a specific base directory to prevent unauthorized system access.

- **Recursive Operations**  
  Supports recursive directory deletion and directory management.

- **Cross-Platform Ready**  
  Verified communication between **Windows (Host)** and **Ubuntu (VM)**.

---

# 📂 Project Structure

```
.
├── client.c              # Client-side implementation (libssh2)
├── server.c              # Multithreaded server logic (libssh)
├── delete_directory.c    # Recursive deletion implementation
├── delete_directory.h    # Header file for file utilities
├── destination/          # Default file storage / Jail directory
├── keys/                 # SSH Host Keys (RSA & ECDSA)
└── README.md             # Project documentation
```

---

# ⚙️ Build & Setup

## 1. Install Dependencies

```bash
sudo apt update
sudo apt install build-essential libssh-dev libssh2-1-dev
```

---

## 2. Prepare SSH Keys

```bash
mkdir -p keys
ssh-keygen -t rsa -b 3072 -f keys/ssh_host_rsa_key -N ""
ssh-keygen -t ecdsa -b 256 -m PEM -f keys/ssh_host_ecdsa_key -N ""
chmod 600 keys/ssh_host_rsa_key keys/ssh_host_ecdsa_key
```

---

## 3. Compilation

### Server

```bash
gcc server.c delete_directory.c -o server -lssh -lpthread
```

### Client

```bash
gcc client.c -o client -lssh2
```

---

# 🕹️ Command Reference

## Remote Operations (Server-side)

- `spwd` / `pwd` → Show remote working directory  
- `sls` / `ls` → List remote directory contents  
- `scd` / `cd <dir>` → Change remote directory  
- `smkdir` / `mkdir <dir>` → Create remote directory  
- `srm` / `rm <path>` → Delete remote file or directory (recursive)  
- `srename <old> <new>` → Rename remote file or directory  
- `send_file <local> [remote]` → Upload a file to the server  

---

## Local Operations (Client-side)

- `lpwd` → Show local working directory  
- `lls` → List local directory contents  
- `lcd <dir>` → Change local directory  
- `lmkdir <dir>` → Create local directory  
- `lrm <path>` → Delete local file  
- `lrename <old> <new>` → Rename local file or directory  

---

# 🔒 Security Architecture

- **Jail Path Restriction**  
  The server restricts all operations to the `destination/` directory.  
  Path traversal attempts such as `../../` are intercepted.

- **SSH Encryption Layer**  
  All commands are transmitted through an SSH session rather than plain TCP communication.  
  This prevents sensitive data from being exposed to packet sniffing tools such as **Wireshark**.

---

# 👤 Author

**Ege Levent Şener**  
Computer Engineering Student  
TED University