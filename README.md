# SEB Bypass Patch v3.5.0.544

By using this patch, you can use a Windows Virtual Machine [[VMware Player](https://www.vmware.com/go/getplayer-win)] for SEB and use your usual desktop for searching answers for the questions asked in the exams conducted in SEB without them knowing 😉




## How to use

1․ Download [SafeExamBrowser.Monitoring.dll](https://github.com/nxvvvv/safe-exam-browser-bypass/releases/download/v3.5.0.544/SafeExamBrowser.Monitoring.dll), [SafeExamBrowser.SystemComponents.dll](https://github.com/nxvvvv/safe-exam-browser-bypass/releases/download/v3.5.0.544/SafeExamBrowser.SystemComponents.dll) and [SafeExamBrowser.Client.exe](https://github.com/nxvvvv/safe-exam-browser-bypass/releases/download/v3.5.0.544/SafeExamBrowser.Client.exe) by clicking these.

2․ Copy these three files.

3․ Go to `C:\Program Files\SafeExamBrowser\Application`.

4․ Paste it in this folder.

5․ You'll need `admin perms` to replace files in this folder.

6․ After replacing, open SEB and voila it runs in a VM.
## Recommended

It's recommended to use VMware(Free/Paid) since we can make the VM look like a real computer by copying the host computer's (the computer in which VM is running) model and company.

**Steps to do this:**

1․ First go to the directory where you installed the Windows VM. Its the *Documents* folder by default.

2․ In *Documents* folder, there will be a folder called *Virtual Machines*.

3․ Open that folder and select the Windows VM folder and open it.

4․ Now there will be a file with extension `.vmx`. Right click that file and open with Notepad.

5․ Now paste `smbios.reflecthost = "TRUE"` in a new line like this:

![image](https://user-images.githubusercontent.com/34748927/167270852-36b89b22-bb09-4633-9040-90bc29e64f75.png)

It doesn't matter where this is pasted. Just paste it in a new line.

6․ Now save the file and voila! Your VM manufacturer and model is similar as your PC. This reduces the chance of VM detection.

# DON'T FORGET

Don't forget to edit logs if your exam invigilator asks you for your Client and Runtime logs. 

Logs Path: `C:\Users\<username>\AppData\Local\SafeExamBrowser` and put your user name in place of `<username>`

Just edit these parts:
1. In **Runtime.log** file,

        
        INFO: [DisplayMonitor] Detected 0 active displays, 1 are allowed.

to

        INFO: [DisplayMonitor] Detected 1 active displays, 1 are allowed.

2. In **Client.log** file,

        
        INFO: [WirelessAdapter] Wireless networks cannot be monitored, as there is no hardware adapter available or it is turned off.

to

        INFO: [WirelessAdapter] Started monitoring the wireless network adapter.


## Follow these steps if you're using v2.4 of SEB
These `.dll` files are absent in the legacy version i.e. v2.4 of SEB.

Basically this version of SEB doesn't check if you're using VM or not. So it's pretty easy to use it.

But, if they ask for logs, you need to delete these lines from the log file[Logs Path: `C:\Users\<username>\AppData\Roaming\SafeExamBrowser`  and put your user name in place of `<username>`]:

1. Open **Sebclient.log**

2. Now press Ctrl key and F key.

3. Now paste `vm3dservice` there and click find.

4. Now delete the lines where this is shown.

5. Do the same for `VGAuthService` and `vmtoolsd` and delete those lines.

6. We did this to prevent the invigilators from detecting a VM since VMware tools run these services and if they see these services, you may get disqualified. Also this only works for VMware.

## License
[![FOSSA Status](https://app.fossa.com/api/projects/git%2Bgithub.com%2Fnxvvvv%2Fsafe-exam-browser-bypass.svg?type=small)](https://app.fossa.com/projects/git%2Bgithub.com%2Fnxvvvv%2Fsafe-exam-browser-bypass?ref=badge_small)

<details>
  <summary>Note</summary>

## Patches Disclaimer

The patches included in this repository were created by [nxvvvv](https://github.com/nxvvvv). However, it's important to note the following:

- The Safe Exam Browser itself is not developed or owned by [nxvvvv](https://github.com/nxvvvv).
- The patches provided in this repository are modifications made by [nxvvvv](https://github.com/nxvvvv) and are licensed separately.

## Patch Licensing

The patches in this repository are licensed under AGPL-3.0. You can find the details of the license in the [LICENSE](./LICENSE) file.

## Original Safe Exam Browser

The original Safe Exam Browser is not affiliated with [nxvvvv](https://github.com/nxvvvv), and its development and ownership are separate entities. For information about the original Safe Exam Browser, please refer to their official [repository](https://github.com/SafeExamBrowser/seb-win-refactoring).

</details>
