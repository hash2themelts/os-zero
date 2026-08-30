# OS Zero - Security-First Bare-Metal Nanokernel

> **"FUTURE IS YOURS LEAVE A LEGACY"**

## Copyright & License

**Copyright © 2024-2026 hash2themelts**

This project is licensed under the **MIT License**. See [LICENSE](LICENSE) file for full terms.

### Quick Summary

✓ You can use, modify, and distribute this software  
✓ You must include the copyright notice and license  
✓ No warranty provided (use at your own risk)  
✓ Contribute back improvements to the community  

---

## What is OS Zero?

A minimal x86-64 nanokernel designed from the ground up with **cryptographic security** as the foundation.

### Key Features

- **Two-Tier Download Verification**
  - Tier 1: Fast pre-scan (< 1 second) - URL, domain, cache lookup
  - Tier 2: Deep scan (30-60 sec) - AI threat detection, signature verification

- **Cryptographic Terminal** (/850/)
  - Ed25519 signature verification
  - SHA-256 cryptographic hashing
  - Reproducible binary builds (prove no backdoor)
  - Immutable audit log with TPM

- **Network Gate** (/420/)
  - Kernel-level per-process network control
  - Auto-close network after download
  - No process can exfiltrate data

- **AI Scanner** (/410/)
  - 5-layer threat detection
  - Magic byte verification
  - Malware signatures
  - Hidden/obfuscated code detection
  - Zero-width character scanning

- **Fresh OS Every Boot**
  - Read-only kernel partition (immutable)
  - Pre-scan filesystem on each boot
  - Zero OS persistence (malware dies on reboot)

- **Open-Source Only Policy**
  - Closed-source apps auto-deleted
  - All dependencies verified open-source
  - Whitelist of known safe software

---

## Project Structure

```
os-zero/
├── LICENSE                          # MIT License
├── CONTRIBUTING.md                  # How to contribute
├── boot/
│   └── boot.asm                     # x86-64 bootloader (Stage 1)
├── kernel/
│   ├── kernel.asm                   # Kernel entry point
│   ├── Cargo.toml                   # Rust dependencies
│   └── src/
│       ├── main.rs                  # Kernel entry
│       ├── lib.rs                   # Kernel library docs
│       ├── crypto/
│       │   ├── mod.rs               # Crypto module
│       │   ├── ed25519.rs           # Ed25519 signatures
│       │   ├── sha256.rs            # SHA-256 hashing
│       │   └── publisher_keys.rs    # Hardcoded trusted keys
│       ├── terminal.rs              # Secure terminal (/850/)
│       ├── scanner.rs               # AI Scanner (/410/)
│       ├── checker.rs               # Runtime Checker (/420/)
│       ├── network_gate.rs          # Network Gate (/420/)
│       ├── audit_log.rs             # Immutable audit log
│       ├── boot.rs                  # Boot verification
│       ├── memory.rs                # Memory manager
│       ├── interrupts.rs            # Interrupt handlers
│       ├── syscall.rs               # Syscall dispatcher
│       ├── process.rs               # Process management
│       ├── fs.rs                    # Filesystem
│       └── network.rs               # Network stack
├── docs/
│   ├── SECURE_TERMINAL.md           # Terminal architecture
│   ├── ARCHITECTURE.md              # System design
│   └── SECURITY_MODEL.md            # Security guarantees
└── README.md                        # This file
```

---

## Build & Run

### Prerequisites

```bash
nasm          # Netwide Assembler
cargo         # Rust package manager
qemu-system-x86_64  # Emulator
```

### Compile

```bash
# Build bootloader
nasm -f bin boot/boot.asm -o boot.bin

# Build kernel
cd kernel
cargo build --target x86_64-unknown-linux-gnu --release
cd ..

# Create disk image
dd if=/dev/zero of=os_zero.img bs=1M count=32
dd if=boot.bin of=os_zero.img bs=512 count=1 conv=notrunc
dd if=kernel/target/x86_64-unknown-linux-gnu/release/kernel of=os_zero.img bs=512 seek=1 conv=notrunc
```

### Run in QEMU

```bash
qemu-system-x86_64 -drive format=raw,file=os_zero.img -m 512M
```

### What You'll See

```
╔═══════════════════════════════════════╗
║  OS ZERO - NANOKERNEL BOOT            ║
║  FUTURE IS YOURS LEAVE A LEGACY       ║
║  Copyright (c) 2024 hash2themelts     ║
║  Licensed under MIT License           ║
╚═══════════════════════════════════════╝

[KERNEL] Memory manager initialized
[KERNEL] Interrupt handlers loaded
[KERNEL] Filesystem driver initialized
[KERNEL] Network stack initialized
[/410/] AI Scanner initialized
[/420/] Runtime Checker initialized
[/420/] Network Gate initialized
[AUDIT] Immutable audit log initialized
[CRYPTO] Ed25519 verification engine initialized

[BOOT] Starting verification cascade...
[BOOT] ✓ Bootloader verified
[BOOT] ✓ Kernel verified (read-only)
[BOOT] ✓ Pre-scan passed: 0 changes detected
[BOOT] ✓ No app violations found

╔═══════════════════════════════════════╗
║  OS ZERO - READY                      ║
║  All systems verified and secure      ║
╚═══════════════════════════════════════╝

SECURE SHELL (/850/)
Cryptographically verified terminal
$ _
```

