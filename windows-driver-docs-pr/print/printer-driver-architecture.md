---
title: Printer Driver Architecture
author: windows-driver-content
description: Printer Driver Architecture
MS-HAID:
- 'drvarch\_ea28fa5f-7815-44f6-bbfa-d7d19a500411.xml'
- 'print.printer\_driver\_architecture'
MSHAttr:
- 'PreferredSiteName:MSDN'
- 'PreferredLib:/library/windows/hardware'
ms.assetid: 68a61007-8f0d-4fd4-b4a7-c8acbc101236
keywords: ["print jobs WDK , printer drivers", "jobs WDK print , printer drivers", "Windows printing architecture WDK", "printer driver architecture WDK", "printer drivers WDK , architecture"]
---

# Printer Driver Architecture


## <a href="" id="ddk-printer-driver-architecture-gg"></a>


Print jobs are created by applications through calls to Microsoft Win32 GDI or, in Windows Vista, Windows Presentation Foundation (WPF) functions. The Win32 functions spool application data as [EMF](emf-data-type.md) records for later playback by the EMF [*print processor*](https://msdn.microsoft.com/library/windows/hardware/ff556325#wdkgloss-print-processor), or they can immediately render a printable image for each document page. The WPF functions spool application data as an XPS spool file.

Prior to Windows Vista, applications communicated printer settings to the printer by using a [**DEVMODEW**](https://msdn.microsoft.com/library/windows/hardware/ff552837) structure. In Windows Vista, the Print Ticket and Print Capabilities technologies communicate printer settings so that printer settings are more compatible across printers and applications.

Image rendering, whether performed immediately or during print processing, is performed in the print driver:

-   A [GDI-based printer driver](gdi-printer-drivers.md) performs the image rendering during the playback of EMF records from the spool file and is controlled by the GDI rendering engine. During the rendering operation, the GDI rendering engine calls the appropriate Windows 2000 and later printer driver for assistance.

-   [XPSDrv print drivers](xpsdrv-printer-drivers.md) use a series of processing filters to process the XPS spool file content for output to the printer.

Windows 2000 and later GDI-based printer drivers must:

-   Assist GDI in rendering print jobs by providing printer-specific drawing capabilities that GDI cannot support.

-   Send the rendered image's data stream to the print spooler.

-   Provide a user interface to the modifiable configuration parameters that are associated with printers and print documents, such as which input and output trays are selected, the number of copies, image resolution and orientation, and so on.

XPSDrv printer drivers have the same user interface responsibility as the GDI-based drivers and are also responsible for processing the print job data and sending the data to the printer. XPSDrv printer drivers, however, do not need to use GDI to render the page images for the printer.

Windows 2000 and later printer drivers are made up of a set of [printer driver components](gdi-printer-drivers.md) that divide a driver's drawing and user interface operations into separate DLLs. XPSDrv printer drivers are also made up of components that divide the configuration and the drawing and rendering functions into separate objects.

This section is intended to help you understand the different types of printer drivers that the Windows 2000 and later operating systems support, but you should also remember that the following three printer drivers are shipped with the operating system:

[Microsoft Universal Printer Driver](microsoft-universal-printer-driver.md)

[Microsoft PostScript Printer Driver](microsoft-postscript-printer-driver.md)

[Microsoft Plotter Driver](microsoft-plotter-driver.md)

These three drivers support most printing devices that end-users can purchase today. You need to write a printer driver only if your printing device is not compatible with the appropriate Microsoft-supplied driver. You can support most new printers by simply adding a [printer data file](printer-data-files.md) to one of the Microsoft-supplied drivers. Devices that might require a new driver include those containing hardware drawing accelerators controlled by proprietary command sequences.

This section contains the following topics, which describe the Windows printing architecture.

[XPSDrv Printer Drivers](xpsdrv-printer-drivers.md)

[GDI Printer Drivers](gdi-printer-drivers.md)

[Print Ticket and Print Capabilities Technologies](print-ticket-and-print-capabilities-technologies.md)

[Writing 64-Bit Printer Drivers](writing-64-bit-printer-drivers.md)

 

 


--------------------
[Send comments about this topic to Microsoft](mailto:wsddocfb@microsoft.com?subject=Documentation%20feedback%20%5Bprint\print%5D:%20Printer%20Driver%20Architecture%20%20RELEASE:%20%289/1/2016%29&body=%0A%0APRIVACY%20STATEMENT%0A%0AWe%20use%20your%20feedback%20to%20improve%20the%20documentation.%20We%20don't%20use%20your%20email%20address%20for%20any%20other%20purpose,%20and%20we'll%20remove%20your%20email%20address%20from%20our%20system%20after%20the%20issue%20that%20you're%20reporting%20is%20fixed.%20While%20we're%20working%20to%20fix%20this%20issue,%20we%20might%20send%20you%20an%20email%20message%20to%20ask%20for%20more%20info.%20Later,%20we%20might%20also%20send%20you%20an%20email%20message%20to%20let%20you%20know%20that%20we've%20addressed%20your%20feedback.%0A%0AFor%20more%20info%20about%20Microsoft's%20privacy%20policy,%20see%20http://privacy.microsoft.com/default.aspx. "Send comments about this topic to Microsoft")


