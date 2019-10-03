# Workspace VM Setup

This guide helps in setting up the Workspace VM, running in Lubuntu.
The resulting VM would be ideal for software development work.

---

## Prerequisites

* Dedicated resources for the VM:
  * At least 2GB of RAM; recommended 4GB of RAM
  * At least 2 physical cores of CPU; recommended 4 cores
* Installed VirtualBox (get one [here](https://www.virtualbox.org/wiki/Downloads)), with Extensions pack installed (get one [here](https://www.virtualbox.org/wiki/Downloads))
* Latest copy of Lubuntu, in ISO format (get one [here](https://lubuntu.net/downloads/))
* A 100GB virtual disk file, in VHD format (learn to create one [here](../../tutorial/create-a-virtual-disk/readme.md))

---

## 1. Creating and Configuring the VM in VirtualBox

1. Open `VirtualBox`.
2. Press `Ctrl + N`.
3. Provide the desired name, and base location of the VM.
4. Select the following options:
  * `Type`: `Linux`
  * `Version`: `Ubuntu (64bit)`
  * `Memory Size`: `4096 MB` (or the `MB` equivalent available RAM to dedicate)
  * `Hard disk`: `Use an existing virtual hard disk file`; when prompted, select the virtual disk file to use.
5. Click `Create`.
6. Select the newly created VM, and press `Ctrl + S`.
7. Verify and configure the following settings:
  * `General`
    * `Advanced`
      * `Shared Clipboard`: `Bidirectional`
      * `Drag 'n Drop`: `Bidirectional`
    * `Description`: provide a concise description of the VM, if necessary
  * `System`
    * `Boot Order` (checked only)
      * `Optical`
      * `Hard Disk`
    * `Pointing Device`: `USB Tablet`
    * `Extended Features` (checked only)
      * `Enable I/O APIC`
      * `Hardware Clock in UTC Time`
    * `Processor`
      * `Execution Cap`: `100%`
      * `Extended Features` (checked only)
        * `Enable PAE/NX`
    * `Acceleration`
      * `Paravirtualization Interface`: `Default`
      * `Hardware Virtualization` (checked only)
        * `Enable VT-x/AMD-V`
        * `Enable Nested Paging`
  * `Display`
    * `Video Memory`: `128MB`
    * `Graphics Controller`: `VMSVGA`
  * `Audio`
    * `Host Audio Driver`: `Windows DirectSound`
    * `Audio Controller`: `Intel HD Audio`
    * `Extended Features` (checked only)
      * `Enable Audio Input`
      * `Enable Audio Output`
  * `Network` (enabled only)
    * `Network 1`
      * `Attached to`: `NAT`
    * `Network 2`
      * `Attached to`: `Host-only Adapter`
      * `Name`: `VirtualBox Host-Only Ethernet Adaptor`
      * `Promiscuous Mode`: `Allow VMs`
  * `USB`
    * `Enable USB Contoller`: `USB 3.0 (xHCI) Controller`
8. Click `OK`, for the changes to take effect.

---

# 2. Installing the Guest OS into the Virtual Disk

1. Select the newly created VM, and press `Ctrl + S`.
2. Go to `Storage` > `Controller: IDE`, and select an empty CD-ROM drive.
3. Go to `Attributes`, click on the CD icon, and select `Choose Virtual Optical Disk file`. When prompted, go and select the Lubuntu ISO file.
  * Or if the Lubuntu ISO has been previously mounted, select it instead.
4. Click `OK`.
5. Double-click the VM to start it.
  * The next steps will refer to the booted environment of the Lubuntu ISO.
6. Select the desired language to use for the booted environment.
7. Select `Start Lubuntu`.
8. *(Optional)* On the Application menu, go to `Preferences` > `LXQt settings` > `Date and Time` > `Timezone` and change the desired time zone.
9. *(Optional)* On the Application menu, go to `Preferences` > `LXQt settings` > `Monitor settings` > `Resolution` and change the desired resolution.
10. On the desktop, open `Install Lubuntu vv.vv`. (`vv.vv` is the version of Lubuntu to install)
11. In the `Welcome` screen, select the desired language for the installer, then click `Next`.
12. In the `Location` screen, select the desired time zone, system language, and region formatting for the OS to install, then click `Next`.
13. In the `Keyboard` screen, select the desired keyboard layout and model, then click `Next`.
14. Refer to the [Setting up Partitions](#setting-up-partitions) section for completing the `Partitions` screen.
15. In the `Users` screen, fill in the desired details, then click `Next`.
  * If preferred, check `Log in automatically without asking for the password`.
16. In the `Summary` screen, review the installation options set up in the previous steps, then click `Install`.
  * If there are any missing or unconfigured installation options, go back to the appropriate screen to resolve those options.
17. A prompt appears to ask for confirmation of the installation. Click `Install Now`.
  * **Note:** This is the last chance the installer prompts the user to review changes. If undecided about the installation options, click `Go Back` and resolve those options.
18. Once the installation is done, on the `Finish` screen, click `Restart`.
  * The booted environment will prompt to press `Enter` to dismount the ISO and continue the restart, in order to boot from the newly installed OS. Do so.

---

## 3. Configuring post-installation of the Guest OS

1. Go to the window of the VM.
2. Go to `Devices` > `Insert Guest Additions CD image...`.
  * After a few moments, the Guest OS recognizes the mounted CD image, and prompts for action. Do nothing and close the prompt.
3. Open the Terminal.
4. Issue the following commands:
  1. `sudo mkdir /media/guest-cd` -- *create a mount point `guest-id` to access the CD image.*
  2. `sudo mount /dev/cdrom /media/guest-cd` -- *mount the CD-ROM device (containing the CD image) to the mount point previously created*
    * `mount` may issue a warning about the device being `write-protected`. This is expected, and thus ignored.
  3. `cd /media/guest-cd` -- *go to the mount point*
  4. `sudo su` -- *continue as root*
  5. `./VBoxLinuxAdditions.run` -- *run the post-installation*
  6. `cd /` -- *go to root directory*
  7. `umount /media/guest-cd` -- *dismount the CD-ROM device*
  7. ` && rm -r /media/guest-cd` -- *remove the mount point; no longer needed*
5. Close the Terminal.
6. Reboot the Guest OS.

---

## Setting up Partitions

1. For `Select Storage Device`, select the 100GB virtual disk.
  * If the 100GB virtual disk is not present, verify that it is mounted from the `Storage` screen in the settings of the VirtualBox VM.
2. Select `Manual partitioning`, then click `Next`.
3. Click `New Partition Table`, select `Master Boot Record (MBR)`, and click `OK`.
4. Create the following partitions in order, by clicking `Create`, configuring the specified parameters, and clicking `OK`:
  1. Swap partition:
    * `Size`: `8192 MiB`
    * `Filesystem`: `linuxswap`
  2. Root partition:
    * `Size`: `<remaining> MiB`
    * `Filesystem`: `ext4`
    * `Mount Point`: `/`
  3. Home partition:
    * `Size`: `48640 MiB`
    * `Filesystem`: `ext4`
    * `Mount Point`: `/home`
5. For `Install boot loader`, select `Master Boot Record of <device name>`.
6. Click `Next`.
7. Continue with the previous section.
