---
sidebar_position: 1
---

# Best Practices

Here you can find notes on best practices for setting up an SAP Business One environment, which plays a crucial role in CompuTec product performance.

## Hardware requirements

SAP Business One's performance is highly dependent on the overall hardware performance.

### CPU

It would be best if you use the latest available hardware platform.

In the case of SAP HANA, only Intel CPUs are supported. You can find all the supported hardware configurations in the [Certified and Supported SAP HANA Hardware Directory](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/#/solutions?filters=appliance;v:at4). Unfortunately, there is a long delay when a hardware vendor certifies its newest platform in many cases. Therefore, it's a widespread practice to choose the latest available hardware platform which matches the certified hardware, and the CPU has the same or higher specification (cores, frequency, and cache).

### Disks

You should not use spinning disks as they are slow or SATA SSD disks that are half-duplex. Only SAS SSD or, even better, NVMe disks should be used.

As NVMe disks are attached to PCI slots, only software RAID on the OS level can be used for older systems. For better performance, it's advisable to use hardware platforms with support for Intel Virtual RAID on CPU (Intel VROC) or use an external all-flash storage array.

SAP HANA data and log volumes should be placed on different disks.

Please be aware that SAP doesn't certify iSCSI connections and only Fiber Channel (FC) if a storage array is used. For more details, please consult this document: SAP HANA TDI-Storage Requirements.

In the virtualized installation, the LVM can be used. Although in physical installation, it can be avoided to have the best disk performance.

### Network

A low-latency network should be used between all SAP Business One components. SAP doesn't support networks with latency higher than 1 ms. Therefore, Wi-Fi and WAN connections don't guarantee a pleasant experience in the responsiveness of the system.

### Virtualization

SAP Business One installation can only be virtualized on VMware, and there are specific requirements for the installation.

They are greatly described on [the page with links to SAP Notes](https://wiki.scn.sap.com/wiki/display/VIRTUALIZATION/SAP+HANA+on+VMware+vSphere).

There is also additional information on [the VMware site](https://blogs.vmware.com/apps/2018/01/hyper-threading-impact-virtual-sap-sizing-performance-part-1-2.html).

### OS Configuration

As we only have practical experience with SUSE, all the links are provided.

Most settings can be automatically applied by using sapconf in SUSE or saptune in the case of SUSE for the SAP Applications version.

These tools are described in the SAP Note [1275776 - Linux: Preparing SLES for SAP environments](https://launchpad.support.sap.com/#/notes/1275776), and also here:

- [SAP blog: sapconf – A way to prepare a SLES system for SAP workload – Part 1](https://blogs.sap.com/2018/06/13/sapconf-a-way-to-prepare-a-sles-system-for-sap-workload-part-1)

- Recommended SUSE SLES 4 SAP Settings.

Please verify your OS settings with all the SAP Notes even after running them, as not every setting is applied.

Here are the most important SAP Notes:

- [2235581 - SAP HANA: Supported Operating Systems](https://launchpad.support.sap.com/#/notes/2235581)

- [1944799 - SAP HANA Guidelines for SLES Operating System Installation](https://launchpad.support.sap.com/#/notes/1944799)

- [2684254 - SAP HANA DB: Recommended OS settings for SLES 15 / SLES for SAP Applications 15](https://launchpad.support.sap.com/#/notes/2684254)

- [2382421 - Optimizing the Network Configuration on HANA- and OS-Level](https://launchpad.support.sap.com/#/notes/2382421).

### SAP HANA Replication

In the case of using SAP HANA replication for HA purposes, there are three modes: SYNC, SYNCMEM, and ASYNC. Please be aware that SYNC and SYNCMEM provide additional bottlenecks on each transaction as it needs to be confirmed on the second server.

You can check the details in the SAP documentation: [Replication Modes for SAP HANA System Replication](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.05/en-US/c039a1a5b8824ecfa754b55e0caffc01.html).

### Final Thoughts

All the settings described in the above links and SAP Notes should be carefully read, understood, and applied, as each of them contributes to the overall system performance used by customers for many years.
