# First Test

Load the new box on VirtualBox

```bash
gavin.didrichsen@Gavins-MacBook-Pro chocolatey-test-environment % vagrant up
zsh: command not found: vagrant
gavin.didrichsen@Gavins-MacBook-Pro chocolatey-test-environment % brew cask install vagrant
Error: `brew cask` is no longer a `brew` command. Use `brew <command> --cask` instead.
gavin.didrichsen@Gavins-MacBook-Pro chocolatey-test-environment % brew install --cask vagrant
==> Downloading https://formulae.brew.sh/api/cask.jws.json
####################################################################################### 100.0%
==> Downloading https://releases.hashicorp.com/vagrant/2.4.1/vagrant_2.4.1_darwin_amd64.dmg
####################################################################################### 100.0%
==> Installing Cask vagrant
==> Running installer for vagrant with sudo; the password may be necessary.
Password:
installer: Package name is Vagrant
installer: Installing at base path /
installer: The install was successful.
🍺  vagrant was successfully installed!
gavin.didrichsen@Gavins-MacBook-Pro chocolatey-test-environment % vagrant up
Bringing machine 'default' up with 'virtualbox' provider...
==> default: Box 'chocolatey/test-environment' could not be found. Attempting to find and install...
    default: Box Provider: virtualbox
    default: Box Version: >= 0
==> default: Loading metadata for box 'chocolatey/test-environment'
    default: URL: https://vagrantcloud.com/api/v2/vagrant/chocolatey/test-environment
==> default: Adding box 'chocolatey/test-environment' (v3.0.0) for provider: virtualbox
    default: Downloading: https://vagrantcloud.com/chocolatey/boxes/test-environment/versions/3.0.0/providers/virtualbox/unknown/vagrant.box
    default: Calculating and comparing box checksum...
==> default: Successfully added box 'chocolatey/test-environment' (v3.0.0) for 'virtualbox'!
==> default: Preparing master VM for linked clones...
    default: This is a one time operation. Once the master VM is prepared,
    default: it will be used as a base for linked clones, making the creation
    default: of new VMs take milliseconds on a modern system.
==> default: Importing base box 'chocolatey/test-environment'...
==> default: Cloning VM...
==> default: Matching MAC address for NAT networking...
==> default: Checking if box 'chocolatey/test-environment' version '3.0.0' is up to date...
==> default: Setting the name of the VM: chocolatey-test-environment_default_1710169328278_24413
Vagrant is currently configured to create VirtualBox synced folders with
the `SharedFoldersEnableSymlinksCreate` option enabled. If the Vagrant
guest is not trusted, you may want to disable this option. For more
information on this option, please refer to the VirtualBox manual:

  https://www.virtualbox.org/manual/ch04.html#sharedfolders

This option can be disabled globally with an environment variable:

  VAGRANT_DISABLE_VBOXSYMLINKCREATE=1

or on a per folder basis within the Vagrantfile:

  config.vm.synced_folder '/host/path', '/guest/path', SharedFoldersEnableSymlinksCreate: false
==> default: Clearing any previously set network interfaces...
==> default: Preparing network interfaces based on configuration...
    default: Adapter 1: nat
==> default: Forwarding ports...
    default: 3389 (guest) => 3389 (host) (adapter 1)
    default: 5985 (guest) => 55985 (host) (adapter 1)
    default: 22 (guest) => 2222 (host) (adapter 1)
    default: 5986 (guest) => 55986 (host) (adapter 1)
==> default: Running 'pre-boot' VM customizations...
==> default: Booting VM...
==> default: Waiting for machine to boot. This may take a few minutes...
    default: WinRM address: 127.0.0.1:55985
    default: WinRM username: vagrant
    default: WinRM execution_time_limit: PT2H
    default: WinRM transport: negotiate

 *  History restored 
```

Save a "good" snapshot before doing any testing

```bash
gavin.didrichsen@Gavins-MacBook-Pro chocolatey-test-environment % vagrant snapshot save good
==> default: Snapshotting the machine as 'good'...
==> default: Snapshot saved! You can restore the snapshot at any time by
==> default: using `vagrant snapshot restore`. You can delete it using
==> default: `vagrant snapshot delete`.
gavin.didrichsen@Gavins-MacBook-Pro chocolatey-test-environment % git remote -v
origin  https://github.com/gavindidrichsen-forks/chocolatey-test-environment.git (fetch)
origin  https://github.com/gavindidrichsen-forks/chocolatey-test-environment.git (push)
gavin.didrichsen@Gavins-MacBook-Pro chocolatey-test-environment % git branch
* master
gavin.didrichsen@Gavins-MacBook-Pro chocolatey-test-environment % git checkout -b first_test
Switched to a new branch 'first_test'
gavin.didrichsen@Gavins-MacBook-Pro chocolatey-test-environment % git push -u origin first_test
Total 0 (delta 0), reused 0 (delta 0), pack-reused 0
remote: 
remote: Create a pull request for 'first_test' on GitHub by visiting:
remote:      https://github.com/gavindidrichsen-forks/chocolatey-test-environment/pull/new/first_test
remote: 
To https://github.com/gavindidrichsen-forks/chocolatey-test-environment.git
 * [new branch]      first_test -> first_test
branch 'first_test' set up to track 'origin/first_test'.
```

Make the following change to the `Vagrantfile` before provisioning and running the test

```diff
gavin.didrichsen@Gavins-MacBook-Pro chocolatey-test-environment % git diff Vagrantfile 
diff --git a/Vagrantfile b/Vagrantfile
index 457264c..8b5d815 100644
--- a/Vagrantfile
+++ b/Vagrantfile
@@ -135,7 +135,7 @@ Write-Output "Testing package if a line is uncommented."
 # THIS IS WHAT YOU CHANGE
 # - uncomment one of the two and edit it appropriately
 # - See the README for details
-#choco.exe install -fdvy INSERT_NAME --version INSERT_VERSION  --allow-downgrade
+choco.exe install -fdvy pdk --version 3.0.1.3  --allow-downgrade
 #choco.exe install -fdvy INSERT_NAME  --allow-downgrade --source "'c:\\packages;http://chocolatey.org/api/v2/'"
 
 $exitCode = $LASTEXITCODE
gavin.didrichsen@Gavins-MacBook-Pro chocolatey-test-environment % 
```


Now start the test

