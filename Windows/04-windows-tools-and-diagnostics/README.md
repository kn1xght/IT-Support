# Windows Process & Service Diagnostics

## Scenario

A Windows 11 virtual machine was required to practice diagnosing performance and service configuration issues with built-in administrative tools. The goal was to generate controlled CPU activity, identify and terminate the responsible process, simulate a disabled service, verify the failure through Command Prompt and Event Viewer, restore the original configuration, and confirm normal operation.

## Environment

- **Operating system:** Windows 11 Enterprise Evaluation
- **Local administrator account:** `Stanic`
- **Test process:** Windows PowerShell
- **Test service:** Print Spooler (`Spooler`)
- **Administrative tools:** Task Manager, Services, Command Prompt, and Event Viewer

## Skills Demonstrated

- Process and CPU usage monitoring
- Process termination
- Windows service configuration
- Command-line service verification
- Event log filtering and analysis
- Service recovery validation

## Implementation

### 1. Generated controlled CPU activity

A controlled performance issue was created by running the following PowerShell loop from Command Prompt:

```cmd
powershell -NoProfile -Command "while ($true) {}"
```

The loop continuously executed on one of the VM's four virtual processors, increasing total CPU usage from the `0–3%` idle baseline to approximately `29%`.

![Generated controlled CPU activity](screenshots/01-controlled-cpu-load.png)

### 2. Identified the high-CPU process

Task Manager was sorted by CPU usage, allowing the Windows PowerShell process to be identified as the source of the increased utilization.

![Identified high-CPU PowerShell process](screenshots/02-high-cpu-process.png)

### 3. Terminated the process and verified CPU recovery

The Windows PowerShell process was ended through Task Manager. CPU utilization dropped to `1%`, and the performance graph showed the decline from the elevated level back toward the original baseline.

![Verified CPU usage recovery](screenshots/03-cpu-usage-restored.png)

### 4. Recorded the Print Spooler baseline configuration

Before introducing the service issue, the Print Spooler properties were reviewed. The service was configured to start automatically and was running.

![Recorded Print Spooler baseline configuration](screenshots/04-print-spooler-baseline.png)

### 5. Simulated a service configuration issue

The Print Spooler service was stopped, and its startup type was changed from `Automatic` to `Disabled`. This created a configuration issue that prevented Windows from starting the service.

![Disabled the Print Spooler service](screenshots/05-print-spooler-disabled.png)

### 6. Verified the service failure through Command Prompt

An elevated Command Prompt was used to query the service:

```cmd
sc query spooler
```

The output reported `STATE : 1 STOPPED`. A start attempt was then made:

```cmd
sc start spooler
```

Windows returned error `1058`, confirming that the service could not start while disabled.

![Verified Print Spooler failure through Command Prompt](screenshots/06-cmd-service-failure.png)

### 7. Reviewed the service configuration event

The System log in Event Viewer was filtered for Service Control Manager events with IDs `7036` and `7040`. Event ID `7040` recorded that the Print Spooler startup type had changed from automatic to disabled, providing a timestamped record of the configuration change.

![Reviewed Service Control Manager events](screenshots/07-service-control-manager-events.png)

### 8. Restored the Print Spooler configuration

The startup type was returned to `Automatic`, and the service was started. Its properties showed that the original configuration and running state had been restored.

![Restored the Print Spooler configuration](screenshots/08-print-spooler-restored.png)

### 9. Verified the service recovery

Command Prompt was used to verify the service state and startup configuration:

```cmd
sc query spooler
sc qc spooler
```

The output reported `STATE : 4 RUNNING` and `START_TYPE : 2 AUTO_START`, confirming that the service had been restored correctly.

![Verified Print Spooler recovery](screenshots/09-service-recovery-verification.png)

## Result

Controlled CPU activity was generated and traced to the Windows PowerShell process through Task Manager. After the process was terminated, CPU utilization returned from approximately `29%` to `1%`.

A disabled Print Spooler service was then diagnosed through Services, Command Prompt, and Event Viewer. The service was restored to its original automatic startup configuration and running state, returning the Windows 11 VM to its original operational state.

## Safety Note

The PowerShell loop and Print Spooler configuration change were created only for controlled testing inside the virtual machine. The original service configuration was recorded before testing and restored afterward.

[← Return to Windows](../)