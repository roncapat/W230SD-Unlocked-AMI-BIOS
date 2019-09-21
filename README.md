# W230SD-Unlocked-AMI-BIOS
Modded Setup UEFI DXE for Clevo W230SD laptops - All hidden voices unlocked

Firmware version: 1.05.01\
Modded UEFI package: https://github.com/roncapat/W230SD-Unlocked-AMI-BIOS/releases/tag/1.0

W230SD motherboard is equipped with a UEFI firmware. \
In a UEFI firmware, the system setup panel is a EFI application embedded in the firmware image as a DXE driver module.\
Menus and options are described in a format called IFR.

## How to tweak the setup panel
First of all, compile UEFITool and Universal-IFR-Extractor\
https://github.com/LongSoft/UEFITool/UEFITool.git\
https://github.com/rmast/Universal-IFR-Extractor.git

Download an original copy of the UEFI update package. You can obtain one from your barebone integrator, such as Santech.\
Here's the link for my Santech T55 (Clevo W230ST Barebone) notebook:\
https://www.santech.eu/index.php?route=product/attachmanager/getfile&product_attach_file_id=1311

In the downloaded .zip file, you must locate the firmware package. In the referenced T55 package, for example, the file is called T55D_105.01. If you open it with UEFITool, you should see "UEFI Image" as the root of the main panel hierarchy.

You should now locate and extract the Setup DXE. Look for a subtree element which has "Setup" in the "Text" column. In the example file, it has GUID 899407D7-99FE-43D8-9A21-79EC328CAC21. Right-click on it, select "Extract body..." and save it as "setup.bin".

Open the Universal IFR Extractor, select "setup.bin" file and click "extract". Save the file as "setup_ifr.txt".

Open the "setup_ifr.txt". In the "Form Sets" section, there are 6 items listed: Main, Advanced, Chipset, Boot, Security, Exit.

If you look in your Clevo W230SD setup panel, you actually have 5 panels: Main, Advanced, Boot, Security, Exit, but the Chipset tab is missing. AMI PXE Setup enables/disables tabs by means of a simple byte-mask. The mask is somewhere in the ROM, and we need to change it.

Open setup.bin with an hex editor and search the hex sequence 01 01 00 01 01 01. Swap 00 with 01 and save.

If you read setup.txt in depth, you could notice that a lot of submenus are disabled by a conditional statement "Suppress If". In particular, True (hex 46 02) is hardwired in the statement arguments. To enable the correspondent options, you can toggle 46 02 with 47 02 (False).

Save the edited setup.bin, go back to UEFITool, right click on the setup DXE, "Replace body...". Select "setup.bin", then save the file as "T55D_105.01" (make sure to have a backup copy of the original image).

Now you can flash your modded firmware with the tools provided by your system integrator.
