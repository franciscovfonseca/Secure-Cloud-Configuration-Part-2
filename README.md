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

In this Lab we’re going to:

-	Configure a Network Security Group on our Subnet.
  
-	Implement Private Link & Private Endpoints on our Storage Account & Key Vault.
  
-	Enable the Built-In Firewall on our Storage Account & Key Vault.

<br>

<br>

  <details close> 
  
**<summary> 💡 Refresher</summary>**

<br>

To give you an idea of how our Environment has evolved over the Previous Labs:

➡️ Originally we had our Virtual Machines, Storage Account & Key Vault deployed and exposed to the Public Internet in an Insecure Way.

➡️ We added NSGs to our VMs ➜ but they were configured to be wide-open on purpose so bad actors would find them.

<br>

So this was our Initial Configuration:

![azure portal](https://github.com/user-attachments/assets/1cee5fb8-aa04-4d5b-af31-56440194db9b)

<br>

When we were working our Incidents ➜ there were a lot of Brute-Force Events.

To mitigate those instances and go through the Incident Management Lifecycle ➜ we ended up Hardening our NSGs.

We configured the NSGs to only allow Inbound Traffic from our own Personal Computer:

![azure portal](https://github.com/user-attachments/assets/a8522ad3-e296-4885-96ad-384560f79ec6)

<br>

Our Storage Account & Key Vault are still exposed to the Public Internet though.

<br>

So what we’re going to do in this Lab is:

<br>

❶ Apply another NSG to our Subnet ➜ since the Security Control is requesting it.

<br>

❷ Enable Private Endpoint for our Storage Account & our Key Vault


- What that does is take them off the Public Internet and makes them accessible only within our Subnet and Virtual Network

- So theoretically only the VMs that are in the Subnet and VNet are going to be able to access those 2 Resources.

<br>

❸ And we’re also going to Enable the Built-in Firewall on these 2 Resources as well to disallow access from the Public Internet.

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

We’ll first enable the Firewall ➜ so click on the **Networking** blade:

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
-	**Resource** ➜ pick the name of your Key Vault instance: ```akv-cyber-lab```
-	**Target sub-resource** ➜ lab ```vault```

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

✅ We’ll then just **Review + create** to finish creating the Key Vault’s Private Endpoint.

<br>

  </details>

<h2></h2>

<details close> 
<summary> <h2>2️⃣ Enable NIST 800-53</h2> </summary>
<br>

> Add [**NIST 800-53: Security and Privacy Controls for Information Systems and Organizations**](https://csrc.nist.gov/pubs/sp/800/53/r5/upd1/final) to **Microsoft Defender for Cloud**.
> 
> 💡 [**Full Publication**](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-53r5.pdf).

<br>

The next thing we’re going to do is **Enable Regulatory Compliance for NIST 800-53**.

This will basically do the same thing as the **MDC Secure Score Recommendations** ➜ but it’ll do it in the “lens” of **NIST 800-53**.

<br>

  <details close> 
  
**<summary> 📌 Refresher</summary>**

<br>

[NIST 800-53](https://csrc.nist.gov/projects/cprt/catalog#/cprt/framework/version/SP_800_53_5_1_0/home) is a really large control catalog with a lot of different control families like:

-	 Access Control
-	 Identification and Authentication
-	 Incidence Response, etc.

<br>

![azure portal](https://github.com/user-attachments/assets/9c1cce53-082a-4c9e-b6d5-7da25a14a9d7)

<br>

These are all different categories ➜ and then inside of these there’s a lot of **Sub-Controls** essentially

For example: inside of **"SC - SYSTEM AND COMMUNICATION PROTECTION"** ➜ there's a lot of different Categories:

<br>

![azure portal](https://github.com/user-attachments/assets/9c1cce53-082a-4c9e-b6d5-7da25a14a9d7)

<br>

When we enable NIST 800-53 Regulatory Compliance inside of Defender for Cloud:

  - It’s basically going to look at all the different Controls inside of NIST 800-53
    
  - And it'll suggest different things we can do in our Environment that align with, for example, SC-7:

<br>

![azure portal](https://github.com/user-attachments/assets/9c1cce53-082a-4c9e-b6d5-7da25a14a9d7)

<br>

Essentially it will suggest different implementations we can do in Azure to bring our Environment "Up to Par" with NIST 800-53.

In a way, show us Gaps in relation to Controls that exist in NIST 800-53.

  </details>

<br>

To add NIST 800-53:

  - First inside the MDC home page ➜ click on the **Regulatory Compliance** blade on the left side:

<br>

![azure portal](https://github.com/user-attachments/assets/9c1cce53-082a-4c9e-b6d5-7da25a14a9d7)

<br>

  - Then at the top ➜ click on **Manage compliance policies**:

<br>

![azure portal](https://github.com/user-attachments/assets/9c1cce53-082a-4c9e-b6d5-7da25a14a9d7)

<br>

  - We'll then click on our ```Azure subscription 1```:

<br>

![azure portal](https://github.com/user-attachments/assets/9c1cce53-082a-4c9e-b6d5-7da25a14a9d7)

<br>

  - Inside the **Security policy** blade ➜ under **Industry & regulatory standards**  ➜ click on the **"Add more standards"** button:

<br>

![azure portal](https://github.com/user-attachments/assets/9c1cce53-082a-4c9e-b6d5-7da25a14a9d7)

<br>

  - Add ```NIST 800-53 R5``` as a **Regulatory Compliance Standard**:

<br>

![azure portal](https://github.com/user-attachments/assets/9c1cce53-082a-4c9e-b6d5-7da25a14a9d7)

<br>

- We'll leave the Configuration Settings for the Initiative Assignment as the Default ones ➜ and click **"Create"**

<br>

![azure portal](https://github.com/user-attachments/assets/9c1cce53-082a-4c9e-b6d5-7da25a14a9d7)

<br>

The Policy Initiative Assignment takes a few minutes to take effect.

After waiting a while ➜ we can confirm that **NIST 800-53 R5** was successfully assigned to our Subscription ✅

<br>

![azure portal](https://github.com/user-attachments/assets/9c1cce53-082a-4c9e-b6d5-7da25a14a9d7)

<br>


⚠️ Again ➜ it takes some time for the policy to be added to our Environment.

But eventually it should appear inside of our MDC Dashboard ➜ under **Regulatory compliance**:

<br>

![azure portal](https://github.com/user-attachments/assets/9c1cce53-082a-4c9e-b6d5-7da25a14a9d7)

<br>


  <details close> 
  
**<summary> 📝 Context</summary>**

Basically NIST 800-53 contains a whole bunch of control families with different Controls that you can apply to your Environment.

NIST 800-53 is not specific to any Environment ➜ they're just generalized Controls.

But Microsoft created this NIST 800-53 Policy to map those General Controls to things that we can actually do inside of Azure.

This are the general Control Families defined by NIST:

<br>

![azure portal](https://github.com/user-attachments/assets/9c1cce53-082a-4c9e-b6d5-7da25a14a9d7)

<br>

If we expand one of the Control Families ➜ **“AC. Access Control”** for example:

It shows what we can do inside of the Azure Portal to our Resources to be “Compliant” with each individual Sub-Control:

<br>

![azure portal](https://github.com/user-attachments/assets/9c1cce53-082a-4c9e-b6d5-7da25a14a9d7)

<br>

Again as an example: for **IR-6(2)** as defined by NIST ➜ there could be some improvements made in our Environment:

<br>

![azure portal](https://github.com/user-attachments/assets/9c1cce53-082a-4c9e-b6d5-7da25a14a9d7)

<br>

We can click on the first **Automated assessment** ➜ and MDC will give us Instructions on how to remediate this issue:

<br>

![azure portal](https://github.com/user-attachments/assets/9c1cce53-082a-4c9e-b6d5-7da25a14a9d7)

<br>

If we expand the ```∨ Remediation steps``` ➜ it'll give us the Steps to Take in order to Manually Remidiate this Sub-Control.

  </details>



➡️ In our upcoming lab ➜ we're going to implement **SC-7 Boundary Protection**, in order to bring this Control "Up to Compliance".

<br>

<h2></h2>

  </details>

<br>

<br>

<br>

<h1>Summary</h1>
<br>

To recap:

In this lab we Enabled NIST 800-53 Regulatory Compliance Policy.

Basically when we did that ➜ Azure looked at our entire Environment and compared it to NIST 800-53 General Control Families:

<br>

![azure portal](https://github.com/user-attachments/assets/9c1cce53-082a-4c9e-b6d5-7da25a14a9d7)

<br>

In doing so, it essentially found all the weak spots in our Environment that could be addressed by the **NIST 800-53 Control Catalog**.

And in the next lab we'll start remediating a few of these **Compliance Controls**.


<br>

<br>

<br>

<br>

<br>

<br>

<br>
  
<br>
