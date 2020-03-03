﻿---

copyright:
  years: 2019, 2020

lastupdated: "2020-03-03"

keywords: network interface, TCP/IP address, IBM i VM, external IP address, DNS, LIND, CFGTCP command

subcollection: power-iaas

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:external: .external}

# How to add or remove a network interface from an IBM i virtual machine (VM)
{: #managing-network-interface-ibmi}

Since IBM PowerVC Version 1.2.2, IBM PowerVC can dynamically add a network interface controller (NIC) to a VM or remove a NIC from a VM. IBM PowerVC does not set the IP address for new network interfaces that are created after the machine deployment. Any removal of a NIC results in freeing the IP address that was set on it. You must remove and readd the AIX VM network interface if you choose to disconnect the {{site.data.keyword.powerSys_notm}} AIX VM from a public network.
{: shortdesc}

When you toggle a public network off and then on, the IBM console regenerates new internal and external IP addresses. You need to check the IBM console for the new internal IP address to complete this procedure.
{: note}

## Removing a network interface from an IBM i VM
{: #remove-nic-ibmi}

Learn how to change the TCP/IP address of your IBM i VM. You can change your system's TCP/IP address while the TCP/IP is active. However, you must deactivate the TCP/IP interface (TCP/IP address). For a complete set of instructions, see [Changing the TCP/IP Address of the IBM System i System](https://www.ibm.com/support/pages/changing-tcpip-address-ibm-system-i-system){: new_window}{: external}.

1. Before you change a TCP/IP address (interface), determine whether it has any associated routes. Choose **Option 8** from the **NETSTAT \*IFC** screen.

    It's a good idea to select **Option 5** to display the details of the route. Remember to press **F6** to print the details for reference for when the route must be readded. This step should be done for all of the **non-\*DIRECT** routes that are listed on the screen.
    {: tip}

2. Run the `CFGTCP` command, and select **Option 2** to work with your TCP/IP routes. Select **Option 4** next to the routes you'd like to remove. All of that communication that is going over the route is terminated after you remove it.

3. To make the actual changes, you must deactivate and remove the interface before you add it. Select **Option 10** next to the interface you'd like to deactivate on the **NETSTAT \*IFC** screen.

4. To remove the interface after deactivation, run the `CFGTCP` command and select **Option 1** from that
menu. Select **Option 4** next to the interface you'd like to remove.

    ![Removing a network interface](./images/terminal-ibmi-remove-nic.png "Removing a network interface"){: caption="Figure 2. Revmoing a network interface" caption-side="bottom"}

5. You must vary off the Line description (LIND) after you remove the interface.

## Adding a network interface to an IBM i VM
{: #add-nic-ibmi}

1. Now that you removed the routes and interfaces, create the new configuration in the reverse order. To get to the **ADDTCPIFC** screen, run the `CFGTCP` command and select **Option 1**.

    Most configurations require you to update only the first three fields. Add the new TCP/IP address for the iSeries family (you can type over the quotation marks) to the **Internet address** field. Add the new subnet mask to the **Subnet mask** field and complete the remaining fields by using your reference printout. The line description (LIND) must be the same as the LIND defined on the removed interface.
    {: note}

2. After you add the interface, activate it from the **NETSTAT \*IFC** screen by selecting **Option 9**.

3. To verify that the new interface is active, ping the address from the command line. If the ping responds, the change is complete.

![Adding a network interface](./images/terminal-ibmi-add-nic.png "Adding a network interface"){: caption="Figure 2. Adding a network interface" caption-side="bottom"}