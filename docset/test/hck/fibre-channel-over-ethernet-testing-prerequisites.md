---
author: joshbax-msft
title: Fibre Channel Over Ethernet Testing Prerequisites
description: Fibre Channel Over Ethernet Testing Prerequisites
MSHAttr:
- 'PreferredSiteName:MSDN'
- 'PreferredLib:/library/windows/hardware'
ms.assetid: e70686e6-ef60-4d37-8580-a06738a60b51
---

# Fibre Channel Over Ethernet Testing Prerequisites


This section describes the tasks that you must complete before you test a Fibre Channel over Ethernet (FCoE) controller by using the Windows Hardware Certification Kit (Windows HCK):

-   [Review the hardware requirements](#bkmk-hardwarerequirements).

-   [Review the software requirements](#bkmk-softwarerequirements).

-   [Configure the test computer](#bkmk-configure).

## <a href="" id="bkmk-hardwarerequirements"></a>Hardware Requirements


The following hardware is required for testing an FCoE controller. You might need additional hardware if the test device offers other features. To determine whether additional hardware requirements apply, see the test description for each test that appears for the device in Windows HCK Studio.

**Note**  
With the exception of the test computer and the test controller, all hardware involved in the test must already have a logo.

 

-   Two test computers. The test computers must meet the Windows HCK requirements as described in [Windows HCK Prerequisites](windows-hck-prerequisites.md). and the following operating system-specific requirements.

    -   For testing on Windows 8, Windows 7, Windows Vista or Windows XP:

        -   One dual-core or equivalent processor

        -   4 GB of memory

    -   For testing on Windows Server® 2012, Windows Server 2008 R2, Windows Server 2008, or Windows Server 2003:

        -   One quad-core or equivalent processor

        -   6 GB of memory

-   Two identical PCI-X or PCIe-based dual-port FCoE adapters (the test devices). Four identical FCoE adapters are needed if the adapter is single-ported.

-   One FCF (FCoE switch) that supports 10G/E bandwidth for FCoE traffic. FCF is installed with four available FCoE ports/SFP+, and one FC port/SFP+.

-   Copper- or fibre-based FCoE cable and Fibre Channel cable for the above ports.

-   One Fibre Channel storage array system that can support any one of the following: RAID-0, RAID-1, RAID-5, RAID-10, or RAID-6.

-   If the FCoE adapter is not bootable, one on-board SATA or SAS controller.

-   If the FCoE adapter is not bootable, one SATA or SAS disk drive for operating system installation.

**Note**  
To certify your product for use on servers, the test computer must support four processors and a minimum of 1 GB of RAM. These system capabilities are required to test the Rebalance, D3 State, and Multiple Processor Group functionality of the device and driver. You do not need a computer that actually has more than 64 processors to test your device. Additionally, the server system(s) being used for device or driver testing must have Server Core installed prior to testing. For more information see [Windows Server Installation Options](http://go.microsoft.com/fwlink/p/?LinkID=251454).

If you use a pool of test computers to test devices, at least one computer in the pool must contain four processors and a minimum of 1 GB of RAM. Additionally, that computer must contain the device and the driver that you want to test. If the driver is the same on all the computers in the pool, the system creates a schedule to run against all test computers.

For tests that do not include a driver to test, such as hard disk drive tests, the Windows HCK scheduler constrains the tests that validate the device’s and driver’s rebalance, D3 state, and multiple processor groups functionality to run on the default test computer. You must manually configure this computer to have multiple processor groups. The default computer is the first test computer in the list. Test personnel must make sure that the first test computer in the list meets the minimum hardware requirements.

 

**Note**  
Except for para-virtualization drivers (as defined by Logo Program Requirement Policy-0020), you may not use any form of virtualization when you test physical devices and their associated drivers for server certification or signature. All virtualization products do not support the underlying functionality that is required to pass the tests that relate to multiple processor groups, device power management, device PCI functionality, and other tests.

 

## <a href="" id="bkmk-softwarerequirements"></a>Software Requirements


The following software is required to test a Fibre Channel storage controller:

-   The drivers for the test device.

-   The latest Windows HCK filters or updates.

-   The current release of the Windows Driver Kit (WDK).

-   Windows symbol files. These are available from the [Symbol Files website](http://go.microsoft.com/fwlink/?LinkId=231439).

## <a href="" id="bkmk-configure"></a>Test Computer Configuration


To configure the test computer to test a Fibre Channel storage controller, follow these steps:

1.  When the test computer is turned off, complete the following assembly steps:

    1.  If the FCoE adapter is not bootable, configure an alternate SATA or SAS on-board boot controller, and then install a respective SATA or SAS hard disk drive if they are not present.

    2.  Install one FCoE adapter in the first test system. (Install two FCoE adapters in the first computer if the computer has only one port.)

    3.  Connect all ports of the FCoE adapter(s) to the FCF.

    4.  Repeat step 2 through step 4 for the second FCoE adapter in the second test system. (Install two FCoE adapters in the second computer if the computer has only one port.)

    5.  Connect the FCF to the storage array, as described in the following diagram:

        ![fibre chaneel over ethernet configuration diagram](images/hck-win8-fcoe-config.png)

    6.  Create LUN and configure LUN mapping on the storage array system. The array must contain at least 120 GB of disk space for SAN boot configuration. The LUN mapping is only needed for the first test system. Complete this step according to the storage array vendor instructions.

    7.  Configure the ports and zoning on the FCF. Complete this step according to the switch vendor instructions.

    8.  Turn on the test systems.

2.  Turn on the test computer, install the appropriate Windows operating system, install all available Windows updates, and then configure the computer for your test network. The test network is the network that contains the Windows HCK Studio and Windows HCK Controller.

3.  Install any HBA or RAID system drivers that are necessary to connect to or manage the peripheral devices.

4.  Do one of the following to install the operating system:

    1.  If the test FCoE adapter is bootable, install the operating system on an NTFS, 120 GB partition on the RAID system.

    2.  Install the operating system on an NTFS-formatted 120 GB partition on the SATA or SAS hard disk drive that is connected to the alternate SATA or SAS boot controller in the test system.

    **Warning**  
    Do not follow this step when you test MPIO.

     

5.  Start the test system in the Microsoft Windows operating system.

6.  Configure MPIO for the first test system only:

    -   Install the Windows MPIO feature through Server Manager.

    -   Reboot the system.

    -   Use the mpclaim.exe command line utility or edit the MPIO CPL (Control Panel) GUI interface to add the LUN ID to be claimed.

    -   Restart the system.

7.  Complete the following procedure to set the system page file and enable crashdump:

    1.  Click the **Start** button, right-click **My Computer**, and then click **Properties.**

    2.  Click the **General** tab, and then note the amount of RAM that the computer contains.

    3.  Click the **Advanced** tab (or click **Advanced system settings** in the left pane for Windows Vista, Windows 7, Windows 8, Windows Server 2008, Windows Server 2008 R2 or Windows Server® 2012), and then, in the **Performance** area, click **Settings**.

        **Note**  
        If you are prompted to enter administrative credentials or allow the action, enter the credentials or allow the action.

         

    4.  Click the **Advanced** tab, and then, in the **Virtual Memory** area, click **Change**.

    5.  Select **Custom Size**, and then enter a number in the **Initial size (MB)** box that is larger than the size of RAM that you noted in step b.

    6.  In the **Maximum size (MB)** text box, enter a maximum size value that is larger than the initial size that you entered in the **Initial size (MB)** box. (The maximum size is typically 1.5 to 2 times the initial size.)

    7.  Click **Set**, and then click **OK** two times.

    8.  Click **OK**, and then restart the computer to update the page file size.

8.  Copy the Windows symbol files to %SystemDrive%\\Symbols.

9.  Verify that Windows can access the storage array over the FCoE adapter.

10. Install the Windows HCK client application on the test computer.

11. Use Windows HCK Studio to create a computer pool, and then move the test computer to that pool.

Make sure that the test computer is in the ready state before you begin your testing. If a test requires parameters to be set before it is run, a dialog box appears for that test. Review the specific test topic for more information.

Some Windows HCK tests require user intervention. When you run tests for a submission, it is a best practice to run the automated tests in a block separately from manual tests. This prevents a manual test from interrupting completion of an automated test.

**Warning**  
When testing storage devices, we strongly recommend that you complete all Device Fundamentals tests before starting storage tests. Storage tests will reconfigure your test device, leaving the device in a state unsuitable to support Device Fundamentals tests. The following configurations provide steps to create volume on the storage test device. This is important to complete the Device Fundamental part of testing (DevFund).

 

 

 

[Send comments about this topic to Microsoft](mailto:wsddocfb@microsoft.com?subject=Documentation%20feedback%20%5Bp_hck\p_hck%5D:%20Fibre%20Channel%20Over%20Ethernet%20Testing%20Prerequisites%20%20RELEASE:%20%284/27/2016%29&body=%0A%0APRIVACY%20STATEMENT%0A%0AWe%20use%20your%20feedback%20to%20improve%20the%20documentation.%20We%20don't%20use%20your%20email%20address%20for%20any%20other%20purpose,%20and%20we'll%20remove%20your%20email%20address%20from%20our%20system%20after%20the%20issue%20that%20you're%20reporting%20is%20fixed.%20While%20we're%20working%20to%20fix%20this%20issue,%20we%20might%20send%20you%20an%20email%20message%20to%20ask%20for%20more%20info.%20Later,%20we%20might%20also%20send%20you%20an%20email%20message%20to%20let%20you%20know%20that%20we've%20addressed%20your%20feedback.%0A%0AFor%20more%20info%20about%20Microsoft's%20privacy%20policy,%20see%20http://privacy.microsoft.com/default.aspx. "Send comments about this topic to Microsoft")



