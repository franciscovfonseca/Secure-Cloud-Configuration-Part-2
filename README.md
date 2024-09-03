<h1 align="center">Secure Cloud Configuration - Part 2</h1>

<br>

<h2 align="center">Securing our Resources: NIST 800-53 SC-7</h2>

<br>

<p align="center">
<img width="900" src="https://github.com/user-attachments/assets/53d48cbe-2efe-4d07-9540-8a393bcacf84" alt="Banner"/>

<br>

<br>

In the previous labs we performed some **Incident Response**, and we even **Remediated some Vulnerabilities** like Hardening the NSGs.

We also **Captured our Environment's Traffic** in an Insecured State for 24 hours & **Recorded our Security Metrics**.

<br>

In this next step of our Project we’ll work towards **Securing our Environment**.

<br>

Basically in this lab we’re going to:

  -	Look at Microsoft Defender for Cloud’s **Secure Score**.
    
  -	Look at Microsoft Defender for Cloud’s **Recommendations**, and learn what that is.
    
  -	Enable MDC’s **Regulatory Compliance for NIST 800-53**.

<br>

>   <details close> 
>   
> **<summary> 💭 Reminder</summary>**
> 
> If you remember ➜ **NIST 800-53** is the **Security and Privacy Control Family**.
> 
> It’s basically just a really large **Catalog of Controls** that we can use to either **Evaluate** or help **Secure our Environment**.
> 
> When we enable the **NIST 800-53 Regulatory Compliance** in **Azure**:
>   
>  - It’s going to show us things that we can do in Azure to **Secure our Environment** ➜ that align with **NIST 800-53 Controls**.
> 
> It gives us a better intuition of What NIST 800-53 is, How it’s Used and Why it’s Useful.
>   
>   </details>

<br>

<br>

<details close> 
<summary> <h2>1️⃣ Inspect MDC Secure Score & Recommendations</h2> </summary>
<br>

The first thing we’re going to do is Inspect the [**Microsoft Defender for Cloud Secure Score**](https://learn.microsoft.com/en-us/defender-xdr/microsoft-secure-score?view=o365-worldwide)

We’ll go to the **Azure Portal** ➜ open **Microsoft Defender for Cloud**:

<br>

![azure portal](https://github.com/user-attachments/assets/9c1cce53-082a-4c9e-b6d5-7da25a14a9d7)

<br>

>   <details close> 
>   
> **<summary> 💡 </summary>**
> 
> Defender for Cloud provides us with a 🛡️ **Secure Score**.
> 
> It’s a single metric that we can use to gage our **Security Posture** and how good it is.
>   
>   </details>

<br>

Then we can click on the **Recommendations** blade on the left side:

<br>

![azure portal](https://github.com/user-attachments/assets/9c1cce53-082a-4c9e-b6d5-7da25a14a9d7)

<br>

The interface will show us all the things that are contributing to our Environment’s **Security Score**.

<br>

  <details close> 
  
**<summary> 💡 </summary>**

It’ll show us areas that essentially have gaps:

➡️ So we can implement things in Azure & perform some Configurations in order to make our Security Score go Up.

<br>

  </details>

<br>

![azure portal](https://github.com/user-attachments/assets/9c1cce53-082a-4c9e-b6d5-7da25a14a9d7)

<br>

🔍 We can look through all the different **Recommendations** and get a good sense of what we can do to **Harden our Environment**.

<br>

<h2></h2>
<br>

<h3>Inspect MDC Recommendations</h3>

<br>

If we click on ```View recommendations >```:

<br>

![azure portal](https://github.com/user-attachments/assets/9c1cce53-082a-4c9e-b6d5-7da25a14a9d7)

<br>

It'll show all the things that are contributing to our **Secure Score** ➜ areas in our Environment that essentially have "Gaps".

<br>

![azure portal](https://github.com/user-attachments/assets/9c1cce53-082a-4c9e-b6d5-7da25a14a9d7)

<br>

We can implent things in **Azure** ➜ make some Configurations to get our **Secure Score** to go Up.

For example: under ```> Restrict unauthorized network access``` ➜ if we click on the first Recommendation:

<br>

![azure portal](https://github.com/user-attachments/assets/9c1cce53-082a-4c9e-b6d5-7da25a14a9d7)

<br>

MDC will give us Recommendations on how we can fix it:

- There's a detailed **Description** of what the Recomendation involves.
  
- And there's also the **Remediation Steps** section that breaks down what we have do to Remediate the issue.

<br>

![azure portal](https://github.com/user-attachments/assets/9c1cce53-082a-4c9e-b6d5-7da25a14a9d7)

<br>

✅ So we've analysed the **Recommendations** that when fixed would improve our Environmen's **Secure Score**.

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
