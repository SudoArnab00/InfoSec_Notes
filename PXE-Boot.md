## PXE Boot
- used to allow new devices that are connected to network to load and install the OS directly over a network connection
- [MDT](/MDT.md) can be used to create, manage and host PXE boot images
- usually integrated with DHCP, which means if DHCP assigns an IP lease, the host is allowed to request the PXE boot image and start the network OS installation process

[Image of process]

- Once the process is performed, the client will use a TFTP connection to download (recover) the PXE boot image from MDT server


## Risks
Exploiting the PXE boot images can lead to:
- inject a privilege escation vector, such as a Local Admin account, to gain Admin access to the OS once the PXE boot has been completed
- perform password scrapping attakcs to recover AD creds used during install (unattended)

BCD (.bcd) files store relevant information to PXE boots for different types of architecture - use TFTP (not like FTP, needs specific file name and details to transfer the file using UDP, no way of fetch file lists) to request each BCD files and enumerate the configs.
[WIM files](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/work-with-windows-images?view=windows-11)

[]More insight](https://www.riskinsight-wavestone.com/en/2020/01/taking-over-windows-workstations-pxe-laps/)

## Tools
- [PowerPxe](https://github.com/wavestone-cdt/powerpxe)
