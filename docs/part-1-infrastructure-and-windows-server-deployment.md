# Part 1 - Infrastructure & Windows Server 2022 Deployment
## Overview

The goal of this project is to gain hands-on experience with:
- Windows Server administration
- Virtualization using Unraid

## Lab Objectives
- Deploy Windows Server 2022 as a VM on Unraid

## Environment
- Host OS: Unraid
- Hypervisor: KVM (via Unraid)
- Guest OS: Windows Server 2022

## Installation
1. Uploaded the Windows Server 2022 ISO to the Unraid ISO share.
2. Created a new Windows VM using Unraid VM Manager.
3. Attached the VirtIO driver ISO for storage and network support.
4. Booted the VM and initiated the Windows installation.
5. Loaded VirtIO storage drivers during setup to detect the virtual disk.
6. Completed the Windows Server installation process.
7. Installed VirtIO drivers post-installation for optimal performance.

## Post-Installation Configuration
After installation, the server was configured with baseline settings to prepare it for domain services.

**Post-Installation Tasks:**
- Logged in using the local Administrator account
- Applied Windows updates
- Configured system time and time zone
- Renamed the server following naming conventions (pre-domain)
- Verified network connectivity and IP configuration
- Disabled unnecessary default features where appropriate

## Results
System validation was performed to ensure stability and readiness for Active Directory deployment.

**Validation Checks:**
- Verified successful boot without errors
- Confirmed network connectivity to the local network
- Ensured adequate system performance and resource utilization
- Verified administrative access and system responsiveness

## Conclusion
This phase successfully established the foundational infrastructure required for the next step for our Active Directory environment.
