# Threat Protection with XDR

### Overall Estimated Duration: 4 Hours

## Overview

Threat Protection with Extended Detection and Response (XDR) is a game-changing paradigm in cybersecurity, providing an unparalleled shield against modern threats. Unlike conventional security measures, XDR doesn’t just stop at surface-level defense; it orchestrates a symphony of security layers, unifying disparate tools and technologies to create an impenetrable fortress for an organization.

## Objectives

- **Review and explore sentinel workspace**: In this lab, you will explore a pre-configured Azure Sentinel workspace, investigating security incidents, configuring automated responses, and analyzing data to strengthen threat detection and response capabilities within the Azure environment.
- **Integrate Logic App with Threat Protection and XDR**
- **Conduct attacks**
- **Create Detections**
- **Investigate an Incident**
- **Threat Hunting using Notebooks with Microsoft Sentinel**
- **Mitigate threats using Microsoft 365 Defender**

## Prerequisites

Participants should have:

- **Familiarity with Azure Sentinel and Defender for Cloud**: Understanding of Microsoft Sentinel and Microsoft Defender for Cloud capabilities, including security incident management and cloud security monitoring.
- **Experience with Azure Portal and Logic Apps**: Familiarity with navigating the Azure Portal and using Azure Logic Apps for automation and workflow management.
Basic Security Operations Knowledge: Proficiency in security operations, including working with security incidents, alerts, and configuring threat detection tools.
- **Basic Security Operations Knowledge**: Proficiency in security operations, including working with security incidents, alerts, and configuring threat detection tools.

## Explanation of Components

The architecture for this lab involves the following key components:

- **Azure Sentinel**: Azure Sentinel is a cloud-native SIEM (Security Information and Event Management) solution that provides intelligent security analytics and threat intelligence across your enterprise. It enables you to collect, analyze, and respond to security incidents in real time.
- **Microsoft Defender for Cloud**: This service enhances your security posture by providing continuous security assessment and recommendations for Azure resources, helping to protect against threats and vulnerabilities.
- **Logic Apps**: Azure Logic Apps is a cloud service that allows you to automate workflows and integrate applications and data across various systems. In this lab, it is used to create automated responses to security incidents and alerts.
- **Windows Security Event Connector**: This connector facilitates the collection of security events from Windows systems, enabling the monitoring and analysis of potential threats and incidents within the Azure environment.
- **Microsoft Teams**: Microsoft Teams serves as a collaboration platform where the Security Operations Center (SOC) team can communicate and manage alerts and incidents effectively, streamlining incident response efforts.
- **Playbooks in Microsoft Sentinel**: Playbooks are workflows built using Logic Apps that automate responses to incidents, allowing for quick and consistent actions based on predefined rules and conditions.

## Getting Started with the Lab
 
Welcome to your Threat protection with XDR workshop! We've prepared a seamless environment for you familiarize yourself with the Microsoft security operations analyst, you monitor, identify, investigate, and respond to threats in multicloud environments and related Microsoft services. Let's begin by making the most of this experience:
 
## Accessing Your Lab Environment
 
Once you're ready to dive in, your virtual machine and lab guide will be right at your fingertips within your web browser.
 
![Access Your VM and Lab Guide](./media/labguide-0123.png)

### Virtual Machine & Lab Guide
 
Your virtual machine is your workhorse throughout the workshop. The lab guide is your roadmap to success.
 
## Exploring Your Lab Resources
 
To get a better understanding of your lab resources and credentials, navigate to the **Environment** details tab.
 
![Explore Lab Resources](./media/img-01-02.png)
 
## Utilizing the Split Window Feature
 
For convenience, you can open the lab guide in a separate window by selecting the **Split Window** button from the Top right corner.
 
![Use the Split Window Feature](./media/img-01-03.png)
 
## Managing Your Virtual Machine
 
Feel free to start, stop, or restart your virtual machine as needed from the **Resources** tab. Your experience is in your hands!
 
![Manage Your Virtual Machine](./media/img-04.png)

## Lab validation

1. After completing the task, hit the Validate button under Validation tab integrated within your lab guide. If you receive a success message, you can proceed to the next task, if not, carefully read the error message and retry the step, following the instructions in the lab guide.

   ![](./media/update03.png)

1. If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com.

## Login to the Azure Portal
 
1. In the JumpVM, click on the Azure portal shortcut of the Microsoft Edge browser, which is created on the desktop.
 
    ![Launch Azure Portal](./media/img-05.png)

1. On the Sign in to Microsoft Azure tab, you will see the login screen. Enter the following email or username, and click on Next.
 
   - **Email/Username:** <inject key="AzureAdUserEmail"></inject>
 
     ![Enter Your Username](./media/sc900-image-1.png)
 
1. Next, provide your password:
 
   - **Temporary Access Pass:** <inject key="AzureAdUserPassword"></inject>
 
     ![Enter Your Password](./media/password-1211.png)

1. If you see the pop-up Stay Signed in?, select No.

1. If you see the pop-up **You have free Azure Advisor recommendations!**, close the window to continue the lab.

1. If a Welcome to **Microsoft Azure** popup window appears, select **Maybe Later** to skip the tour.

1. Now that you will see the Azure Portal Dashboard, click on Resource groups from the Navigate panel to see the resource groups.

   ![](./media/update05.png)  
 
In this hands-on lab, you'll explore Azure Sentinel and Microsoft Defender for Cloud while integrating Logic Apps for automated incident responses, enhancing your security operations and threat detection capabilities.

## Support Contact

The CloudLabs support team is available 24/7, 365 days a year, via email and live chat to ensure seamless assistance at any time. We offer dedicated support channels tailored specifically for both learners and instructors, ensuring that all your needs are promptly and efficiently addressed.

Learner Support Contacts:

 - Email Support: cloudlabs-support@spektrasystems.com
 - Live Chat Support: https://cloudlabs.ai/labs-support

Now, click on Next from the lower right corner to move on to the next page.

![Start Your Azure Journey](./media/img-06.png)

## Happy Learning!!
