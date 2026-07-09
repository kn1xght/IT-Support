# Windows 11 VM Setup & Snapshots

## Scenario

A Windows 11 virtual machine (VM) was required as a controlled environment for future IT labs. The goal was to install a functional system in VMware Workstation Pro, manually configure the virtual hardware, set up a local account, create a baseline snapshot, and verify that the system could be restored after unwanted configuration changes.

## Environment

- **Guest operating system:** Windows 11 Enterprise Evaluation
- **Hypervisor:** VMware Workstation Pro 26H1
- **Installation media:** Windows 11 ISO
- **Host operating system:** Windows 10

## VM Configuration

- **Memory:** 16 GB
- **Processors:** 1 processor, 4 cores
- **Storage:** 100 GB virtual disk
- **Disk type:** NVMe
- **Network type:** NAT
- **Firmware:** UEFI with Secure Boot
- **Virtual TPM:** Enabled
- **Snapshot name:** `Fresh Windows 11 install`

## Skills Demonstrated

- Windows 11 VM setup
- VM hardware configuration
- Local Windows account setup
- VMware Tools installation
- Snapshot management

## Implementation

### 1. Created and configured the virtual machine

A Windows 11 virtual machine was created in VMware Workstation Pro using the Custom (advanced) configuration option. This allowed manual configuration of the virtual hardware, including memory, CPU cores, disk settings, and network type.

![Custom virtual machine configuration](screenshots/01-windows-11-vm-installation.png)

The VM was configured with UEFI firmware and Secure Boot enabled to support the Windows 11 installation requirements. UEFI is the modern replacement for legacy BIOS and is responsible for initializing the system hardware and starting the boot process. Secure Boot helps ensure that trusted boot components are used during startup.

![UEFI and Secure Boot configuration](screenshots/02-vm-boot-configuration.png)

In addition to UEFI and Secure Boot, Windows 11 also requires TPM 2.0. TPM (Trusted Platform Module) provides hardware-based security functions, including secure storage for cryptographic keys and support for features such as BitLocker.

![Virtual TPM and encryption configuration](screenshots/03-tpm-configuration.png)

### 2. Configured virtual hardware resources

The VM was assigned 16 GB of RAM to ensure smooth Windows 11 performance while leaving resources available for the host system.

![Memory configuration](screenshots/04-memory-configuration.png)

NAT (Network Address Translation) was selected so the virtual machine could access the internet through the host computer without appearing as a separate device on the physical network.

![NAT network configuration](screenshots/05-network-type-configuration.png)

### 3. Installed Windows 11

Disk 0 was selected as the installation target.

Since the disk was left unallocated, Windows Setup could create the required partitions automatically.

![Windows 11 disk selection](screenshots/06-disk-allocation.png)

### 4. Created a local Windows account

During the Windows 11 out-of-box experience, the setup process required a Microsoft account sign-in. Since this VM was intended for lab use, I used Command Prompt to launch the local account setup flow and created a local user account instead.

This kept the lab system separate from personal cloud services and made the VM easier to reset, snapshot, and reuse.

![Local account setup command](screenshots/07-local-user-account-configuration.png)

### 5. Installed VMware Tools

After Windows 11 was installed, I installed VMware Tools inside the guest operating system.

VMware Tools improves display scaling, mouse movement, clipboard behavior, and general integration between the host and guest systems.

![VMware Tools installation completed](screenshots/08-vmware-tools-installation.png)

### 6. Created a baseline snapshot

Once the VM was fully installed and operating correctly, I created a baseline snapshot named `Fresh Windows 11 install`.

This snapshot provides a known working state that can be restored before starting future labs or after making unwanted configuration changes.

![Baseline snapshot created](screenshots/09-snapshot-manager.png)

### 7. Tested snapshot restore

To verify the snapshot, I made a temporary change inside the VM by creating a test folder named `DELETE ME`.

![Testing snapshot functionality](screenshots/10-snapshot-test.jpg)

I then restored the VM back to the baseline snapshot. After restoration, I confirmed that the "DELETE ME" folder had been removed and that the VM returned to its clean working state.

![Successful snapshot restoration](screenshots/12-successful-snapshot-restoration.jpg)

## Result

The Windows 11 virtual machine was successfully deployed in VMware Workstation Pro and prepared for future IT labs. 

The VM was configured with Windows 11-compatible firmware, virtual TPM support, NAT networking, VMware Tools, and a clean baseline snapshot. Snapshot functionality was verified by restoring the VM after making a temporary change.

## Troubleshooting

### Windows 11 setup required a Microsoft account

- **Symptoms:** During Windows 11 setup, the installer required a Microsoft account sign-in.
- **Cause:** Windows 11 OOBE encourages or requires Microsoft account login during setup.
- **Resolution:** I opened Command Prompt during setup and ran:

```cmd
start ms-cxh:localonly
```

[← Return to Windows](../)