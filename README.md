# FOR DUAL-BOOT

# Removing Ubuntu and Adding Again

## Step 1: Remove Ubuntu Partitions Using Disk Management

1. **Open Disk Management:**
   - Press `Win + X` and select "Disk Management".

2. **Identify Ubuntu Partitions:**
   - Look for partitions that are not used by Windows (e.g., EXT4, Linux Swap).

3. **Delete Ubuntu Partitions:**
   - Right-click on each Ubuntu-related partition and select "Delete Volume".

4. **Create Unallocated Space:**
   - This will create unallocated space which can be used for the new Linux installation.

## Step 2: Remove GRUB Bootloader Entries

1. **Boot into Windows**.

2. **Open Command Prompt as Administrator**.

3. **List and Remove the Unwanted Ubuntu Boot Entry:**
   ```
   shell
   bcdedit /enum firmware
   bcdedit /delete {identifier}  # Replace {identifier} with the GUID of the Ubuntu entry
   ```

4. **Identify and Select the EFI Partition:**
    ```
    diskpart
    list disk
    select disk 0  # Replace with the correct disk number if necessary
    list partition
    select partition 1  # Ensure this is the EFI System Partition
    assign letter=Z
    exit
    ```

5. **Navigate to the EFI Partition and Remove the Ubuntu Boot Directory:**
    ```
    Z:
    cd EFI
    rmdir /S /Q ubuntu
    ```
 
6. **Restart Your PC and check the BIOS boot list to ensure the Ubuntu entry is gone.**

## Step 3: Installing Ubuntu 24.04

1. **Create a Bootable USB Drive:**

- Use a tool like Rufus or Balena Etcher to create a bootable USB with the Ubuntu 24.04 ISO.
- Insert the Bootable USB and Restart Your PC.

2. **Access the Boot Menu:**

- Press the appropriate key (e.g., F12, Esc, F2, F10, Del) to access the boot menu.
- Select the Bootable USB Drive.

3. **Follow On-Screen Instructions to Install Ubuntu:**

- Automatic Install: Easiest way (Not need to worry about allocating swap sectors / boot sectors)
- Manual Install: Choose to install Ubuntu on the unallocated space created earlier.
- Single Boot (Ubuntu): Select the option to erase the disk and install Ubuntu if you want to completely overwrite previous partitions.
- Completion: Remove the bootable USB and restart your PC.

By following these steps, you will successfully remove the old Ubuntu installation, clean up the boot entries, and install a fresh copy of Ubuntu 24.04.

## Step 4: Making the Bootable USB Normal Again!

1. **Via Windows**
- Insert the USB while in windows 
- You can see that windows FS is not able to read the USB as it's in ext FS (Linux ISO) and it can read GPT (NTFS, FAT32 etc.) FS.
- Follow the commands
    ```
    Open CMD in Administrator Mode
    diskpart
    list disk
    select disk [usb sl no.]
    clean
    convert gpt
    clean
    create partition primary
    select partition 1
    active
    format fs=fat32 quick or format fs=ntfs quick
    assign
    exit
    ```
- NB: You may face errors like "access denied", "don't have permission", "unknown capacity", "cannot format" etc. But this method is able to format the USB and revert it back to normal.


