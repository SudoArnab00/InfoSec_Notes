
## WMI - Windows Management Instrumentation

WMI is Windows implementation of [WBEM](/WBEM.md), an enterprise standard for accessing management information across devices. 

## Risks
WMI allows administrators to perform standard management tasks that attackers can abuse to perform lateral movement in various ways

**DCOM**: RPC over IP will be used for connecting to WMI. This protocol uses port 135/TCP and ports 49152-65535/TCP, just as explained when using sc.exe.

**Wsman**: WinRM will be used for connecting to WMI. This protocol uses ports 5985/TCP (WinRM HTTP) or 5986/TCP (WinRM HTTPS).