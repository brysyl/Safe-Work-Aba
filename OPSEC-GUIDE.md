## This guide outlines the baseline Operational Security (OPSEC) protocols for software engineers operating in high-risk or emerging tech environments. The goal is to move from "Password-Based" security to a Zero-Trust, Hardware-Bound identity.

1. Hardware-Bound Identity (FIDO2/WebAuthn)

The primary attack vector in modern remote work is Identity Theft via Session Hijacking or SIM-Swapping. ### Implementation:
 * Disable SMS/TOTP: Move away from mobile-app-based codes (Authenticator) and SMS. These are susceptible to device compromise and port-out attacks.
 * Enroll YubiKey: Register a FIDO2-compliant hardware key (like the YubiKey 5C) on GitHub, GitLab, and AWS.
 * Enforce Pin-Protection: Always set a local PIN on your FIDO2 key. This ensures that even if the physical key is stolen, it cannot be used without the knowledge-based factor.

2. Secure Git Operations (Commit Signing)
An engineer’s reputation is tied to their code. Unsigned commits can be spoofed.
Setup (PGP on Hardware):

Generate your GPG keys directly on the YubiKey's OpenPGP slot. This ensures the private key never leaves the hardware.
# Verify YubiKey status
gpg --card-status

# Generate key on card
gpg --card-edit
admin
generate

## Note: Export only your Public Key to GitHub. Your Private Key remains air-gapped within the hardware.

3. Network Isolation & Tunneling
In environments with shared or ISP-level monitored internet, the network itself is untrusted.
Protocol: Hardware VPN Kill-Switch
 * Hardware Tunneling: Use a travel router (e.g., GL-iNet) configured with a WireGuard client.
 * Local Isolation: Ensure the router is the only device connected to the ISP. Your workstation connects to the router’s private LAN.
 * DNS Leak Protection: Force all DNS queries through a secure resolver (like Cloudflare 1.1.1.1 or Quad9) via encrypted tunnels (DoH/DoT).

4. Data at Rest (Physical Layer Encryption)
Cloud storage is convenient but insufficient for high-stakes source code or client API keys.
Cold Storage Workflow:
 * Keypad Authentication: Use a drive with a physical keypad (Aegis Padlock). This prevents "Software-Level" keyloggers from capturing your encryption password.
 * Encryption Standard: Ensure the drive uses AES-XTS 256-bit hardware encryption.
 * Air-Gapping: Only connect the drive when actively syncing local repositories. Never leave it plugged in while browsing the open web.

5. Emerging Market Resilience (The "Aba" Standard)
Working in regions with power and internet instability requires additional OPSEC considerations:
 * Encrypted Offline Documentation: Keep a local, encrypted copy of your security protocols and emergency recovery codes (RECOVERY-KEYS.txt) on your hardware-encrypted drive.
 * Graceful Shutdowns: Ensure your hardware encryption mounts are unmounted before a total power loss to prevent partition corruption.
 * Dual-Key Redundancy: Always have a "Primary" and a "Backup" hardware key. In isolated hubs, the lead time for hardware replacement is high; redundancy is security.
