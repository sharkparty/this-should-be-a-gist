
# Running API on VMWare Fusion with OSX

This documentation has been authored to assist in bridging the gap between running the Rx Saver API in Visual Studio on Windows 10 and being able to access that API via OSX. 

### VMWare Configuration

1. Download and install [VMWare Fusion 10](https://www.vmware.com/products/fusion/fusion-evaluation.html). Run through the installer.
1. Download and install [Windows 10](https://www.microsoft.com/en-us/software-download/windows10ISO).
1. Create a new VMWare Fusion instance from Windows 10 (HD: 60GB, RAM: 6GB, CPU: 4 Cores, NAT [Share With My Mac] Networking)
1. Download and install [Visual Studio 2017](https://www.visualstudio.com/downloads/).

### Cloning the Repo

1. Sign in to the Live account that has been granted access to RX Saver's Team Foundation Server (TFS) instance (request this access from Rustin Reese or Shaun Dubuque).
1. Clone the `api3.lowestmed.com` repo (this should automatically run `nuget restore` which restores your project dependencies)
1. Open the `api3.lowestmed.com` solution.

### Running The Project Locally

1. From the Solution Explorer Window (View > Solution Explorer), toggle the 'Solutions and Folders' view (4th icon from the top left)
1. Open 'Web' and then right click on `api3.lowestmed.com` and select 'Set as StartUp Project', which will change the default Run command to point to this project.
1. As well, right click on that project and go to 'Project Settings' then navigate to the 'Web' Tab
1. Select the 'Override Application Root URL' checkbox and add the following URL: `http://windows:9999/`
1. From the Solution Explorer, toggle the 'Solutions and Folders' view back to folders and then click the 'Show All Files' icon (top, 3rd icon from the right)
1. Open up **api3.lowestmed.com > Local Settings (.vs) > config > applicationhost.config** and scroll to the bindings configuration (~line:169) and add the following entry: `<binding protocol="http" bindingInformation="*:9999:windows" />`
1. Then run the project (Ctrl+F5)

### Configuring Windows to talk to Mac
1. Disable your Windows Firewall (all of them)
2. Open up the external port by running the following command in PowerShell as Administrator (right click on PowerShell in the start menu > Run as Administrator): `netsh http add urlacl url=http://windows:9999/ user=everyone`


### Configuring Mac to talk to Windows
1. Get the IP Address of your Windows VM from terminal: `vmrun list` and then `vmrun getGuestIPAddress "YOUR/MACHINE/PATH"` Or, open command prompt on your running container and run the following command: `ipconfig /all` then look for the `ipv4` or `(preferred)` IP Address
2. Edit your host file to include an entry for your Windows VM: `sudo nano /etc/hosts` by adding the following line: `[YOUR.VM.IP.ADDRESS]       windows`
3. Save your host file
4. Open VMWare's network configuration file `sudo nano /Library/Preferences/VMware Fusion/vmnet8/nat.conf` and look for the `incomingtcp` section and add the following lines: `9999 = [YOUR.VM.IP.ADDRESS]:9999`
5. Save that file
6. Restart your VM network controller: `sudo /Applications/VMware Fusion.app/Contents/Library/vmnet-cli --stop` and then `sudo /Applications/VMware Fusion.app/Contents/Library/vmnet-cli --start`

### Curl The API from your Mac
1. Curl the following to test connectivity: `curl -X GET  http://windows:9999/api/drugs/all  -H 'lm-application-key: Test Key' -H 'lm-application-name: Test'`
1. Expect a 200 OK or send angry requests for help to Kameron Sween

