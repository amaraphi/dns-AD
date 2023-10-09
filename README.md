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
    <li>Let's say that we want our DNS server to associate the alias <code>mainframe</code> with the domain controller's IP address. In Client-1, open the command line and ping <code>mainframe</code>. Note that the host name cannot be found.</li>
  </ul>
  <img src="https://github.com/amaraphi/dns-AD/assets/144752187/89504fe9-55e3-441d-9595-6fdf4de5e490"/>
  <ul>
    <li>Use the <code>nslookup</code> command for <code>mainframe</code>. Note that this request also fails and that there is no DNS record for <code>mainframe</code>.</li>
  </ul>
  <img src="https://github.com/amaraphi/dns-AD/assets/144752187/97526852-95ed-4927-8e7c-9fcc12c14731"/>
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
  <img src="ttps://github.com/amaraphi/dns-AD/assets/144752187/b2e6a195-39e2-4fd1-9ffa-1ff7ee5ba30f"/>
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
  <img src="https://github.com/amaraphi/dns-AD/assets/144752187/e2e832b2-1d02-42b4-aa94-3dec01054ca0"/>
  <ul>
    <li>Return to Client-1’s command line and ping <code>mainframe</code>.</li>
  </ul>
  <ul>
    <li>We can see that Client-1 is now able to send ICMP packets to the IP address associated with <code>mainframe</code>.</li>
  </ul>
  <img src="https://github.com/amaraphi/dns-AD/assets/144752187/791b09f9-e730-4ca3-b9e0-9070d4cc4a00"/>
  <ul>
    <li>We can look up the updated DNS settings for <code>mainframe</code> in the command line with <code>nslookup</code>:</li>
  </ul>
  <ul>
    <li>Note that in response to this query, the DNS server has returned DC-1’s IP address</li>
  </ul>
  <img src="https://github.com/amaraphi/dns-AD/assets/144752187/124a983d-ea42-4b28-92cc-9490dbfcab78"/>
  <h3>Now that our DNS server has resolved <code>mainframe</code>, the IP address to hostname mapping should be found in its local cache.</h3>
  <ul>
    <li>Use <code>ipconfig /displaydns</code> to pull up the DNS cache.</li>
  </ul>
  <img src="https://github.com/amaraphi/dns-AD/assets/144752187/0a6d88d8-eba9-4502-9a61-9b036e8536bd"/>
  <h3>Changing the <code>mainframe</code> IP address</h3>
  <ul>
    <li>Let's change <code>mainframe</code>'s associated IP address one more time. In the DNS Manager, open the record for <code>mainframe</code> and change its IP address to 8.8.8.8, which is an IP address associated with Google.</li>
  </ul>
  <img src="https://github.com/amaraphi/dns-AD/assets/144752187/908fad4c-6745-4a92-a341-2204ec97a034"/>
  <ul>
    <li>Back in Client-1’s command line, ping <code>mainframe</code> again.</li>
  </ul>
  <ul>
    <li>Note that the <code>mainframe</code> host name still resolves to DC-1 IP address.</li>
  </ul>
  <ul>
    <li>Use <code>ipconfig /displaydns</code> to display the DNS cache again. Note that <code>mainframe</code> is still mapped to DC-1’s IP address.</li>
  </ul>
  <img src="https://github.com/amaraphi/dns-AD/assets/144752187/3bbb81c3-42b5-4fb4-ac6a-6f5ea35f7ee8"/>
  <ul>
    <li>Open the command prompt as an admin by searching for the command prompt, right-clicking and selecting “Run as administrator.”</li>
  </ul>
  <ul>
    <li>Use the command <code>ipconfig /flushdns</code> to flush the local DNS cache.</li>
  </ul>
  <ul>
    <li>Ping <code>mainframe</code> again.</li>
  </ul>
  <img src="https://github.com/amaraphi/dns-AD/assets/144752187/887f4eae-be3d-41e7-a6cd-41f9021fdcfb"/>
  <ul>
    <li>Note that the updated A record — Google’s IP address — is now in the DNS cache.</li>
  </ul>
