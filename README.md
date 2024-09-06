<h1 align="center">Secure Cloud Configuration - Part 2</h1>

<br>

<h2 align="center">Securing our Resources: NIST 800-53 SC-7</h2>

<p align="center">
<img width="900" src="https://github.com/user-attachments/assets/01b8265b-2ff6-4ea2-969c-58eec61c54ce" alt="Banner"/>

<br>

<br>

In this Lab we’re going to Implement **NIST 800-53 Control SC-7 - Boundary Protection**.

In the previous Lab we looked through the different **Security Recommendations**, **Regulatory Compliance** & **Microsoft Defender for Cloud**.

We have already analyzed a few **Vulnerabilities in our Cloud Environment and Resources**.

So the next step is to work on Implementing this **Security Control**:

<br>

![azure portal](https://github.com/user-attachments/assets/a368d43c-06b0-40d9-a4ec-6ccb7eb1a8ea)

<br>

So in this part of our Project we’re going to:

-	Configure a **Network Security Group** on our **Subnet**.
  
-	Implement **Private Link & Private Endpoints** on our **Storage Account & Key Vault**.
  
-	Enable the Built-In **Firewall** on our **Storage Account & Key Vault**.

<br>

<br>

  <details close> 
  
**<summary> 💡 Refresher</summary>**

<br>

To give you an idea of how our Environment has evolved over the Previous Labs:

➡️ Originally we had our Virtual Machines, Storage Account & Key Vault deployed and Exposed to the Public Internet in an Insecure Way.

➡️ We added NSGs to our VMs ➜ but they were configured to be wide-open on purpose so bad actors would find them.

<br>

So this was our **Initial Configuration**:

![azure portal](https://github.com/user-attachments/assets/1cee5fb8-aa04-4d5b-af31-56440194db9b)

<br>

When we were working our Incidents ➜ there were a lot of Brute-Force Events.

To mitigate those instances and go through the Incident Management Lifecycle ➜ we ended up **Hardening our NSG**s.

We configured the NSGs to only allow Inbound Traffic from our own Personal Computer:

![azure portal](https://github.com/user-attachments/assets/a8522ad3-e296-4885-96ad-384560f79ec6)

<br>

Our Storage Account & Key Vault are still Exposed to the Public Internet though.

<br>

So what we’re going to do in this Lab is:

<br>

❶ Apply another **NSG** to our Subnet ➜ since the Security Control is requesting it.

<br>

❷ Enable **Private Endpoint** for our Storage Account & our Key Vault


- What that does is take them off the Public Internet and makes them accessible only within our Subnet and Virtual Network

- So theoretically only the VMs that are in the Subnet and VNet are going to be able to access those 2 Resources.

<br>

❸ And we’re also going to Enable the Built-in **Firewall** on these 2 Resources as well to disallow access from the Public Internet.

<br>

<h2></h2>

<br>

So at the end of this Lab our Environment will look something like this:

![azure portal](https://github.com/user-attachments/assets/c53301c2-058d-43db-a543-8001ce0f4bf7)

<br>

Most of the following Vulnerabilities (except for the Azure Firewall on the VNet) should be Remediated:

![azure portal](https://github.com/user-attachments/assets/1a74fb8f-ed49-4806-a3e0-507a3f2cafb1)

<br>

And then we’re going to wait another 24 hours and take a Snapshot of our Stats in the Maps.

We'll then compare those new stats with the previous Security Metrics from the when our Environment was Insecure.

<br>

  </details>

<br>

<br>

<details close> 
<summary> <h2>1️⃣ Configure the Firewall & Azure Private Link for our Azure Key Vault</h2> </summary>
<br>

The first thing we’re going to do is Enable the Firewall and Configure Azure Private Link for our Key vault instance.

Inside the Azure Portal ➜ click on our ```akv-cyber-lab``` Key Vault instance:

![azure portal](https://github.com/user-attachments/assets/1a74fb8f-ed49-4806-a3e0-507a3f2cafb1)

<br>

We’ll first **Enable the Firewall** ➜ so click on the **Networking** blade:

![azure portal](https://github.com/user-attachments/assets/1a74fb8f-ed49-4806-a3e0-507a3f2cafb1)

<br>

Under the **Firewalls and virtual networks** tab we’ll:

> ◉ **Disable public access**
> 
> ☑ **Allow trusted Microsoft services to bypass this firewall**

Click the **Apply** button:

![azure portal](https://github.com/user-attachments/assets/1a74fb8f-ed49-4806-a3e0-507a3f2cafb1)

<br>

✅ Our Key Vault has now the Firewall Enabled

<br>

<h2></h2>

<br>

Next we’re going to Configure the Private Endpoint for our Key Vault.

<br>

  <details close> 
  
**<summary> 💡</summary>**

This will take it from being totally exposed to the Public Internet ➜ to only being accessible through our Virtual Network and Subnet.

  </details>

<br>

Still inside Networking ➜ this time under the **Private endpoint connections** tab ➜ click on ➕ **Create**:

![azure portal](https://github.com/user-attachments/assets/1a74fb8f-ed49-4806-a3e0-507a3f2cafb1)

<br>

Under the ❶ **Basics** tab:
-	Select our ```RG-Cyber-Lab``` **Resource group**
-	We’ll **Name** it ```PE-AKV```
-	Select the same **Region** that we’ve been using ➜ ```East US 2```

![azure portal](https://github.com/user-attachments/assets/1a74fb8f-ed49-4806-a3e0-507a3f2cafb1)

<br>

Now under the ❷ **Resources** tab we’ll set up:
-	**Connection method**➜ ◉ ```Connect to an Azure resource in my directory``` 
-	**Resource type** ➜ search for and select ```Microsoft.KeyVault/vaults```
-	**Resource** ➜ pick the name of our Key Vault instance: ```akv-cyber-lab```
-	**Target sub-resource** ➜ ```vault```

![azure portal](https://github.com/user-attachments/assets/1a74fb8f-ed49-4806-a3e0-507a3f2cafb1)

<br>

Under the ❸ **Virtual Network** tab:
-	**Virtual Network**➜ pick our VNet ```Lab-Vnet (RG-Cyber-Lab)``` where we’re having everything connect to
-	**Subnet** ➜ select our ```default``` Subnet
-	**Private IP configuration**➜ ◉ ```Dynamically allocate IP address```

![azure portal](https://github.com/user-attachments/assets/1a74fb8f-ed49-4806-a3e0-507a3f2cafb1)

<br>

For the ❹ **DNS** tab:
-	**Integrate with private DNS zone**➜ ◉ ```Yes```

⚠️ Make sure the **Subscription** & **Resource Group** are correct.

![azure portal](https://github.com/user-attachments/assets/1a74fb8f-ed49-4806-a3e0-507a3f2cafb1)

<br>

✅ We’ll then just **Review + create** to finish creating the Key Vault’s **Private Endpoint**.

<br>

  </details>

<h2></h2>

<details close> 
<summary> <h2>2️⃣ Configure the Firewall & Azure Private Link for our Azure Storage Account</h2> </summary>
<br>

Inside the Azure Portal ➜ click on our ```sacyberlab01``` Storage Account instance:

![azure portal](https://github.com/user-attachments/assets/1a74fb8f-ed49-4806-a3e0-507a3f2cafb1)

<br>

First we’ll click on the **Configuration** blade.

For **Allow Blob public access** ➜ we’ll select ◉ ```Disabled```

⚠️ We have to do this in order to satisfy the **NIST 800-53 SC-7** Requirement.

![azure portal](https://github.com/user-attachments/assets/1a74fb8f-ed49-4806-a3e0-507a3f2cafb1)

<br>

Now we'll click on the **Networking** blade ➜ and under the **Firewalls and virtual networks** tab we’ll select:
-	**Public network access** ➜ ◉ ```Disabled```
-	**Routing preference** ➜  ◉ ```Microsoft network routing```

Click the 💾 **Save** button

![azure portal](https://github.com/user-attachments/assets/1a74fb8f-ed49-4806-a3e0-507a3f2cafb1)

<br>

✅ The Firewall and VNet setting were successfully saved for our Storage Account.

<h2></h2>

The next step is to Add a Private Endpoint for our ```sacyberlab01``` Storage Account instance.

We’ll go to the **Private endpoint connections** tab ➜ and click on ➕ **Private endpoint**:

![azure portal](https://github.com/user-attachments/assets/1a74fb8f-ed49-4806-a3e0-507a3f2cafb1)

<br>

Under the ** ❶ Basics** tab:
-	Select our ```RG-Cyber-Lab``` **Resource group**
-	We’ll **Name** it ```PE-Storage```
-	Make sure it’s in the correct **Region** with the rest of our Resources  ➜ ```East US 2```

![azure portal](https://github.com/user-attachments/assets/1a74fb8f-ed49-4806-a3e0-507a3f2cafb1)

<br>

As for ** ❷ Resources** ➜ this is for our Blob Storage:
-	So for **Target sub-resource** ➜ select ```blob```

![azure portal](https://github.com/user-attachments/assets/1a74fb8f-ed49-4806-a3e0-507a3f2cafb1)

<br>

Under the ** ❸ Virtual Network** tab:
-	**Virtual Network**➜ select our VNET ```Lab-Vnet (RG-Cyber-Lab)``` where everything else is in
-	**Subnet** ➜ select our only ```default``` Subnet
-	**Private IP configuration**➜ ◉ ```Dynamically allocate IP address```

![azure portal](https://github.com/user-attachments/assets/1a74fb8f-ed49-4806-a3e0-507a3f2cafb1)

<br>

** ❹ DNS** tab:
-	**Integrate with private DNS zone** ➜ ◉ ```Yesv

⚠️ Again ➜ make sure the **Subscription** & **Resource Group** are correct.

![azure portal](https://github.com/user-attachments/assets/1a74fb8f-ed49-4806-a3e0-507a3f2cafb1)

<br>

✅ And finally we’ll click on **Review + create** to finish creating the Storage Account’s **Private Endpoint**.

<br>

  </details>

<h2></h2>

<details close> 
<summary> <h2>2️⃣ Observe the Network Watcher Topology for our Region & Resource Group</h2> </summary>
<br>

Inside the **Azure Portal** ➜ we'll go to **Network Watcher**

![azure portal](https://github.com/user-attachments/assets/1a74fb8f-ed49-4806-a3e0-507a3f2cafb1)

<br>

Click on the **Topology** Blade ➜ and this is our **Topology Page**:

![azure portal](https://github.com/user-attachments/assets/1a74fb8f-ed49-4806-a3e0-507a3f2cafb1)

<br>

It shows a Network Diagram of what our Azure Resources look like in our Subscription.

Basically we need to "Filter the Topology" to show the correct Resources ➜ so we'll click on the **Scope** section:

![azure portal](https://github.com/user-attachments/assets/1a74fb8f-ed49-4806-a3e0-507a3f2cafb1)

<br>

We'll select:
- Our **Subscription** ➜ ```Azure Subscription 1```
- ⚠️ Only the **Resource Group** ```RG-Cyber-Lab``` ➜ because that's essentially where all our Resources are.
- Our Environment's Resources' **Location** ➜ ```East US 2```

Then we'll click **Save**

![azure portal](https://github.com/user-attachments/assets/1a74fb8f-ed49-4806-a3e0-507a3f2cafb1)

<br>

This is basically all the Resources we have inside of our Subnet:

![azure portal](https://github.com/user-attachments/assets/1a74fb8f-ed49-4806-a3e0-507a3f2cafb1)

<br>

<h2></h2>

We'll now observe if the Key Vault and Storage Account Private Endpoints are shown in our Network Topology Diagram.

If we click the ➕ on the Private Endpoint for our Azure Key Vault:

![azure portal](https://github.com/user-attachments/assets/1a74fb8f-ed49-4806-a3e0-507a3f2cafb1)

<br>

✅ We can see that it is associated with the Subnet which is attached to our Key Vault instance ```akv-cyber-lab-9999```

![azure portal](https://github.com/user-attachments/assets/1a74fb8f-ed49-4806-a3e0-507a3f2cafb1)

<br>

We can click the ➕ on the Private Endpoint for our Azure Storage Account:

![azure portal](https://github.com/user-attachments/assets/1a74fb8f-ed49-4806-a3e0-507a3f2cafb1)

<br>

✅ And we can see that it is inside of our Default Subnet and attached to our Storage Account ```sacyberlab999```

<br>

  </details>

<h2></h2>

<details close> 
<summary> <h2>3️⃣ Check the IP addresses of our Key Vault & Storage Account inside our Windows VM</h2> </summary>
<br>

The next thing we're going to do is login to our **Windows VM** and make sure **Private Endpoint** is working.

So inside the **Azure Portal** ➜ we'll go our ```windows-vm``` ➜ and copy its **Public IP Address**:

![azure portal](https://github.com/user-attachments/assets/1a74fb8f-ed49-4806-a3e0-507a3f2cafb1)

<br>

We'll open up Microsoft Remote Desktop ➜ and then attempt to connect to the ```windows-vm```

![azure portal](https://github.com/user-attachments/assets/1a74fb8f-ed49-4806-a3e0-507a3f2cafb1)

<br>

Once we've RDP connected into our Windows VM ➜ we'll open **Powershell**

![azure portal](https://github.com/user-attachments/assets/1a74fb8f-ed49-4806-a3e0-507a3f2cafb1)

<br>

The next step is to check the **Private IP Addresses** of our Key Vault and Storage Account instances by using ```nslookup```

📝 This is how we resolve FQDN or the name of any host to IP Addresses.

So we'll go back to the **Azure Portal** ➜ inside our Key Vault ```akv-cyber-lab``` ➜ and copy its **FQDN**:

![azure portal](https://github.com/user-attachments/assets/1a74fb8f-ed49-4806-a3e0-507a3f2cafb1)

<br>

We'll go back to the Windows VM ➜ in **Powershell** type in ```nslookup``` and paste the Key Vault's **FQDN** ➜ press Enter:

![azure portal](https://github.com/user-attachments/assets/1a74fb8f-ed49-4806-a3e0-507a3f2cafb1)

<br>

It resolves to ```10.0.0.5```

This means our **Private Endpoint** is working and our Key Vault is actually **within the same VNET / Subnet as our Windows VM** (our at least it is accessible from there).

✅ So we have confirmed that Key Vault is set up correctly with Private Endpoint.

<br>

  <details close> 
  
**<summary> 💡 Reminder</summary>**

I can see Private Endpoint is working, not because it's resolving to exactly ```10.0.0.5``` ➜ but because it's resolving to a **Private IP Address** within our **Subnet's range**.

If I was resolving to a **Public IP Address** ➜ then we'd have to troubleshoot.

  </details>

<br>

<h2></h2>

<br>

We'll check our Storage Account's Private Endpoint next.

Back in the **Azure Portal** ➜ we'll go to our Storage Acount ```sacyberlab01```

![azure portal](https://github.com/user-attachments/assets/1a74fb8f-ed49-4806-a3e0-507a3f2cafb1)

<br>

Scroll down and click on the **Endpoints** blade ➜ copy the **Blob Service FQDN** (the full name of our Storage Account):

![azure portal](https://github.com/user-attachments/assets/1a74fb8f-ed49-4806-a3e0-507a3f2cafb1)

<br>

We'll go back to our Windows VM ➜ type in ```nslookup``` and paste the Storage Account's **FQDN** ➜ press Enter:

![azure portal](https://github.com/user-attachments/assets/1a74fb8f-ed49-4806-a3e0-507a3f2cafb1)

<br>

We get this ```10.0.0.7``` Private IP Address.

✅ So we can confirm that the Private Endpoint for our Storage Account is set up correctly as well

<br>

  </details>

<h2></h2>

<details close> 
<summary> <h2>4️⃣ Set Up a Network Security Group on the Subnet</h2> </summary>
<br>

The last thing we're going to do is Create a **NSG** and attach it to the actual **Subnet** all our Resources are in.

Inside the **Azure Portal** ➜ search for **Network security groups** ➜ and we're going to ➕ **Create** a new NSG:

![azure portal](https://github.com/user-attachments/assets/1a74fb8f-ed49-4806-a3e0-507a3f2cafb1)

<br>

- Make sure it's in the correct **Resource Group** ```RG-Cyber-Lab```
- We'll **Name** it ```nsg-subnet```
- The **Region** is ```East US 2```

Click **Review + create**:

![azure portal](https://github.com/user-attachments/assets/1a74fb8f-ed49-4806-a3e0-507a3f2cafb1)

<br>

We'll then go to our Virtual Network ```Lab-VNet``` where all of our Resources are:

![azure portal](https://github.com/user-attachments/assets/1a74fb8f-ed49-4806-a3e0-507a3f2cafb1)

<br>

Go to the **Subnets** Blade ➜ and select our ```default``` Subnet.

Where it says **Network security group** ➜ we'll select our newly created ```nsg-subnet``` Subnet.

Click **Save**.

![azure portal](https://github.com/user-attachments/assets/1a74fb8f-ed49-4806-a3e0-507a3f2cafb1)

<br>

✅ We have successfully configure a **Network Security Group** on our **Subnet**. 

<br>

<h2></h2>

<br>

We'll now go back to Network Watcher to see everything one more time from a High-Level.

After clicking the **Topology** blade ➜ we'll select the **Scope** to filter what we want to observe:

![azure portal](https://github.com/user-attachments/assets/1a74fb8f-ed49-4806-a3e0-507a3f2cafb1)

<br>

Inside our VNet ➜ we can visually see the new **NSG** attached to our **Subnet**:

![azure portal](https://github.com/user-attachments/assets/1a74fb8f-ed49-4806-a3e0-507a3f2cafb1)

<br>

<h2></h2>

  </details>

<br>

<br>

<br>

<h1>Summary</h1>
<br>

We have completed this lab ✅

Most of the Requirements for **NIST 800-53 SC-7** & Recommendations in **Microsoft Defender for Cloud** were satisfied:

- We configured a **Network Security Group** on our Subnet

- Implemented **Private Endpoints** on our **Storage Account & Key Vault**.
  
-	And enabled the **Firewall** on our **Storage Account & Key Vault**.

<br>

In the next Lab we're going to Capture our Stats after waiting another 24 hours ➜ now that our Environment has been Hardened and Secured.

We'll then compare that Data with the Events we had before.


<br>

<br>

<br>

<br>

<br>

<br>

<br>
  
<br>
