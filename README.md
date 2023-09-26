# dns-AD
Exploring DNS concepts with Virtual Machines in Azure
<body>
  <h1>Tutorial: Using Azure and Active Directory to Explore DNS Concepts</h1>
  <p>Prerequisites: you must have two Azure virtual machines set up from the previous tutorials, which include:</p>
  <ul>
    <li>a Windows Server 2022 named DC-1 with the following settings:
      <ul>
        <li>Active Directory installed and promoted to a domain controller</li>
      </ul>
      <ul>
        <li>domain: mydomain.com</li>
      </ul>
      <ul>
        <li>IP address: static</li>
      </ul>
      <ul>
        <li>User account named jane_admin that is part of the Domain Admins security group</li>
      </ul>
    </li>
  </ul>
  <ul>
    <li>a Windows 10 Virtual Machine with the following settings:
      <ul>
        <li>joined to the domain controller</li>
      </ul>
      <ul>
        <li>DNS server: set to the private IP address of the domain controller, DC-1.</li>
      </ul>
    </li>
  </ul>
  <h3>In the first part of this tutorial we’ll get our DNS server to associate the word “mainframe” with the domain controller’s IP address.</h3>
  <ul>
    <li>Log into the domain controller, DC-1, as the domain admin account, jane_admin</li>
  </ul>
  <ul>
    <li>Log into Client-1 as the domain admin account, jane_admin
      <ul>
        <li>Log in with the fully qualified domain name</li>
      </ul>
    </li>
  </ul>
  <ul>
    <li>Let's say that we want our DNS server to associate the alias "mainframe" with the domain controller's IP address. In Client-1, open the command line and ping <code>mainframe</code>. Note that the host name cannot be found.</li>
  </ul>
  <ul>
    <li>Use the <code>nslookup</code> command for <code>mainframe</code>. Note that this request also fails and that there is no DNS record for <code>mainframe</code>.</li>
  </ul>
  <h3>In order for the DNS server to recognize and associate <code>mainframe</code> with the domain controller, we need to create an A record for mainframe.mydomain.com</h3>
  <ul>
    <li>Return to the Remote Desktop session for DC-1 and open the Server Manager.</li>
  </ul>
  <ul>
    <li>Click on Tools &gt; DNS</li>
  </ul>
  <ul>
    <li>Click on DC-1 &gt; Forward Lookup Zones</li>
  </ul>
  <ul>
    <li>Click on mydomain.com to pull up the DNS server’s host name to IP address mappings.</li>
  </ul>
  <ul>
    <li>Right click and select <strong>New Host (A or AAAA)</strong></li>
  </ul>
  <ul>
    <li>Under name, type <code>mainframe</code>.</li>
  </ul>
  <ul>
    <li>Set the IP address to DC-1’s private IP address. Click Add Host, then Done.</li>
  </ul>
  <ul>
    <li>Return to Client-1’s command line and ping <code>mainframe</code>.</li>
  </ul>
  <ul>
    <li>We can see that Client-1 is now able to send ICMP packets to the IP address associated with <code>mainframe</code>.</li>
  </ul>
  <ul>
    <li>We can look up the DNS settings for <code>mainframe</code> in the command line with <code>nslookup</code>:</li>
  </ul>
  <ul>
    <li>Note that in response to this query, the DNS server has returned DC-1’s IP address</li>
  </ul>
  <h3>Now that our DNS server has resolved <code>mainframe</code>, the IP address to hostname mapping should be found in its local cache.</h3>
  <ul>
    <li>Use <code>ipconfig /displaydns</code> to pull up the DNS cache.</li>
  </ul>
  <h3>Changing the <code>mainframe</code> IP address</h3>
  <ul>
    <li>In the DNS Manager, open the record for <code>mainframe</code> and change its IP address to 8.8.8.8, which is an IP address associated with Google.</li>
  </ul>
  <ul>
    <li>Back in Client-1’s command line, ping <code>mainframe</code> again.</li>
  </ul>
  <ul>
    <li>Note that the <code>mainframe</code> host name still resolves to DC-1 IP address.</li>
  </ul>
  <ul>
    <li>Use <code>ipconfig /displaydns</code> to display the DNS cache again. Note that <code>mainframe</code> is still mapped to DC-1’s IP address.</li>
  </ul>
  <ul>
    <li>Open the command prompt as an admin by searching for the command prompt, right-clicking and selecting “Run as administrator.”</li>
  </ul>
  <ul>
    <li>Use the command <code>ipconfig /flushdns</code> to flush the local DNS cache.</li>
  </ul>
  <ul>
    <li>Ping <code>mainframe</code> again.</li>
  </ul>
  <ul>
    <li>Note that the updated A record — Google’s IP address — is now in the DNS cache.</li>
  </ul>