```bash
gavin.didrichsen@Gavins-MacBook-Pro chocolatey-test-environment % vagrant provision
==> default: Running provisioner: shell...
    default: Running: shell/PrepareWindows.ps1 as C:\tmp\vagrant-shell.ps1
    default: IE Enhanced Security Configuration (ESC) has been disabled. Required for One-Click deploy to work appropriately.
    default: IE first run welcome screen has been disabled. Required for One-Click deploy to work appropriately.
    default: Setting Windows Update service to Manual startup type.
==> default: Running provisioner: shell...
    default: Running: shell/InstallNet4.ps1 as C:\tmp\vagrant-shell.ps1
==> default: Running provisioner: shell...
    default: Running: shell/InstallChocolatey.ps1 as C:\tmp\vagrant-shell.ps1
    default: 
    default: Mode                LastWriteTime         Length Name
    default: ----                -------------         ------ ----
    default: d-----        3/11/2024   3:36 PM                chocInstall
    default: Downloading https://community.chocolatey.org/api/v2/package/chocolatey/2.2.2 to C:\Users\vagrant\AppData\Local\Temp\chocolatey\chocInstall\chocolatey.zip
    default: Download 7Zip commandline tool
    default: Downloading https://community.chocolatey.org/7za.exe to C:\Users\vagrant\AppData\Local\Temp\chocolatey\chocInstall\7za.exe
    default: Extracting C:\Users\vagrant\AppData\Local\Temp\chocolatey\chocInstall\chocolatey.zip to C:\Users\vagrant\AppData\Local\Temp\chocolatey\chocInstall...
    default: 
    default: 7-Zip (a) 23.01 (x86) : Copyright (c) 1999-2023 Igor Pavlov : 2023-06-20
    default: 
    default: Scanning the drive for archives:
    default: 1 file, 5243268 bytes (5121 KiB)
    default: 
    default: Extracting archive: C:\Users\vagrant\AppData\Local\Temp\chocolatey\chocInstall\chocolatey.zip
    default: --
    default: Path = C:\Users\vagrant\AppData\Local\Temp\chocolatey\chocInstall\chocolatey.zip
    default: Type = zip
    default: Physical Size = 5243268
    default: 
    default: Everything is Ok
    default: 
    default: Files: 74
    default: Size:       14325168
    default: Compressed: 5243268
    default: Installing chocolatey on this machine
    default: DEBUG: Initialize-Chocolatey
    default: DEBUG: Host version is 5.1.17763.3770, PowerShell Version is '5.1.17763.3770' and CLR Version is '4.0.30319.42000'.
    default: DEBUG: Install-DotNet48IfMissing called with $forceFxInstall=False
    default: DEBUG: Get-ChocolateyInstallFolder
    default: DEBUG: Set-ChocolateyInstallFolder
    default: DEBUG: Running Install-ChocolateyEnvironmentVariable -variableName 'ChocolateyInstall' -variableValue '' -variableType
    default: 'User'
    default: DEBUG: Running Set-EnvironmentVariable -Name 'ChocolateyInstall' -Value '' -Scope 'User'
    default: DEBUG: Test-ProcessAdminRights: returning True
    default: DEBUG: Administrator installing so using Machine environment variable target instead of User.
    default: DEBUG: Running Install-ChocolateyEnvironmentVariable -variableName 'ChocolateyInstall' -variableValue '' -variableType
    default: 'Machine'
    default: DEBUG: Test-ProcessAdminRights: returning True
    default: DEBUG: Running Set-EnvironmentVariable -Name 'ChocolateyInstall' -Value '' -Scope 'Machine'
    default: Creating ChocolateyInstall as an environment variable (targeting 'Machine')
    default:   Setting ChocolateyInstall to 'C:\ProgramData\chocolatey'
    default: WARNING: It's very likely you will need to close and reopen your shell
    default:   before you can use choco.
    default: DEBUG: Running Install-ChocolateyEnvironmentVariable -variableName 'ChocolateyInstall' -variableValue
    default: 'C:\ProgramData\chocolatey' -variableType 'Machine'
    default: DEBUG: Test-ProcessAdminRights: returning True
    default: DEBUG: Running Set-EnvironmentVariable -Name 'ChocolateyInstall' -Value 'C:\ProgramData\chocolatey' -Scope 'Machine'
    default: DEBUG: Registry type for ChocolateyInstall is/will be String
    default: DEBUG:
    default: using System;
    default: using System.Runtime.InteropServices;
    default: 
    default: namespace Win32
    default: {
    default:     public class NativeMethods
    default:     {
    default:     [DllImport("user32.dll", SetLastError = true, CharSet = CharSet.Auto)]
    default: public static extern IntPtr SendMessageTimeout(
    default:     IntPtr hWnd, uint Msg, UIntPtr wParam, string lParam,
    default:     uint fuFlags, uint uTimeout, out UIntPtr lpdwResult);
    default: 
    default:     }
    default: 
    default: }
    default: DEBUG: Running Update-SessionEnvironment
    default: DEBUG: Create-DirectoryIfNotExists
    default: DEBUG: Ensure-Permissions
    default: DEBUG: Test-ProcessAdminRights: returning True
    default: DEBUG: Removing existing permissions.
    default: Restricting write permissions to Administrators
    default: DEBUG: Current user no longer set due to possible escalation of privileges - set
    default: $env:ChocolateyInstallAllowCurrentUser="true" if you require this.
    default: DEBUG: Set Owner to Administrators
    default: DEBUG: Default Installation folder - removing inheritance with no copy
    default: DEBUG: Allow users to append to log files.
    default: DEBUG: Create-DirectoryIfNotExists
    default: We are setting up the Chocolatey package repository.
    default: The packages themselves go to 'C:\ProgramData\chocolatey\lib'
    default:   (i.e. C:\ProgramData\chocolatey\lib\yourPackageName).
    default: A shim file for the command line goes to 'C:\ProgramData\chocolatey\bin'
    default:   and points to an executable in 'C:\ProgramData\chocolatey\lib\yourPackageName'.
    default: 
    default: Creating Chocolatey folders if they do not already exist.
    default: 
    default: DEBUG: Create-DirectoryIfNotExists
    default: DEBUG: Create-DirectoryIfNotExists
    default: DEBUG: Install-ChocolateyFiles
    default: DEBUG: Removing install files in chocolateyInstall, helpers, redirects, and tools
    default: DEBUG: Attempting to move choco.exe to choco.exe.old so we can place the new version here.
    default: DEBUG: Unpacking files required for Chocolatey.
    default: DEBUG: Copying the contents of 'C:\Users\vagrant\AppData\Local\Temp\chocolatey\chocInstall\tools\chocolateyInstall' to
    default: 'C:\ProgramData\chocolatey'.
    default: DEBUG: Ensure-ChocolateyLibFiles
    default: DEBUG: Create-DirectoryIfNotExists
    default: chocolatey.nupkg file not installed in lib.
    default:  Attempting to locate it from bootstrapper.
    default: DEBUG: First the zip file at 'C:\Users\vagrant\AppData\Local\Temp\chocolatey\chocoInstall\chocolatey.zip'.
    default: DEBUG: Then from a neighboring chocolatey.*nupkg file
    default: 'C:\Users\vagrant\AppData\Local\Temp\chocolatey\chocInstall\tools/../../'.
    default: DEBUG: Install-ChocolateyBinFiles
    default: DEBUG: Installing the bin file redirects
    default: DEBUG: Attempting to copy C:\ProgramData\chocolatey\redirects\choco.exe to C:\ProgramData\chocolatey\bin\choco.exe
    default: DEBUG: Added command choco
    default: DEBUG: Attempting to copy C:\ProgramData\chocolatey\redirects\RefreshEnv.cmd to
    default: C:\ProgramData\chocolatey\bin\RefreshEnv.cmd
    default: DEBUG: Added command RefreshEnv
    default: DEBUG: Initialize-ChocolateyPath
    default: DEBUG: Initializing Chocolatey Path if required
    default: DEBUG: Test-ProcessAdminRights: returning True
    default: DEBUG: Administrator installing so using Machine environment variable target instead of User.
    default: DEBUG: Running Install-ChocolateyPath -pathToInstall 'C:\ProgramData\chocolatey\bin' -pathType 'Machine'
    default: DEBUG: Running Update-SessionEnvironment
    default: PATH environment variable does not have C:\ProgramData\chocolatey\bin in it. Adding...
    default: DEBUG: Test-ProcessAdminRights: returning True
    default: DEBUG: Running Set-EnvironmentVariable -Name 'Path' -Value
    default: '%SystemRoot%\system32;%SystemRoot%;%SystemRoot%\System32\Wbem;%SYSTEMROOT%\System32\WindowsPowerShell\v1.0\;%SYSTEMROO
    default: T%\System32\OpenSSH\;C:\ProgramData\chocolatey\bin;' -Scope 'Machine'
    default: DEBUG: Registry type for Path is/will be ExpandString
    default: DEBUG: Running Update-SessionEnvironment
    default: DEBUG: Process-ChocolateyBinFiles
    default: DEBUG: Host version is 5.1.17763.3770, PowerShell Version is '5.1.17763.3770' and CLR Version is '4.0.30319.42000'.
    default: DEBUG: Add-ChocolateyProfile
    default: DEBUG: Creating 'C:\Users\vagrant\Documents\WindowsPowerShell'
    default: WARNING: Not setting tab completion: Profile file does not exist at
    default: 'C:\Users\vagrant\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1'.
    default: DEBUG: Initializing Chocolatey files, etc by running Chocolatey...
    default: DEBUG: Get-ChocolateyInstallFolder
    default: DEBUG: Chocolatey execution completed successfully.
    default: Chocolatey (choco.exe) is now ready.
    default: You can call choco from anywhere, command line or powershell by typing choco.
    default: Run choco /? for a list of functions.
    default: You may need to shut down and restart powershell and/or consoles
    default:  first prior to using choco.
    default: DEBUG: Remove-OldChocolateyInstall
    default: Ensuring chocolatey commands are on the path
    default: Ensuring chocolatey.nupkg is in the lib folder
    default: Chocolatey v2.2.2
    default: autoUninstaller was enabled by default. Explicitly setting value.
    default: Enabled autoUninstaller
    default: Chocolatey v2.2.2
    default: Enabled allowGlobalConfirmation
    default: Chocolatey v2.2.2
    default: Enabled logEnvironmentValues
    default: Chocolatey v2.2.2
    default: Disabled showDownloadProgress
    default: 
    default: 
==> default: Running provisioner: shell...
    default: Running: shell/NotifyGuiAppsOfEnvironmentChanges.ps1 as C:\tmp\vagrant-shell.ps1
    default: 1
    default: 
    default: SUCCESS: Specified value was saved.
==> default: Running provisioner: shell...
    default: Running: shell/PostSetup.ps1 as C:\tmp\vagrant-shell.ps1
    default: Chocolatey v2.2.2
    default: Installing the following packages:
    default: kb2919442
    default: By installing, you accept licenses for the packages.
    default: [NuGet]   GET https://community.chocolatey.org/api/v2/Packages(Id='kb2919442',Version='1.0.20160915')
    default: [NuGet]   OK https://community.chocolatey.org/api/v2/Packages(Id='kb2919442',Version='1.0.20160915') 395ms
    default: [NuGet]   CACHE https://community.chocolatey.org/api/v2/Packages(Id='KB2919442',Version='1.0.20160915')
    default: [NuGet] Resolving dependency information took 0 ms
    default: [NuGet]   GET https://community.chocolatey.org/api/v2/package/KB2919442/1.0.20160915
    default: [NuGet]   OK https://community.chocolatey.org/api/v2/package/KB2919442/1.0.20160915 538ms
    default: [NuGet] Acquiring lock for the installation of KB2919442 1.0.20160915
    default: [NuGet] Acquired lock for the installation of KB2919442 1.0.20160915
    default: [NuGet] Installed KB2919442 1.0.20160915 from https://community.chocolatey.org/api/v2/ with content hash TQeoUGG09UgFxQ8kkrknyaOGchD6tctB4eWoyVVNZmGcyMVSTSyvgeB3xmPdF/BLXueGJfRFhEtaGKUF3pE6eA==.
    default: [NuGet] Adding package 'KB2919442.1.0.20160915' to folder 'C:\ProgramData\chocolatey\lib'
    default: [NuGet] Added package 'KB2919442.1.0.20160915' to folder 'C:\ProgramData\chocolatey\lib'
    default: 
    default: KB2919442 v1.0.20160915 [Approved]
    default: KB2919442 package files install completed. Performing other installation steps.
    default:  The install of KB2919442 was successful.
    default:   Software installed to 'C:\ProgramData\chocolatey\lib\KB2919442'
    default: 
    default: Chocolatey installed 1/1 packages.
    default:  See the log for details (C:\ProgramData\chocolatey\logs\chocolatey.log).
    default: 
    default: Did you know the proceeds of Pro (and some proceeds from other
    default:  licensed editions) go into bettering the community infrastructure?
    default:  Your support ensures an active community, keeps Chocolatey tip-top,
    default:  plus it nets you some awesome features!
    default:  https://chocolatey.org/compare
    default: Chocolatey v2.2.2
    default: Installing the following packages:
    default: kb2919355
    default: By installing, you accept licenses for the packages.
    default: [NuGet]   GET https://community.chocolatey.org/api/v2/Packages(Id='kb2919355',Version='1.0.20160915')
    default: [NuGet]   OK https://community.chocolatey.org/api/v2/Packages(Id='kb2919355',Version='1.0.20160915') 437ms
    default: [NuGet]   CACHE https://community.chocolatey.org/api/v2/Packages(Id='KB2919355',Version='1.0.20160915')
    default: [NuGet]   GET https://community.chocolatey.org/api/v2/$metadata
    default: [NuGet]   OK https://community.chocolatey.org/api/v2/$metadata 46ms
    default: [NuGet]   GET https://community.chocolatey.org/api/v2/FindPackagesById()?id='KB2919442'&semVerLevel=2.0.0
    default: [NuGet]   OK https://community.chocolatey.org/api/v2/FindPackagesById()?id='KB2919442'&semVerLevel=2.0.0 43ms
    default: [NuGet] Resolving dependency information took 0 ms
    default: [NuGet]   GET https://community.chocolatey.org/api/v2/package/KB2919355/1.0.20160915
    default: [NuGet]   OK https://community.chocolatey.org/api/v2/package/KB2919355/1.0.20160915 552ms
    default: [NuGet] Acquiring lock for the installation of KB2919355 1.0.20160915
    default: [NuGet] Acquired lock for the installation of KB2919355 1.0.20160915
    default: [NuGet] Installed KB2919355 1.0.20160915 from https://community.chocolatey.org/api/v2/ with content hash RCCHts7pC4SV1kJdKJMOC3mzKtjmiShaBkl0KmTsF1G9mNKNXB1qWckzDOQ++J4rIIhk6RWEjQhjb3eFKxOmLg==.
    default: [NuGet] Adding package 'KB2919355.1.0.20160915 : KB2919442 [1.0.20160915, )' to folder 'C:\ProgramData\chocolatey\lib'
    default: [NuGet] Added package 'KB2919355.1.0.20160915 : KB2919442 [1.0.20160915, )' to folder 'C:\ProgramData\chocolatey\lib'
    default: 
    default: KB2919355 v1.0.20160915 [Approved]
    default: KB2919355 package files install completed. Performing other installation steps.
    default:  The install of KB2919355 was successful.
    default:   Software installed to 'C:\ProgramData\chocolatey\lib\KB2919355'
    default: 
    default: Chocolatey installed 1/1 packages.
    default:  See the log for details (C:\ProgramData\chocolatey\logs\chocolatey.log).
    default: Chocolatey v2.2.2
    default: Installing the following packages:
    default: kb2999226
    default: By installing, you accept licenses for the packages.
    default: [NuGet]   GET https://community.chocolatey.org/api/v2/Packages(Id='kb2999226',Version='1.0.20181019')
    default: [NuGet]   OK https://community.chocolatey.org/api/v2/Packages(Id='kb2999226',Version='1.0.20181019') 440ms
    default: [NuGet]   CACHE https://community.chocolatey.org/api/v2/Packages(Id='KB2999226',Version='1.0.20181019')
    default: [NuGet]   CACHE https://community.chocolatey.org/api/v2/$metadata
    default: [NuGet]   GET https://community.chocolatey.org/api/v2/FindPackagesById()?id='kb2919355'&semVerLevel=2.0.0
    default: [NuGet]   OK https://community.chocolatey.org/api/v2/FindPackagesById()?id='kb2919355'&semVerLevel=2.0.0 55ms
    default: [NuGet]   CACHE https://community.chocolatey.org/api/v2/FindPackagesById()?id='KB2919442'&semVerLevel=2.0.0
    default: [NuGet]   CACHE https://community.chocolatey.org/api/v2/FindPackagesById()?id='KB2919442'&semVerLevel=2.0.0
    default: [NuGet]   GET https://community.chocolatey.org/api/v2/FindPackagesById()?id='chocolatey-windowsupdate.extension'&semVerLevel=2.0.0
    default: [NuGet]   OK https://community.chocolatey.org/api/v2/FindPackagesById()?id='chocolatey-windowsupdate.extension'&semVerLevel=2.0.0 52ms
    default: [NuGet] Resolving dependency information took 0 ms
    default: [NuGet]   GET https://community.chocolatey.org/api/v2/Packages(Id='chocolatey-windowsupdate.extension',Version='1.0.5')
    default: [NuGet]   OK https://community.chocolatey.org/api/v2/Packages(Id='chocolatey-windowsupdate.extension',Version='1.0.5') 49ms
    default: [NuGet]   GET https://community.chocolatey.org/api/v2/package/chocolatey-windowsupdate.extension/1.0.5
    default: [NuGet]   OK https://community.chocolatey.org/api/v2/package/chocolatey-windowsupdate.extension/1.0.5 284ms
    default: [NuGet] Acquiring lock for the installation of chocolatey-windowsupdate.extension 1.0.5
    default: [NuGet] Acquired lock for the installation of chocolatey-windowsupdate.extension 1.0.5
    default: [NuGet] Installed chocolatey-windowsupdate.extension 1.0.5 from https://community.chocolatey.org/api/v2/ with content hash uZdp2WxTVq8bq8ksHoeK3EnOmZgyBnMEJf7p9ycVpQKUvEz4T8XjGB0bg6svF8lBvEq9lDhkDXQI1qnnJOxf4A==.
    default: [NuGet] Adding package 'chocolatey-windowsupdate.extension.1.0.5' to folder 'C:\ProgramData\chocolatey\lib'
    default: [NuGet] Added package 'chocolatey-windowsupdate.extension.1.0.5' to folder 'C:\ProgramData\chocolatey\lib'
    default: 
    default: chocolatey-windowsupdate.extension v1.0.5 [Approved]
    default: chocolatey-windowsupdate.extension package files install completed. Performing other installation steps.
    default:  Installed/updated chocolatey-windowsupdate extensions.
    default:  The install of chocolatey-windowsupdate.extension was successful.
    default:   Software installed to 'C:\ProgramData\chocolatey\extensions\chocolatey-windowsupdate'
    default: [NuGet]   GET https://community.chocolatey.org/api/v2/package/KB2999226/1.0.20181019
    default: [NuGet]   OK https://community.chocolatey.org/api/v2/package/KB2999226/1.0.20181019 187ms
    default: [NuGet] Acquiring lock for the installation of KB2999226 1.0.20181019
    default: [NuGet] Acquired lock for the installation of KB2999226 1.0.20181019
    default: [NuGet] Installed KB2999226 1.0.20181019 from https://community.chocolatey.org/api/v2/ with content hash I835VAD2tV0upl6KII8haovTLip3QkrtfdjcrxqdzVPiy+x4E9dln+FBgfl/R5phzD0AWgWmIC7g6PVusrpF+w==.
    default: [NuGet] Adding package 'KB2999226.1.0.20181019 : chocolatey-windowsupdate.extension [1.0.2, ), kb2919355 [1.0.20160915, )' to folder 'C:\ProgramData\chocolatey\lib'
    default: [NuGet] Added package 'KB2999226.1.0.20181019 : chocolatey-windowsupdate.extension [1.0.2, ), kb2919355 [1.0.20160915, )' to folder 'C:\ProgramData\chocolatey\lib'
    default: 
    default: KB2999226 v1.0.20181019 [Approved] - Possibly broken
    default: KB2999226 package files install completed. Performing other installation steps.
    default:  The install of KB2999226 was successful.
    default:   Software installed to 'C:\ProgramData\chocolatey\lib\KB2999226'
    default: 
    default: Chocolatey installed 2/2 packages.
    default:  See the log for details (C:\ProgramData\chocolatey\logs\chocolatey.log).
    default: Chocolatey v2.2.2
    default: Installing the following packages:
    default: kb3035131
    default: By installing, you accept licenses for the packages.
    default: [NuGet]   GET https://community.chocolatey.org/api/v2/Packages(Id='kb3035131',Version='1.0.3')
    default: [NuGet]   OK https://community.chocolatey.org/api/v2/Packages(Id='kb3035131',Version='1.0.3') 593ms
    default: [NuGet]   CACHE https://community.chocolatey.org/api/v2/Packages(Id='KB3035131',Version='1.0.3')
    default: [NuGet]   CACHE https://community.chocolatey.org/api/v2/$metadata
    default: [NuGet]   CACHE https://community.chocolatey.org/api/v2/FindPackagesById()?id='chocolatey-windowsupdate.extension'&semVerLevel=2.0.0
    default: [NuGet] Resolving dependency information took 0 ms
    default: [NuGet]   GET https://community.chocolatey.org/api/v2/package/KB3035131/1.0.3
    default: [NuGet]   OK https://community.chocolatey.org/api/v2/package/KB3035131/1.0.3 285ms
    default: [NuGet] Acquiring lock for the installation of KB3035131 1.0.3
    default: [NuGet] Acquired lock for the installation of KB3035131 1.0.3
    default: [NuGet] Installed KB3035131 1.0.3 from https://community.chocolatey.org/api/v2/ with content hash YSHu/anZrSNGjXxAQoDcqNjhdY2qjZQndonleto3HTnAbit/yA/ltRQrQCZ+kZqMgiJZk8gQ1snn6NupYsdxdg==.
    default: [NuGet] Adding package 'KB3035131.1.0.3 : chocolatey-windowsupdate.extension [1.0.4, )' to folder 'C:\ProgramData\chocolatey\lib'
    default: [NuGet] Added package 'KB3035131.1.0.3 : chocolatey-windowsupdate.extension [1.0.4, )' to folder 'C:\ProgramData\chocolatey\lib'
    default: 
    default: KB3035131 v1.0.3 [Approved]
    default: KB3035131 package files install completed. Performing other installation steps.
    default:  The install of KB3035131 was successful.
    default:   Software installed to 'C:\ProgramData\chocolatey\lib\KB3035131'
    default: 
    default: Chocolatey installed 1/1 packages.
    default:  See the log for details (C:\ProgramData\chocolatey\logs\chocolatey.log).
    default: Chocolatey v2.2.2
    default: Installing the following packages:
    default: kb3118401
    default: By installing, you accept licenses for the packages.
    default: [NuGet]   GET https://community.chocolatey.org/api/v2/Packages(Id='kb3118401',Version='1.0.5')
    default: [NuGet]   OK https://community.chocolatey.org/api/v2/Packages(Id='kb3118401',Version='1.0.5') 431ms
    default: [NuGet]   CACHE https://community.chocolatey.org/api/v2/Packages(Id='KB3118401',Version='1.0.5')
    default: [NuGet]   CACHE https://community.chocolatey.org/api/v2/$metadata
    default: [NuGet]   CACHE https://community.chocolatey.org/api/v2/FindPackagesById()?id='chocolatey-windowsupdate.extension'&semVerLevel=2.0.0
    default: [NuGet]   CACHE https://community.chocolatey.org/api/v2/FindPackagesById()?id='KB2919355'&semVerLevel=2.0.0
    default: [NuGet]   CACHE https://community.chocolatey.org/api/v2/FindPackagesById()?id='KB2919442'&semVerLevel=2.0.0
    default: [NuGet]   CACHE https://community.chocolatey.org/api/v2/FindPackagesById()?id='KB2919442'&semVerLevel=2.0.0
    default: [NuGet] Resolving dependency information took 0 ms
    default: [NuGet]   GET https://community.chocolatey.org/api/v2/package/KB3118401/1.0.5
    default: [NuGet]   OK https://community.chocolatey.org/api/v2/package/KB3118401/1.0.5 291ms
    default: [NuGet] Acquiring lock for the installation of KB3118401 1.0.5
    default: [NuGet] Acquired lock for the installation of KB3118401 1.0.5
    default: [NuGet] Installed KB3118401 1.0.5 from https://community.chocolatey.org/api/v2/ with content hash fNyuRYctdGEKIYj3evNul0/6OSLXmlimNZ1VbHPwDwgSRE2Z+g+3eLBmqOr8zGXrCqvVWW0bmRSQKG197FDyeQ==.
    default: [NuGet] Adding package 'KB3118401.1.0.5 : chocolatey-windowsupdate.extension [1.0.4, ), KB2919355 [1.0.20160915, )' to folder 'C:\ProgramData\chocolatey\lib'
    default: [NuGet] Added package 'KB3118401.1.0.5 : chocolatey-windowsupdate.extension [1.0.4, ), KB2919355 [1.0.20160915, )' to folder 'C:\ProgramData\chocolatey\lib'
    default: 
    default: KB3118401 v1.0.5 [Approved]
    default: KB3118401 package files install completed. Performing other installation steps.
    default:  The install of KB3118401 was successful.
    default:   Software installed to 'C:\ProgramData\chocolatey\lib\KB3118401'
    default: 
    default: Chocolatey installed 1/1 packages.
    default:  See the log for details (C:\ProgramData\chocolatey\logs\chocolatey.log).
==> default: Running provisioner: shell...
    default: Running: inline PowerShell script
    default: 
    default: SUCCESS: Specified value was saved.
    default: Testing package if a line is uncommented.
    default: Chocolatey v2.2.2
    default: Chocolatey is running on Windows v 10.0.17763.0
    default: Attempting to delete file "C:/ProgramData/chocolatey/choco.exe.old".
    default: Attempting to delete file "C:\ProgramData\chocolatey\choco.exe.old".
    default: Command line: "C:\ProgramData\chocolatey\choco.exe" install -fdvy pdk --version 3.0.1.3 --allow-downgrade
    default: Received arguments: install -fdvy pdk --version 3.0.1.3 --allow-downgrade
    default: RemovePendingPackagesTask is now ready and waiting for PreRunMessage.
    default: Sending message 'PreRunMessage' out if there are subscribers...
    default: [Pending] Removing all pending packages that should not be considered installed...
    default: Performing validation checks.
    default: Global Configuration Validation Checks:
    default:  - Package Exit Code / Exit On Reboot = Checked
    default: System State Validation Checks:
    default:  Reboot Requirement Checks:
    default:  - Pending Computer Rename = Checked
    default:  - Pending Component Based Servicing = Checked
    default:  - Pending Windows Auto Update = Checked
    default:  - Pending File Rename Operations = Checked
    default:  - Pending Windows Package Installer = Checked
    default:  - Pending Windows Package Installer SysWow64 = Checked
    default: Cache Folder Lockdown Checks:
    default:  - Elevated State = Checked
    default:  - Folder Exists = Checked
    default:  - Folder lockdown = Checked
    default: The source 'https://community.chocolatey.org/api/v2/' evaluated to a 'normal' source type
    default: 
    default: NOTE: Hiding sensitive configuration data! Please double and triple
    default:  check to be sure no sensitive data is shown, especially if copying
    default:  output to a gist for review.
    default: Configuration: CommandName='install'|
    default: CacheLocation='C:\Users\vagrant\AppData\Local\Temp\chocolatey'|
    default: CommandExecutionTimeoutSeconds='2700'|WebRequestTimeoutSeconds='30'|
    default: Sources='https://community.chocolatey.org/api/v2/'|SourceType='normal'|
    default: ShowOnlineHelp='False'|Debug='True'|Verbose='True'|Trace='False'|
    default: Force='True'|Noop='False'|HelpRequested='False'|
    default: UnsuccessfulParsing='False'|RegularOutput='True'|QuietOutput='False'|
    default: PromptForConfirmation='False'|DisableCompatibilityChecks='False'|
    default: AcceptLicense='True'|AllowUnofficialBuild='False'|Input='pdk'|
    default: Version='3.0.1.3'|AllVersions='False'|
    default: SkipPackageInstallProvider='False'|SkipHookScripts='False'|
    default: PackageNames='pdk'|Prerelease='False'|ForceX86='False'|
    default: OverrideArguments='False'|NotSilent='False'|
    default: ApplyPackageParametersToDependencies='False'|
    default: ApplyInstallArgumentsToDependencies='False'|IgnoreDependencies='False'|
    default: CacheExpirationInMinutes='30'|AllowDowngrade='True'|
    default: ForceDependencies='False'|PinPackage='False'|
    default: Information.PlatformType='Windows'|
    default: Information.PlatformVersion='10.0.17763.0'|
    default: Information.PlatformName='Windows Server 2016'|
    default: Information.ChocolateyVersion='2.2.2.0'|
    default: Information.ChocolateyProductVersion='2.2.2'|
    default: Information.FullName='choco, Version=2.2.2.0, Culture=neutral, PublicKeyToken=79d02ea9cad655eb'|
    default: 
    default: Information.Is64BitOperatingSystem='True'|
    default: Information.Is64BitProcess='True'|Information.IsInteractive='True'|
    default: Information.UserName='vagrant'|
    default: Information.UserDomainName='WIN-4ST2OMCALM8'|
    default: Information.IsUserAdministrator='True'|
    default: Information.IsUserSystemAccount='False'|
    default: Information.IsUserRemoteDesktop='False'|
    default: Information.IsUserRemote='True'|
    default: Information.IsProcessElevated='True'|
    default: Information.IsLicensedVersion='False'|
    default: Information.IsLicensedAssemblyLoaded='False'|
    default: Information.LicenseType='Foss'|
    default: Information.CurrentDirectory='C:\Windows\system32'|
    default: Features.AutoUninstaller='True'|Features.ChecksumFiles='True'|
    default: Features.AllowEmptyChecksums='False'|
    default: Features.AllowEmptyChecksumsSecure='True'|
    default: Features.FailOnAutoUninstaller='False'|
    default: Features.FailOnStandardError='False'|Features.UsePowerShellHost='True'|
    default: Features.LogEnvironmentValues='True'|Features.LogWithoutColor='False'|
    default: Features.VirusCheck='False'|
    default: Features.FailOnInvalidOrMissingLicense='False'|
    default: Features.IgnoreInvalidOptionsSwitches='True'|
    default: Features.UsePackageExitCodes='True'|
    default: Features.UseEnhancedExitCodes='False'|
    default: Features.UseFipsCompliantChecksums='False'|
    default: Features.ShowNonElevatedWarnings='True'|
    default: Features.ShowDownloadProgress='False'|
    default: Features.StopOnFirstPackageFailure='False'|
    default: Features.UseRememberedArgumentsForUpgrades='False'|
    default: Features.IgnoreUnfoundPackagesOnUpgradeOutdated='False'|
    default: Features.SkipPackageUpgradesWhenNotInstalled='False'|
    default: Features.RemovePackageInformationOnUninstall='False'|
    default: Features.ExitOnRebootDetected='False'|
    default: Features.LogValidationResultsOnWarnings='True'|
    default: Features.UsePackageRepositoryOptimizations='True'|
    default: ListCommand.LocalOnly='False'|ListCommand.IdOnly='False'|
    default: ListCommand.IncludeRegistryPrograms='False'|ListCommand.PageSize='25'|
    default: ListCommand.Exact='False'|ListCommand.ByIdOnly='False'|
    default: ListCommand.ByTagOnly='False'|ListCommand.IdStartsWith='False'|
    default: ListCommand.OrderByPopularity='False'|ListCommand.ApprovedOnly='False'|
    default: ListCommand.DownloadCacheAvailable='False'|
    default: ListCommand.NotBroken='False'|
    default: ListCommand.IncludeVersionOverrides='False'|
    default: ListCommand.ExplicitPageSize='False'|
    default: ListCommand.ExplicitSource='False'|
    default: UpgradeCommand.FailOnUnfound='False'|
    default: UpgradeCommand.FailOnNotInstalled='False'|
    default: UpgradeCommand.NotifyOnlyAvailableUpgrades='False'|
    default: UpgradeCommand.ExcludePrerelease='False'|
    default: NewCommand.AutomaticPackage='False'|
    default: NewCommand.UseOriginalTemplate='False'|SourceCommand.Command='unknown'|
    default: SourceCommand.Priority='0'|SourceCommand.BypassProxy='False'|
    default: SourceCommand.AllowSelfService='False'|
    default: SourceCommand.VisibleToAdminsOnly='False'|
    default: FeatureCommand.Command='unknown'|ConfigCommand.Command='Unknown'|
    default: ApiKeyCommand.Command='Unknown'|PinCommand.Command='Unknown'|
    default: OutdatedCommand.IgnorePinned='False'|
    default: ExportCommand.IncludeVersionNumbers='False'|Proxy.BypassOnLocal='True'|
    default: TemplateCommand.Command='unknown'|CacheCommand.Command='Unknown'|
    default: CacheCommand.RemoveExpiredItemsOnly='False'|
    default: _ Chocolatey:ChocolateyInstallCommand - Normal Run Mode _
    default: Installing the following packages:
    default: pdk
    default: By installing, you accept licenses for the packages.
    default: Current environment values (may contain sensitive data):
    default:   * 'Path'='C:\Users\vagrant\AppData\Local\Microsoft\WindowsApps;' ('User')
    default:   * 'TEMP'='C:\Users\vagrant\AppData\Local\Temp' ('User')
    default:   * 'TMP'='C:\Users\vagrant\AppData\Local\Temp' ('User')
    default:   * 'ChocolateyLastPathUpdate'='133546450315386563' ('User')
    default:   * 'trigger'='1' ('User')
    default:   * 'ComSpec'='C:\Windows\system32\cmd.exe' ('Machine')
    default:   * 'DriverData'='C:\Windows\System32\Drivers\DriverData' ('Machine')
    default:   * 'OS'='Windows_NT' ('Machine')
    default:   * 'Path'='C:\Windows\system32;C:\Windows;C:\Windows\System32\Wbem;C:\Windows\System32\WindowsPowerShell\v1.0\;C:\Windows\System32\OpenSSH\;C:\ProgramData\chocolatey\bin;' ('Machine')
    default:   * 'PATHEXT'='.COM;.EXE;.BAT;.CMD;.VBS;.VBE;.JS;.JSE;.WSF;.WSH;.MSC' ('Machine')
    default:   * 'PROCESSOR_ARCHITECTURE'='AMD64' ('Machine')
    default:   * 'PSModulePath'='C:\Program Files\WindowsPowerShell\Modules;C:\Windows\system32\WindowsPowerShell\v1.0\Modules' ('Machine')
    default:   * 'TEMP'='C:\Windows\TEMP' ('Machine')
    default:   * 'TMP'='C:\Windows\TEMP' ('Machine')
    default:   * 'USERNAME'='SYSTEM' ('Machine')
    default:   * 'windir'='C:\Windows' ('Machine')
    default:   * 'NUMBER_OF_PROCESSORS'='2' ('Machine')
    default:   * 'PROCESSOR_LEVEL'='6' ('Machine')
    default:   * 'PROCESSOR_IDENTIFIER'='Intel64 Family 6 Model 158 Stepping 9, GenuineIntel' ('Machine')
    default:   * 'PROCESSOR_REVISION'='9e09' ('Machine')
    default:   * 'ChocolateyInstall'='C:\ProgramData\chocolatey' ('Machine')
    default: Running list with the following filter = ''
    default: --- Start of List ---
    default: Resolving resource PackageSearchResource for source C:\ProgramData\chocolatey\lib
    default: chocolatey 2.2.2
    default: chocolatey-windowsupdate.extension 1.0.5
    default: KB2919355 1.0.20160915
    default: KB2919442 1.0.20160915
    default: KB2999226 1.0.20181019
    default: KB3035131 1.0.3
    default: KB3118401 1.0.5
    default: --- End of List ---
    default: Resolving resource PackageMetadataResource for source https://community.chocolatey.org/api/v2/
    default: [NuGet]   GET https://community.chocolatey.org/api/v2/Packages(Id='pdk',Version='3.0.1.3')
    default: [NuGet]   OK https://community.chocolatey.org/api/v2/Packages(Id='pdk',Version='3.0.1.3') 425ms
    default: Resolving resource DependencyInfoResource for source https://community.chocolatey.org/api/v2/
    default: [NuGet]   CACHE https://community.chocolatey.org/api/v2/Packages(Id='pdk',Version='3.0.1.3')
    default: [NuGet] Resolving dependency information took 0 ms
    default: Resolving resource DownloadResource for source https://community.chocolatey.org/api/v2/
    default: Attempting to delete file "".
    default: [NuGet]   GET https://community.chocolatey.org/api/v2/package/pdk/3.0.1.3
    default: [NuGet]   OK https://community.chocolatey.org/api/v2/package/pdk/3.0.1.3 867ms
    default: [NuGet] Acquiring lock for the installation of pdk 3.0.1.3
    default: [NuGet] Acquired lock for the installation of pdk 3.0.1.3
    default: [NuGet] Installed pdk 3.0.1.3 from https://community.chocolatey.org/api/v2/ with content hash 0+398CoNhTsTzDmYm4C4/Ee5VdSS9rdYceF6FVhqi7rkAVhpi53K3q+Nw07Ej9sUgCWDTX5U+LopQf//4RAshA==.
    default: [NuGet] Adding package 'pdk.3.0.1.3' to folder 'C:\ProgramData\chocolatey\lib'
    default: [NuGet] Added package 'pdk.3.0.1.3' to folder 'C:\ProgramData\chocolatey\lib'
    default: Attempting to delete file "C:\Users\vagrant\AppData\Local\Temp\chocolatey\ChocolateyScratch\pdk/3.0.1.3\pdk.3.0.1.3.nupkg".
    default: Attempting to delete file "C:\Users\vagrant\AppData\Local\Temp\chocolatey\ChocolateyScratch\pdk/3.0.1.3\.nupkg.metadata".
    default: Attempting to delete file "C:\Users\vagrant\AppData\Local\Temp\chocolatey\ChocolateyScratch\pdk/3.0.1.3\pdk.3.0.1.3.nupkg.sha512".
    default: 
    default: pdk v3.0.1.3 (forced) - Possibly broken
    default: pdk package files install completed. Performing other installation steps.
    default: Setting installer args for pdk
    default: Setting package parameters for pdk
    default: Contents of 'C:\ProgramData\chocolatey\lib\pdk\tools\chocolateyinstall.ps1':
    default: $packageName = 'pdk'
    default: $url32       = ''
    default: $url64       = 'https://downloads.puppet.com/windows/puppet8/pdk-3.0.1.3-x64.msi'
    default: $checksum32  = ''
    default: $checksum64  = '301d9930c563c662f170b9b166390750ac2fd7193fe391e2a07ac39a64a051e7'
    default: 
    default: $packageArgs = @{
    default:   packageName    = $packageName
    default:   fileType       = 'MSI'
    default:   url            = $url32
    default:   url64bit       = $url64
    default:   checksum       = $checksum32
    default:   checksum64     = $checksum64
    default:   checksumType   = 'sha256'
    default:   checksumType64 = 'sha256'
    default:   silentArgs     = "/qn /norestart"
    default:   validExitCodes = @(0, 3010, 1641)
    default: }
    default: 
    default: Install-ChocolateyPackage @packageArgs
    default: 
    default: Calling built-in PowerShell host with ['[System.Threading.Thread]::CurrentThread.CurrentCulture = '';[System.Threading.Thread]::CurrentThread.CurrentUICulture = '';[System.Net.ServicePointManager]::SecurityProtocol = [System.Net.SecurityProtocolType]::SystemDefault; & import-module -name 'C:\ProgramData\chocolatey\helpers\chocolateyInstaller.psm1'; & 'C:\ProgramData\chocolatey\helpers\chocolateyScriptRunner.ps1' -packageScript 'C:\ProgramData\chocolatey\lib\pdk\tools\chocolateyinstall.ps1' -installArguments '' -packageParameters '' -preRunHookScripts $null -postRunHookScripts $null']
    default: Redirecting System.Management.Automation.resources, Version=3.0.0.0, Culture=en-US, PublicKeyToken=31bf3856ad364e35, requested by ''
    default: Host version is 5.1.17763.1, PowerShell Version is '5.1.17763.3770' and CLR Version is '4.0.30319.42000'.
    default: VERBOSE: Exporting function 'Format-FileSize'.
    default: VERBOSE: Exporting function 'Get-ChecksumValid'.
    default: VERBOSE: Exporting function 'Get-ChocolateyConfigValue'.
    default: VERBOSE: Exporting function 'Get-ChocolateyPath'.
    default: VERBOSE: Exporting function 'Get-ChocolateyUnzip'.
    default: VERBOSE: Exporting function 'Get-ChocolateyWebFile'.
    default: VERBOSE: Exporting function 'Get-EnvironmentVariable'.
    default: VERBOSE: Exporting function 'Get-EnvironmentVariableNames'.
    default: VERBOSE: Exporting function 'Get-FtpFile'.
    default: VERBOSE: Exporting function 'Get-OSArchitectureWidth'.
    default: VERBOSE: Exporting function 'Get-PackageParameters'.
    default: VERBOSE: Exporting function 'Get-PackageParametersBuiltIn'.
    default: VERBOSE: Exporting function 'Get-ToolsLocation'.
    default: VERBOSE: Exporting function 'Get-UACEnabled'.
    default: VERBOSE: Exporting function 'Get-UninstallRegistryKey'.
    default: VERBOSE: Exporting function 'Get-VirusCheckValid'.
    default: VERBOSE: Exporting function 'Get-WebFile'.
    default: VERBOSE: Exporting function 'Get-WebFileName'.
    default: VERBOSE: Exporting function 'Get-WebHeaders'.
    default: VERBOSE: Exporting function 'Install-BinFile'.
    default: VERBOSE: Exporting function 'Install-ChocolateyEnvironmentVariable'.
    default: VERBOSE: Exporting function 'Install-ChocolateyExplorerMenuItem'.
    default: VERBOSE: Exporting function 'Install-ChocolateyFileAssociation'.
    default: VERBOSE: Exporting function 'Install-ChocolateyInstallPackage'.
    default: VERBOSE: Exporting function 'Install-ChocolateyPackage'.
    default: VERBOSE: Exporting function 'Install-ChocolateyPath'.
    default: VERBOSE: Exporting function 'Install-ChocolateyPinnedTaskBarItem'.
    default: VERBOSE: Exporting function 'Install-ChocolateyPowershellCommand'.
    default: VERBOSE: Exporting function 'Install-ChocolateyShortcut'.
    default: VERBOSE: Exporting function 'Install-ChocolateyVsixPackage'.
    default: VERBOSE: Exporting function 'Install-ChocolateyZipPackage'.
    default: VERBOSE: Exporting function 'Install-Vsix'.
    default: VERBOSE: Exporting function 'Set-EnvironmentVariable'.
    default: VERBOSE: Exporting function 'Set-PowerShellExitCode'.
    default: VERBOSE: Exporting function 'Start-ChocolateyProcessAsAdmin'.
    default: VERBOSE: Exporting function 'Test-ProcessAdminRights'.
    default: VERBOSE: Exporting function 'Uninstall-BinFile'.
    default: VERBOSE: Exporting function 'Uninstall-ChocolateyEnvironmentVariable'.
    default: VERBOSE: Exporting function 'Uninstall-ChocolateyPackage'.
    default: VERBOSE: Exporting function 'Uninstall-ChocolateyZipPackage'.
    default: VERBOSE: Exporting function 'Update-SessionEnvironment'.
    default: VERBOSE: Exporting function 'Write-FunctionCallLogMessage'.
    default: VERBOSE: Exporting alias 'Get-ProcessorBits'.
    default: VERBOSE: Exporting alias 'Get-OSBitness'.
    default: VERBOSE: Exporting alias 'Get-InstallRegistryKey'.
    default: VERBOSE: Exporting alias 'Generate-BinFile'.
    default: VERBOSE: Exporting alias 'Add-BinFile'.
    default: VERBOSE: Exporting alias 'Start-ChocolateyProcess'.
    default: VERBOSE: Exporting alias 'Invoke-ChocolateyProcess'.
    default: VERBOSE: Exporting alias 'Remove-BinFile'.
    default: VERBOSE: Exporting alias 'refreshenv'.
    default: Loading community extensions
    default: Importing 'C:\ProgramData\chocolatey\extensions\chocolatey-windowsupdate\chocolatey-windowsupdate.psm1'
    default: VERBOSE: Loading module from path 'C:\ProgramData\chocolatey\extensions\chocolatey-windowsupdate\chocolatey-windowsupdate.psm1'.
    default: VERBOSE: Exporting function 'Install-WindowsUpdate'.
    default: VERBOSE: Exporting function 'Test-WindowsUpdate'.
    default: VERBOSE: Importing function 'Install-WindowsUpdate'.
    default: VERBOSE: Importing function 'Test-WindowsUpdate'.
    default: VERBOSE: Exporting function 'Format-FileSize'.
    default: VERBOSE: Exporting function 'Get-ChecksumValid'.
    default: VERBOSE: Exporting function 'Get-ChocolateyConfigValue'.
    default: VERBOSE: Exporting function 'Get-ChocolateyPath'.
    default: VERBOSE: Exporting function 'Get-ChocolateyUnzip'.
    default: VERBOSE: Exporting function 'Get-ChocolateyWebFile'.
    default: VERBOSE: Exporting function 'Get-EnvironmentVariable'.
    default: VERBOSE: Exporting function 'Get-EnvironmentVariableNames'.
    default: VERBOSE: Exporting function 'Get-FtpFile'.
    default: VERBOSE: Exporting function 'Get-OSArchitectureWidth'.
    default: VERBOSE: Exporting function 'Get-PackageParameters'.
    default: VERBOSE: Exporting function 'Get-PackageParametersBuiltIn'.
    default: VERBOSE: Exporting function 'Get-ToolsLocation'.
    default: VERBOSE: Exporting function 'Get-UACEnabled'.
    default: VERBOSE: Exporting function 'Get-UninstallRegistryKey'.
    default: VERBOSE: Exporting function 'Get-VirusCheckValid'.
    default: VERBOSE: Exporting function 'Get-WebFile'.
    default: VERBOSE: Exporting function 'Get-WebFileName'.
    default: VERBOSE: Exporting function 'Get-WebHeaders'.
    default: VERBOSE: Exporting function 'Install-BinFile'.
    default: VERBOSE: Exporting function 'Install-ChocolateyEnvironmentVariable'.
    default: VERBOSE: Exporting function 'Install-ChocolateyExplorerMenuItem'.
    default: VERBOSE: Exporting function 'Install-ChocolateyFileAssociation'.
    default: VERBOSE: Exporting function 'Install-ChocolateyInstallPackage'.
    default: VERBOSE: Exporting function 'Install-ChocolateyPackage'.
    default: VERBOSE: Exporting function 'Install-ChocolateyPath'.
    default: VERBOSE: Exporting function 'Install-ChocolateyPinnedTaskBarItem'.
    default: VERBOSE: Exporting function 'Install-ChocolateyPowershellCommand'.
    default: VERBOSE: Exporting function 'Install-ChocolateyShortcut'.
    default: VERBOSE: Exporting function 'Install-ChocolateyVsixPackage'.
    default: VERBOSE: Exporting function 'Install-ChocolateyZipPackage'.
    default: VERBOSE: Exporting function 'Install-Vsix'.
    default: VERBOSE: Exporting function 'Set-EnvironmentVariable'.
    default: VERBOSE: Exporting function 'Set-PowerShellExitCode'.
    default: VERBOSE: Exporting function 'Start-ChocolateyProcessAsAdmin'.
    default: VERBOSE: Exporting function 'Test-ProcessAdminRights'.
    default: VERBOSE: Exporting function 'Uninstall-BinFile'.
    default: VERBOSE: Exporting function 'Uninstall-ChocolateyEnvironmentVariable'.
    default: VERBOSE: Exporting function 'Uninstall-ChocolateyPackage'.
    default: VERBOSE: Exporting function 'Uninstall-ChocolateyZipPackage'.
    default: VERBOSE: Exporting function 'Update-SessionEnvironment'.
    default: VERBOSE: Exporting function 'Write-FunctionCallLogMessage'.
    default: VERBOSE: Exporting function 'Install-WindowsUpdate'.
    default: VERBOSE: Exporting function 'Test-WindowsUpdate'.
    default: VERBOSE: Exporting alias 'Get-ProcessorBits'.
    default: VERBOSE: Exporting alias 'Get-OSBitness'.
    default: VERBOSE: Exporting alias 'Get-InstallRegistryKey'.
    default: VERBOSE: Exporting alias 'Generate-BinFile'.
    default: VERBOSE: Exporting alias 'Add-BinFile'.
    default: VERBOSE: Exporting alias 'Start-ChocolateyProcess'.
    default: VERBOSE: Exporting alias 'Invoke-ChocolateyProcess'.
    default: VERBOSE: Exporting alias 'Remove-BinFile'.
    default: VERBOSE: Exporting alias 'refreshenv'.
    default: VERBOSE: Importing function 'Format-FileSize'.
    default: VERBOSE: Importing function 'Get-ChecksumValid'.
    default: VERBOSE: Importing function 'Get-ChocolateyConfigValue'.
    default: VERBOSE: Importing function 'Get-ChocolateyPath'.
    default: VERBOSE: Importing function 'Get-ChocolateyUnzip'.
    default: VERBOSE: Importing function 'Get-ChocolateyWebFile'.
    default: VERBOSE: Importing function 'Get-EnvironmentVariable'.
    default: VERBOSE: Importing function 'Get-EnvironmentVariableNames'.
    default: VERBOSE: Importing function 'Get-FtpFile'.
    default: VERBOSE: Importing function 'Get-OSArchitectureWidth'.
    default: VERBOSE: Importing function 'Get-PackageParameters'.
    default: VERBOSE: Importing function 'Get-PackageParametersBuiltIn'.
    default: VERBOSE: Importing function 'Get-ToolsLocation'.
    default: VERBOSE: Importing function 'Get-UACEnabled'.
    default: VERBOSE: Importing function 'Get-UninstallRegistryKey'.
    default: VERBOSE: Importing function 'Get-VirusCheckValid'.
    default: VERBOSE: Importing function 'Get-WebFile'.
    default: VERBOSE: Importing function 'Get-WebFileName'.
    default: VERBOSE: Importing function 'Get-WebHeaders'.
    default: VERBOSE: Importing function 'Install-BinFile'.
    default: VERBOSE: Importing function 'Install-ChocolateyEnvironmentVariable'.
    default: VERBOSE: Importing function 'Install-ChocolateyExplorerMenuItem'.
    default: VERBOSE: Importing function 'Install-ChocolateyFileAssociation'.
    default: VERBOSE: Importing function 'Install-ChocolateyInstallPackage'.
    default: VERBOSE: Importing function 'Install-ChocolateyPackage'.
    default: VERBOSE: Importing function 'Install-ChocolateyPath'.
    default: VERBOSE: Importing function 'Install-ChocolateyPinnedTaskBarItem'.
    default: VERBOSE: Importing function 'Install-ChocolateyPowershellCommand'.
    default: VERBOSE: Importing function 'Install-ChocolateyShortcut'.
    default: VERBOSE: Importing function 'Install-ChocolateyVsixPackage'.
    default: VERBOSE: Importing function 'Install-ChocolateyZipPackage'.
    default: VERBOSE: Importing function 'Install-Vsix'.
    default: VERBOSE: Importing function 'Install-WindowsUpdate'.
    default: VERBOSE: Importing function 'Set-EnvironmentVariable'.
    default: VERBOSE: Importing function 'Set-PowerShellExitCode'.
    default: VERBOSE: Importing function 'Start-ChocolateyProcessAsAdmin'.
    default: VERBOSE: Importing function 'Test-ProcessAdminRights'.
    default: VERBOSE: Importing function 'Test-WindowsUpdate'.
    default: VERBOSE: Importing function 'Uninstall-BinFile'.
    default: VERBOSE: Importing function 'Uninstall-ChocolateyEnvironmentVariable'.
    default: VERBOSE: Importing function 'Uninstall-ChocolateyPackage'.
    default: VERBOSE: Importing function 'Uninstall-ChocolateyZipPackage'.
    default: VERBOSE: Importing function 'Update-SessionEnvironment'.
    default: VERBOSE: Importing function 'Write-FunctionCallLogMessage'.
    default: VERBOSE: Importing alias 'Add-BinFile'.
    default: VERBOSE: Importing alias 'Generate-BinFile'.
    default: VERBOSE: Importing alias 'Get-InstallRegistryKey'.
    default: VERBOSE: Importing alias 'Get-OSBitness'.
    default: VERBOSE: Importing alias 'Get-ProcessorBits'.
    default: VERBOSE: Importing alias 'Invoke-ChocolateyProcess'.
    default: VERBOSE: Importing alias 'refreshenv'.
    default: VERBOSE: Importing alias 'Remove-BinFile'.
    default: VERBOSE: Importing alias 'Start-ChocolateyProcess'.
    default: ---------------------------Script Execution---------------------------
    default: Running 'ChocolateyScriptRunner' for pdk v3.0.1.3 with packageScript 'C:\ProgramData\chocolatey\lib\pdk\tools\chocolateyinstall.ps1', packageFolder:'C:\ProgramData\chocolatey\lib\pdk', installArguments: '', packageParameters: '', preRunHookScripts: '', postRunHookScripts: '',
    default: Running package script 'C:\ProgramData\chocolatey\lib\pdk\tools\chocolateyinstall.ps1'
    default: Running Install-ChocolateyPackage -url '' -url64bit 'https://downloads.puppet.com/windows/puppet8/pdk-3.0.1.3-x64.msi' -checksum '' -checksum64 '301d9930c563c662f170b9b166390750ac2fd7193fe391e2a07ac39a64a051e7' -silentArgs '/qn /norestart' -packageName 'pdk' -checksumType 'sha256' -fileType 'MSI' -validExitCodes '0 3010 1641' -checksumType64 'sha256'
    default: Running Get-ChocolateyWebFile -packageName 'pdk' -fileFullPath 'C:\Users\vagrant\AppData\Local\Temp\chocolatey\pdk\3.0.1.3\pdkInstall.MSI' -url '' -url64bit 'https://downloads.puppet.com/windows/puppet8/pdk-3.0.1.3-x64.msi' -checksum '' -checksumType 'sha256' -checksum64 '301d9930c563c662f170b9b166390750ac2fd7193fe391e2a07ac39a64a051e7' -checksumType64 'sha256' -options 'System.Collections.Hashtable' -getOriginalFileName 'True'
    default: Running Get-OSArchitectureWidth -compare '64'
    default: CPU is 64 bit
    default: Setting url to 'https://downloads.puppet.com/windows/puppet8/pdk-3.0.1.3-x64.msi' and bitPackage to 64
    default: Running Get-WebFileName -url 'https://downloads.puppet.com/windows/puppet8/pdk-3.0.1.3-x64.msi' -defaultName 'pdkInstall.MSI'
    default: Using response url to determine file name. 'https://downloads.puppet.com/windows/puppet8/pdk-3.0.1.3-x64.msi'
    default: File name determined from url is 'pdk-3.0.1.3-x64.msi'
    default: Running Get-WebHeaders -url 'https://downloads.puppet.com/windows/puppet8/pdk-3.0.1.3-x64.msi' -ErrorAction 'Stop'
    default: Setting the UserAgent to 'chocolatey command line'
    default: Request Headers:
    default:   'Accept':'*/*'
    default:   'User-Agent':'chocolatey command line'
    default: Response Headers:
    default:   'Connection':'keep-alive'
    default:   'X-Cache':'Miss from cloudfront'
    default:   'X-Amz-Cf-Pop':'LHR61-P4'
    default:   'X-Amz-Cf-Id':'oElAEb98XOuEhzB-ip06dsUg1RAb3UWbyZ43O5A9sZKMnCBvaZF9yQ=='
    default:   'Content-Length':'157721600'
    default:   'Content-Type':'application/x-msi'
    default:   'Date':'Mon, 11 Mar 2024 15:40:20 GMT'
    default:   'ETag':'"85b546ae66902bebebb95fdf5c26d381-19"'
    default:   'Last-Modified':'Wed, 13 Dec 2023 16:16:53 GMT'
    default:   'Server':'AmazonS3'
    default:   'Via':'1.1 bba99a59a85c763f7dd5d6e519a3dfbc.cloudfront.net (CloudFront)'
    default: Downloading pdk 64 bit
    default:   from 'https://downloads.puppet.com/windows/puppet8/pdk-3.0.1.3-x64.msi'
    default: Running Get-WebFile -url 'https://downloads.puppet.com/windows/puppet8/pdk-3.0.1.3-x64.msi' -fileName 'C:\Users\vagrant\AppData\Local\Temp\chocolatey\pdk\3.0.1.3\pdk-3.0.1.3-x64.msi' -options 'System.Collections.Hashtable'
    default: Setting request timeout to  30000
    default: Setting read/write timeout to  2700000
    default: Setting the UserAgent to 'chocolatey command line'
    default: Downloading https://downloads.puppet.com/windows/puppet8/pdk-3.0.1.3-x64.msi to C:\Users\vagrant\AppData\Local\Temp\chocolatey\pdk\3.0.1.3\pdk-3.0.1.3-x64.msi
    default: 
    default: Download of pdk-3.0.1.3-x64.msi (150.42 MB) completed.
    default: No runtime virus checking built into FOSS Chocolatey. Check out Pro/Business - https://chocolatey.org/compare
    default: Verifying package provided checksum of '301d9930c563c662f170b9b166390750ac2fd7193fe391e2a07ac39a64a051e7' for 'C:\Users\vagrant\AppData\Local\Temp\chocolatey\pdk\3.0.1.3\pdk-3.0.1.3-x64.msi'.
    default: Running Get-ChecksumValid -file 'C:\Users\vagrant\AppData\Local\Temp\chocolatey\pdk\3.0.1.3\pdk-3.0.1.3-x64.msi' -checksum '301d9930c563c662f170b9b166390750ac2fd7193fe391e2a07ac39a64a051e7' -checksumType 'sha256' -originalUrl 'https://downloads.puppet.com/windows/puppet8/pdk-3.0.1.3-x64.msi'
    default: checksum.exe found at 'C:\ProgramData\chocolatey\helpers\..\tools\checksum.exe'
    default: Executing command ['C:\ProgramData\chocolatey\helpers\..\tools\checksum.exe' -c="301d9930c563c662f170b9b166390750ac2fd7193fe391e2a07ac39a64a051e7" -t="sha256" -f="C:\Users\vagrant\AppData\Local\Temp\chocolatey\pdk\3.0.1.3\pdk-3.0.1.3-x64.msi"]
    default: Hashes match.
    default: Command ['C:\ProgramData\chocolatey\helpers\..\tools\checksum.exe' -c="301d9930c563c662f170b9b166390750ac2fd7193fe391e2a07ac39a64a051e7" -t="sha256" -f="C:\Users\vagrant\AppData\Local\Temp\chocolatey\pdk\3.0.1.3\pdk-3.0.1.3-x64.msi"] exited with '0'.
    default: Running Install-ChocolateyInstallPackage -packageName 'pdk' -fileType 'MSI' -silentArgs '/qn /norestart' -file 'C:\Users\vagrant\AppData\Local\Temp\chocolatey\pdk\3.0.1.3\pdk-3.0.1.3-x64.msi' -validExitCodes '0 3010 1641' -useOnlyPackageSilentArguments 'False'
    default: Running Get-OSArchitectureWidth -compare '32'
    default: Installing pdk...
    default: Running Start-ChocolateyProcessAsAdmin -validExitCodes '0 3010 1641' -workingDirectory 'C:\Users\vagrant\AppData\Local\Temp\chocolatey\pdk\3.0.1.3' -statements '/i "C:\Users\vagrant\AppData\Local\Temp\chocolatey\pdk\3.0.1.3\pdk-3.0.1.3-x64.msi" /qn /norestart ' -exeToRun 'C:\Windows\System32\msiexec.exe'
    default: Test-ProcessAdminRights: returning True
    default: Elevating permissions and running ["C:\Windows\System32\msiexec.exe" /i "C:\Users\vagrant\AppData\Local\Temp\chocolatey\pdk\3.0.1.3\pdk-3.0.1.3-x64.msi" /qn /norestart ]. This may take a while, depending on the statements.
```

