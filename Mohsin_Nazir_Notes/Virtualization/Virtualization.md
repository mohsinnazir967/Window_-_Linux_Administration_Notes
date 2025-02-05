
## Hypervisor

The hypervisor is a hardware virtualization technique that allows multiple guest operating systems (OS) to run on a single host system at the same time. A hypervisor is sometimes also called a virtual machine manager(VMM).

### Types of Hypervisor 

**TYPE-1 Hypervisor:**   

*The hypervisor runs directly on the underlying host system*. It is also known as a “Native Hypervisor” or “Bare metal hypervisor”. It does not require any base server operating system. It has direct access to hardware resources. Examples of Type 1 hypervisors include VMware ESXi, Citrix XenServer, and Microsoft Hyper-V hypervisor.

**TYPE-2 Hypervisor:**   

*A Host operating system runs on the underlying host system.* It is also known as ‘Hosted Hypervisor”. Such kind of hypervisors doesn’t run directly over the underlying hardware rather they run as an application in a Host system(physical machine). Basically, the software is installed on an operating system. Hypervisor asks the operating system to make hardware calls. An example of a Type 2 hypervisor includes VMware Player or Parallels Desktop. Hosted hypervisors are often found on endpoints like PCs.  The type-2 hypervisor is very useful for engineers, and security analysts (for checking malware, or malicious source code and newly developed applications).

![[Hypervisor.png]]


### Benefits of Hardware Virtualization Supported by CPU

1. **Improved Performance**:
    - Hardware-assisted virtualization reduces overhead, leading to faster and more efficient VM operations 
2. **Enhanced Security**:
    - Provides better isolation between VMs, reducing the risk of security breaches
3. **Advanced Features**:
    - Enables features like live migration, nested virtualization, and better resource management.
4. **Stability and Reliability**:
    - More stable and reliable VM operations due to hardware support

### Drawbacks of Not Having Hardware Virtualization

5. **Performance Degradation**:
    - Higher overhead and slower VM performance due to software-based emulation
6. **Limited Functionality**:
    - Inability to use advanced virtualization features and potential compatibility issues
7. **Increased Risk of Crashes**:
    - Less stable and reliable VM operations, leading to potential crashes
8. **Weaker Security**:
    - Reduced isolation between VMs, increasing vulnerability to attacks