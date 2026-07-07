# Windows 11 VM Setup & Snapshots

## Scenario

A Windows 11 virtual machine was required as a controlled environment for future enterprise IT labs. The goal was to deploy a functional Windows 11 system in VMware Workstation Pro, configure the virtual hardware, create a clean baseline, and verify that snapshots could restore the machine after unwanted configuration changes.

## Environment

- **Guest operating system:** Windows 11 Enterprise Evaluation
- **Hypervisor:** VMware Workstation Pro 26H1
- **Network configuration:** NAT
- **Installation media:** Windows 11 ISO
- **Host operating system:** Windows 10
- **Firmware:** UEFI with Secure Boot
- **Virtual TPM:** Enabled through VM encryption

## Skills Demonstrated

- Virtual machine setup
- Windows 11 installation
- Virtual hardware configuration
- UEFI, Secure Boot, and TPM configuration
- Local Windows account setup
- VMware Tools installation
- Snapshot creation and restoration
- System verification

## Implementation

### 1. Created and configured the virtual machine

I created a new Windows 11 virtual machine in VMware Workstation Pro and selected the custom configuration option. This allowed me to manually configure the virtual hardware, firmware, storage, and network settings instead of relying on the default setup.

![Custom virtual machine configuration](images/01-windows-11-vm-installation.png)

The VM was configured with UEFI firmware and Secure Boot enabled to meet Windows 11 requirements.

![UEFI and Secure Boot configuration](images/02-uefi-secure-boot.png)

Because Windows 11 requires TPM support, I configured virtual machine encryption so VMware could enable a virtual TPM device.

![Virtual TPM and encryption configuration](images/03-virtual-tpm-encryption.png)

### 2. Configured virtual hardware resources

The VM was assigned enough resources to run Windows 11 smoothly while still leaving resources available for the host system.

The system was configured with 16 GB of RAM for better performance during installation and later lab work.

![Memory configuration](images/04-memory-configuration.png)

NAT networking was selected so the virtual machine could access the internet through the host computer without being directly exposed to the physical network.

![NAT network configuration](images/05-network-nat.png)

### 3. Installed Windows 11

I started the VM and booted from the Windows 11 ISO. During installation, Disk 0 was selected as the installation target.

The disk was left as unallocated space so Windows Setup could automatically create the required system, recovery, and primary partitions.

![Windows 11 disk selection](images/06-disk-selection.png)

### 4. Created a local Windows account

During the Windows 11 out-of-box experience, the setup process required a Microsoft account sign-in. Since this VM was intended for lab use, I used Command Prompt to launch the local account setup flow and created a local user account instead.

This kept the lab system separate from personal cloud services and made the VM easier to reset, snapshot, and reuse.

![Local account setup command](images/07-local-account-command.png)

### 5. Installed VMware Tools

After Windows 11 was installed, I installed VMware Tools inside the guest operating system.

VMware Tools improves display scaling, mouse movement, clipboard behavior, and general integration between the host and guest systems.

![VMware Tools installation completed](images/08-vmware-tools-installed.png)

### 6. Created a baseline snapshot

Once the VM was fully installed and operating correctly, I created a baseline snapshot named `Fresh Windows 11 install`.

This snapshot provides a known working state that can be restored before starting future labs or after making unwanted configuration changes.

![Baseline snapshot created](images/09-baseline-snapshot.png)

### 7. Tested snapshot restoration

To verify the snapshot, I made a temporary change inside the VM by creating a test folder named `DELETE ME`.

I then restored the VM back to the baseline snapshot. After restoration, I confirmed that the temporary change had been removed and that the VM returned to its clean working state.

![Virtual machine restored to baseline snapshot](images/10-snapshot-restored.png)

## Configuration Notes

- **Memory:** 16 GB
- **Processors:** 1 processor, 4 cores
- **Storage:** 64 GB virtual disk
- **Disk type:** NVMe
- **Network mode:** NAT
- **Firmware:** UEFI with Secure Boot
- **Virtual TPM:** Enabled
- **Snapshot name:** `Fresh Windows 11 install`

These settings were selected to provide a smooth Windows 11 lab environment while keeping the VM isolated and reusable.

## Result

The Windows 11 virtual machine was successfully deployed in VMware Workstation Pro and prepared for future IT support and cybersecurity labs.

The VM was configured with Windows 11-compatible firmware, virtual TPM support, NAT networking, VMware Tools, and a clean baseline snapshot. Snapshot functionality was verified by restoring the VM after making a temporary change.

## Troubleshooting

### Windows 11 setup required a Microsoft account

- **Symptoms:** During Windows 11 setup, the installer required a Microsoft account sign-in.
- **Cause:** Windows 11 OOBE encourages or requires Microsoft account login during setup.
- **Resolution:** I opened Command Prompt during setup and ran:

```cmd
start ms-cxh:localonly

[← Back to Windows Labs](../README.md)