Then there was a wait for 5 or 10 minutes, and then:

```bash
 default: Command ["C:\Windows\System32\msiexec.exe" /i "C:\Users\vagrant\AppData\Local\Temp\chocolatey\pdk\3.0.1.3\pdk-3.0.1.3-x64.msi" /qn /norestart ] exited with '0'.
    default: Finishing 'Start-ChocolateyProcessAsAdmin'
    default: pdk has been installed.
    default: ----------------------------------------------------------------------
    default: Built-in PowerShell host called with ['[System.Threading.Thread]::CurrentThread.CurrentCulture = '';[System.Threading.Thread]::CurrentThread.CurrentUICulture = '';[System.Net.ServicePointManager]::SecurityProtocol = [System.Net.SecurityProtocolType]::SystemDefault; & import-module -name 'C:\ProgramData\chocolatey\helpers\chocolateyInstaller.psm1'; & 'C:\ProgramData\chocolatey\helpers\chocolateyScriptRunner.ps1' -packageScript 'C:\ProgramData\chocolatey\lib\pdk\tools\chocolateyinstall.ps1' -installArguments '' -packageParameters '' -preRunHookScripts $null -postRunHookScripts $null'] exited with '0'.
    default: Calling command ['"C:\Windows\System32\shutdown.exe" /a']
    default: Command ['"C:\Windows\System32\shutdown.exe" /a'] exited with '1116'
    default:   pdk may be able to be automatically uninstalled.
    default: Environment Vars (like PATH) have changed. Close/reopen your shell to
    default:  see the changes (or in powershell/cmd.exe just type `refreshenv`).
    default: The following values have been added/changed (may contain sensitive data):
    default:   * Path='C:\Windows\system32;C:\Windows;C:\Windows\System32\Wbem;C:\Windows\System32\WindowsPowerShell\v1.0\;C:\Windows\System32\OpenSSH\;C:\ProgramData\chocolatey\bin;C:\Program Files\Puppet Labs\DevelopmentKit\bin' (Machine)
    default: Capturing package files in 'C:\ProgramData\chocolatey\lib\pdk'
    default:  Found 'C:\ProgramData\chocolatey\lib\pdk\pdk.nupkg'
    default:   with checksum '3F54B34D6E7C60723D5C91BABF9504F4'
    default:  Found 'C:\ProgramData\chocolatey\lib\pdk\pdk.nuspec'
    default:   with checksum '354434ABB5CE5BBA6D88459BD0F8E7E3'
    default:  Found 'C:\ProgramData\chocolatey\lib\pdk\tools\chocolateyinstall.ps1'
    default:   with checksum '2B785025A5A8F32096AC38E7E20E7B7C'
    default:  Found 'C:\ProgramData\chocolatey\lib\pdk\tools\LICENSE.md'
    default:   with checksum '87CBA64D0CA5A3ED4C15D4679A6B928A'
    default:  Found 'C:\ProgramData\chocolatey\lib\pdk\tools\VERIFICATION.md'
    default:   with checksum '3641D9461CFDF3218B46B940C8C24D78'
    default: Attempting to create directory "C:\ProgramData\chocolatey\.chocolatey\pdk.3.0.1.3".
    default: There was no original file at 'C:\ProgramData\chocolatey\.chocolatey\pdk.3.0.1.3\.registry'
    default: There was no original file at 'C:\ProgramData\chocolatey\.chocolatey\pdk.3.0.1.3\.files'
    default: Attempting to delete file "C:\ProgramData\chocolatey\.chocolatey\pdk.3.0.1.3\.extra".
    default: Attempting to delete file "C:\ProgramData\chocolatey\.chocolatey\pdk.3.0.1.3\.version".
    default: Attempting to delete file "C:\ProgramData\chocolatey\.chocolatey\pdk.3.0.1.3\.sxs".
    default: Attempting to delete file "C:\ProgramData\chocolatey\.chocolatey\pdk.3.0.1.3\.pin".
    default: Sending message 'HandlePackageResultCompletedMessage' out if there are subscribers...
    default: Attempting to delete file "C:\ProgramData\chocolatey\lib\pdk\.chocolateyPending".
    default:  The install of pdk was successful.
    default:   Software installed to 'C:\Program Files\Puppet Labs\DevelopmentKit\'
    default: 
    default: Chocolatey installed 1/1 packages.
    default:  See the log for details (C:\ProgramData\chocolatey\logs\chocolatey.log).
    default: Sending message 'PostRunMessage' out if there are subscribers...
    default: Exiting with 0
    default: Exit code was 0
gavin.didrichsen@Gavins-MacBook-Pro chocolatey-test-environment % 
```