---

## Usage Examples

### Download with Two-Tier Verification

```bash
$ wget https://github.com/rust-lang/rust/releases/download/1.75.0/rustc.tar.gz

[/850/] TIER 1: Fast Pre-scan (URL + Domain + Cache Lookup)
  ✓ Protocol: HTTPS
  ✓ Domain whitelisted: github.com
  ✓ DNS verified
  ✓ File in database (cached)
  
[/850/] ✓ Pre-scan PASSED - File known and safe
[/420/] Network Gate: OPENING (60 seconds)
[Downloading...] 156.2 MB
[/420/] Network Gate: CLOSING
✓ Download complete
```

### Download Unknown File (Deep Scan)

```bash
$ wget https://github.com/user/project/releases/download/v1.0/app.tar.gz

[/850/] TIER 1: Fast Pre-scan
  ⚠ File not in cache - will need deep scan
  
[/420/] Network Gate: OPENING (90 seconds)
[Downloading...] 45.3 MB
[/420/] Network Gate: CLOSING

[/850/] TIER 2: Deep Scan (AI + Signature + Binary Verification)
  ✓ File hash verified
  ✓ AI Scanner: No threats detected
  ✓ Publisher signature verified: [Publisher Name]
  ✓ Binary matches source code (reproducible build verified)
  
[/850/] ✓ Deep scan PASSED
File saved to ./app.tar.gz
```

---

## Security Model

### Chain of Trust

```
Firmware/BIOS
  ↓ (verifies bootloader hash)
Bootloader (/410/)
  ↓ (verifies kernel signature)
Kernel (/420/)
  ↓ (verifies drivers & apps)
Trusted Apps / Sandboxed Apps
```

### Data Protection

| Component | Backup | Persistence | Scan on Boot |
|-----------|--------|-------------|---|
| **OS Core** | Golden copy only | ✗ None (RAM) | ✓ Verify integrity |
| **Apps** | ✗ None | ✓ /opt/ | ✓ Verify open-source |
| **User Data** | ✗ None | ✓ /home/ | ✓ Detect changes |

### Threat Model

✓ Protected against:
- Man-in-the-middle attacks (HTTPS + signature verification)
- Malware downloads (Tier 1 + Tier 2 scanning)
- Dependency chain attacks (reproducible builds)
- Ransomware (fresh OS every boot)
- User error (command whitelist, network gate)
- Closed-source backdoors (open-source only)

✗ Not designed for:
- Network-based attacks at scale
- Supply chain attacks at kernel compile time
- Physical attacks (DMA, cold boot, etc)

---

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

### To Contribute:

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests and documentation
5. Submit a pull request

All contributions must:
- Be licensed under MIT
- Include copyright notice
- Have clear commit messages
- Pass security review

---

## Citation

If you use OS Zero in research or academic work:

```bibtex
@software{os_zero_2024,
  author = {hash2themelts},
  title = {OS Zero: Security-First Bare-Metal Nanokernel},
  year = {2024},
  url = {https://github.com/hash2themelts/os-zero},
  license = {MIT}
}
```

---

## Disclaimer

**OS Zero is provided "as-is" without warranty.**

This is experimental software. Use at your own risk.

- Not recommended for production systems
- Security vulnerabilities may exist
- Incomplete implementation (in development)
- Educational purposes only

---

## Security Vulnerabilities

Found a security issue? **Please report responsibly:**

1. **Do NOT** open a public issue
2. Email: `security@[your-domain]` (once set up)
3. Include: Description, impact, proof-of-concept
4. Allow 30 days for response before disclosure

---

## Roadmap

- [x] Bootloader (x86-64)
- [x] Nanokernel (Rust)
- [x] Cryptographic terminal (/850/)
- [x] Two-tier download verification
- [x] AI Scanner (/410/)
- [x] Network Gate (/420/)
- [x] Immutable audit log
- [ ] Full reproducible build system
- [ ] TPM integration
- [ ] Multi-user support
- [ ] Container runtime
- [ ] Package manager

---

## Resources

- **Docs**: [docs/](docs/)
- **Source**: [kernel/src/](kernel/src/)
- **License**: [LICENSE](LICENSE)
- **GitHub**: https://github.com/hash2themelts/os-zero

---

## Acknowledgments

- Rust project for excellent systems programming language
- Ed25519 cryptography by DJB
- Linux kernel community for inspiration
- Open-source contributors worldwide

---

**FUTURE IS YOURS LEAVE A LEGACY**

*OS Zero - Where security meets simplicity*
