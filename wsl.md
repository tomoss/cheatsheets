# 🐧 Windows Subsystem for Linux (WSL)

### Install WSL

```
wsl --install
```

### Install a Specific Distribution

First, view available distributions online:

```powershell
wsl --list --online
```

Then install the desired one:

```powershell
wsl --install -d <DistroName>
# Example: wsl --install -d Ubuntu
```

### Check Installed Distributions

```powershell
wsl --list --verbose
```

### Set the default distro

```powershell
wsl --set-default <DistroName>
# Example: wsl --set-default Ubuntu
```

### Verify the default distro

```powershell
wsl --status
```

*Example output:* `Default Distribution: Ubuntu`

### Start and Connect

Optional: start default distro directly

```powershell
wsl
```

To start a specific distribution:

```powershell
wsl -d <DistroName>
# Example: wsl -d Debian
```

### Run Linux Commands from Windows

Run a single command without entering the shell:

```powershell
wsl ls -la
wsl uname -a
```

Run as root:

```powershell
wsl -u root
```

### Check WSL Status

```powershell
wsl --status
```

### Set WSL Version

Set WSL 2 as the global default (recommended):

```powershell
wsl --set-default-version 2
```

### Start with a Login Shell

```powershell
wsl --exec bash --login
```

### Exit and Stop WSL

Exit the current shell:

```powershell
exit
```

Stop all running WSL instances immediately:

```powershell
wsl --shutdown
```

### Restart

```powershell
wsl --shutdown
wsl
```

### Verify Installation

```powershell
wsl --status
wsl --list --verbose
```

### Update WSL Kernel

```powershell
wsl --update
```

### Cleanup / Reset a Distribution

Warning: This deletes all data inside that Linux instance.

```powershell
wsl --shutdown
wsl --unregister <DistroName>
wsl --install -d <DistroName>
```

### Copy Files from Windows to WSL

**Option A** - Using Windows File Explorer (GUI)

```powershell
\\wsl$\Ubuntu\home\<linux-user>\
```

**Option B** - Using PowerShell

```powershell
wsl cp /mnt/c/Users/<windows-user>/Desktop/file.txt /home/<linux-user>/projects/
```


