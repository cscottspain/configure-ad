<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Video Demonstration</h2>

- ### (COMING SOON!) [YouTube: How to Deploy on-premises Active Directory within Azure Compute](https://www.youtube.com)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Step 1
- Step 2
- Step 3
- Step 4

<h2>Deployment and Configuration Steps</h2>

<p>Setup Resources in Azure</p>
<ol>
    <li>
        <p>Create the Domain Controller VM (Windows Server 2022) (example named &ldquo;DC-1&rdquo;)</p>
        <ul>
            <li>
                <p>Take note of the Resource Group and Virtual Network (Vnet) that get created at this time</p>
            </li>
        </ul>
    </li>
    <li>
        <p>Set Domain Controller&rsquo;s NIC Private IP address to be static</p>
    </li>
    <li>
        <p>Create the Client VM (Windows 10) (example named &ldquo;Client-1&rdquo;). Use the same Resource Group and Vnet that was created in Step 1.a</p>
    </li>
    <li>
        <p>Ensure that both VMs are in the same Vnet (you can check the topology with Network Watcher</p>
    </li>
</ol>
<p><br></p>
<p>Ensure Connectivity between the client and Domain Controller</p>
<ol start="5">
    <li>
        <p>Login to Client-1 with Remote Desktop and ping DC-1&rsquo;s private IP address with&nbsp;ping -t &lt;ip address&gt;&nbsp;(perpetual ping)</p>
    </li>
    <li>
        <p>Login to the Domain Controller and enable ICMPv4 in on the local windows Firewall</p>
    </li>
    <li>
        <p>Check back at Client-1 to see the ping succeed</p>
    </li>
</ol>
<p><br></p>
<p>Install Active Directory</p>
<ol start="8">
    <li>
        <p>Login to DC-1 and install Active Directory Domain Services</p>
    </li>
    <li>
        <p>Promote as a DC: Setup a new forest as&nbsp;mydomain.com&nbsp;(can be anything, just remember what it is)</p>
    </li>
    <li>
        <p>Restart and then log back into DC-1 as user:&nbsp;mydomain.com\labuser</p>
    </li>
</ol>
<p><br></p>
<p>Create an Admin and Normal User Account in AD</p>
<ol start="11">
    <li>
        <p>In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called &ldquo;_EMPLOYEES&rdquo;</p>
    </li>
    <li>
        <p>Create a new OU named &ldquo;_ADMINS&rdquo;</p>
    </li>
    <li>
        <p>Create a new employee named &ldquo;Jane Doe&rdquo; (same password) with the username of &ldquo;jane_admin&rdquo;</p>
    </li>
    <li>
        <p>Add jane_admin to the &ldquo;Domain Admins&rdquo; Security Group</p>
    </li>
    <li>
        <p>Log out/close the Remote Desktop connection to DC-1 and log back in as &ldquo;mydomain.com\jane_admin&rdquo;</p>
    </li>
    <li>
        <p>User jane_admin as your admin account from now on</p>
    </li>
</ol>
<p><br></p>
<p><br></p>
<p>Join Client-1 to your domain (mydomain.com)</p>
<ol start="17">
    <li>
        <p>From the Azure Portal, set Client-1&rsquo;s DNS settings to the DC&rsquo;s Private IP address</p>
    </li>
    <li>
        <p>From the Azure Portal, restart Client-1</p>
    </li>
    <li>
        <p>Login to Client-1 (Remote Desktop) as the original local admin (labuser) and join it to the domain (computer will restart)</p>
    </li>
    <li>
        <p>Login to the Domain Controller (Remote Desktop) and verify Client-1 shows up in ADUC</p>
    </li>
    <li>
        <p>Create a new OU named &ldquo;_CLIENTS&rdquo; and drag Client-1 into there</p>
    </li>
</ol>
<p><br></p>
<p><br></p>
<p>Setup Remote Desktop for non-administrative users on Client-1</p>
<ol start="22">
    <li>
        <p>Log into Client-1 as mydomain.com\jane_admin and open system properties</p>
    </li>
    <li>
        <p>Click &ldquo;Remote Desktop&rdquo;</p>
    </li>
    <li>
        <p>Allow &ldquo;domain users&rdquo; access to remote desktop</p>
    </li>
    <li>
        <p>You can now log into Client-1 as a normal, non-administrative user now</p>
    </li>
    <li>
        <p>Normally you&rsquo;d want to do this with Group Policy that allows you to change MANY systems at once (maybe a future lab)</p>
    </li>
</ol>
<p><br></p>
<p>Create a bunch of additional users and attempt to log into client-1 with one of the users</p>
<ol start="27">
    <li>
        <p>Login to DC-1 as jane_admin</p>
    </li>
    <li>
        <p>Open PowerShell_ise&nbsp;as an administrator</p>
    </li>
    <li>
        <p>Create a new File and paste the contents of the&nbsp;<a href="https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1">script</a> into it (<a href="https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1">https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1</a>)</p>
    </li>
    <li>
        <p>Run the script and observe the accounts being created</p>
    </li>
    <li>
        <p>When finished, open ADUC and observe the accounts in the appropriate OU</p>
    </li>
    <li>
        <p>attempt to log into Client-1 with one of the accounts (take note of the password in the script)</p>
    </li>
</ol>
