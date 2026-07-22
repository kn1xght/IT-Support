# Windows Server Deployment & Baseline Configuration

## Scenario

A Windows Server virtual machine and lab network were required to establish the server infrastructure for future Active Directory labs. The goal was to install Windows Server 2025, configure a stable baseline, verify network and remote connectivity, install updates, and create a rollback snapshot before installing Active Directory Domain Services.

## Environment

- **Guest operating system:** Windows Server 2025 Standard Evaluation (Desktop Experience)
- **Host operating system:** Windows 10
- **Administrator account:** `Stanic`
- **Computer name:** `DC01`
- **Hypervisor:** VMware Workstation Pro 26H1
- **Installation media:** Windows Server 2025 ISO

## VM Configuration

- **Memory:** 6 GB
- **Processor:** 1 processor, 2 cores
- **Storage:** 80 GB SCSI virtual disk
- **Firmware:** UEFI with Secure Boot
- **Network:** VMware NAT using `VMnet8`
- **Lab subnet:** `192.168.50.0/24`
- **IPv4 address:** `192.168.50.10`
- **Default gateway:** `192.168.50.2`
- **Temporary DNS server:** `1.1.1.1`

## Skills Demonstrated

- VMware virtual machine creation and hardware configuration
- Windows Server installation and baseline configuration
- VMware virtual network configuration
- Static IPv4 addressing
- Network connectivity testing with PowerShell
- Windows Server update management
- Remote Desktop configuration and verification
- Virtual machine snapshot management
- Basic network troubleshooting

## Implementation

### 1. Created the Windows Server virtual machine

Created a custom VMware virtual machine for Windows Server 2025 using UEFI firmware with Secure Boot. The VM was assigned 6 GB of memory, two processor cores, an 80 GB SCSI virtual disk, and a NAT network adapter.

The installation ISO was attached to the virtual CD/DVD drive after the VM hardware had been configured.

![Windows Server virtual machine hardware configuration](images/01-vm-hardware-configuration.png)

### 2. Installed Windows Server 2025

Installed **Windows Server 2025 Standard Evaluation (Desktop Experience)** on the unallocated virtual disk.

The Desktop Experience edition provided the graphical interface and Server Manager required for the planned administration labs. After installation, the Windows version and initial Local Server configuration were verified.

![Windows Server 2025 installation verification](images/02-windows-server-installation.png)

### 3. Configured the lab network

Configured `VMnet8` as the NAT network for the Windows Server and Active Directory environment.

The subnet was changed to `192.168.50.0/24`, while the VMware NAT service was retained to provide internet access through the host computer. VMware DHCP was disabled because Windows Server will later take responsibility for assigning network settings during the DHCP lab.

![VMware lab network configuration](images/03-dedicated-lab-network.png)

This configuration separates the two network functions:

- VMware NAT provides the route to external networks.
- Windows Server DHCP will later assign IP addresses and network settings to lab clients.

### 4. Configured the server baseline

Installed VMware Tools to provide the drivers and integration features required for stable virtual machine operation.

Renamed the server to `DC01` before installing Active Directory Domain Services. This avoids having to rename the server after it becomes a domain controller.

The time zone was changed to:

```text
(UTC+01:00) Amsterdam, Berlin, Bern, Rome, Stockholm, Vienna
```

A static IPv4 configuration was then assigned:

```text
IP address:       192.168.50.10
Subnet mask:      255.255.255.0
Default gateway:  192.168.50.2
Preferred DNS:    1.1.1.1
```

The static address ensures that clients can consistently locate `DC01` after DNS and Active Directory are installed. The external DNS address is temporary and will be replaced with the server’s own address when `DC01` becomes a DNS server.

The hostname, time zone, IP address, default gateway, and DNS configuration were verified with PowerShell.

```powershell
hostname
Get-TimeZone
Get-NetIPConfiguration -InterfaceAlias "Ethernet0"
```

![Server baseline configuration](images/04-server-baseline-configuration.png)

Outbound HTTPS connectivity was tested with:

```powershell
Test-NetConnection www.microsoft.com -Port 443
```

The result returned `TcpTestSucceeded : True`, confirming that name resolution, routing, and outbound TCP connectivity were working.

![Network connectivity verification](images/05-network-connectivity-verification.png)

### 5. Installed Windows updates

Checked for and installed the available Windows Server security and cumulative updates. The server was restarted where required and checked again until Windows Update reported that the system was up to date.

![Windows Update verification](images/06-windows-update-verification.png)

### 6. Enabled and verified Remote Desktop

Enabled Remote Desktop through Server Manager while keeping Network Level Authentication enabled.

Connected to `DC01` from the physical host using Remote Desktop Connection and the server’s static IP address:

```text
192.168.50.10
```

The active sessions were checked with:

```powershell
query session
```

The current `rdp-tcp` session was listed as active for the local `Administrator` account, confirming that the server could be administered remotely.

![Remote Desktop verification](images/07-remote-desktop-verification.png)

### 7. Created the baseline snapshot

Shut down the server and disconnected the Windows Server installation ISO from automatic startup.

Created the following snapshot:

```text
DC01 Baseline - Pre-AD DS
```

The snapshot description recorded the completed state:

```text
Windows Server 2025 installed and updated, with VMware Tools,
hostname DC01, static IPv4 networking, and Remote Desktop configured.
Created before AD DS installation.
```

This provides a clean rollback point before installing Active Directory Domain Services and promoting the server to a domain controller.

![Pre-AD DS baseline snapshot](images/08-baseline-snapshot.png)

## Result

Windows Server 2025 was installed, updated, and configured with VMware Tools, a permanent hostname, static IPv4 networking, the correct time zone, and Remote Desktop access. Local network access, external name resolution, outbound HTTPS connectivity, and remote administration were verified.

The completed baseline snapshot provides a clean recovery point. `DC01` is now ready for Active Directory Domain Services installation and domain controller promotion.

## Troubleshooting & Lessons Learned

### Static IPv4 configuration was not applied

**Symptoms**

After VMware DHCP was disabled, the server received an address in the `169.254.x.x` range. It had no default gateway or IPv4 default route and could not reach the VMware NAT gateway or external addresses.

Network tests failed, including:

```powershell
Test-NetConnection 1.1.1.1 -Port 443
ping 192.168.50.2
```

**Cause**

The intended static IPv4 settings had not been applied to the network adapter. Because VMware DHCP was disabled, Windows could not obtain an address automatically and assigned itself an Automatic Private IP Addressing address instead.

An address in the `169.254.0.0/16` range indicates that a device is configured for automatic addressing but could not contact a DHCP server.

**Resolution**

Reopened the IPv4 properties for `Ethernet0` and applied the static address, subnet mask, default gateway, and temporary DNS server.

The corrected configuration was verified with:

```powershell
Get-NetIPConfiguration
Get-NetRoute -DestinationPrefix 0.0.0.0/0
ping 192.168.50.2
```

The server then displayed `192.168.50.10`, contained a valid default route through `192.168.50.2`, and reached the gateway without packet loss. A final HTTPS connection test confirmed that external connectivity and DNS resolution were working.