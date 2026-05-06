# Lab 01 - Review and explore sentinel workspace

## Lab scenario

In this lab, participants will explore a pre-configured Azure Sentinel workspace, investigating security incidents, configuring automated responses, and analyzing data to strengthen threat detection and response capabilities within the Azure environment.

You are a Security Operations Analyst working at a company that is implementing Microsoft Defender for Endpoint. Your manager plans to onboard a few devices to provide insight into required changes to the Security Operations (SecOps) team response procedures.
You start by initializing the Defender for the Endpoint environment. Next, you onboard the initial devices for your deployment by running the onboarding script on the devices.

## Lab objectives
 In this lab, you will perform the following:
 - Task 1: Log in to Azure Portal
 - Task 2: Explore Sentinel workspace
 - Task 3: Onboard a Device

## Estimated timing: 30 minutes

## Architecture Diagram

  ![Lab overview.](./media/lab-01.png)

## Exercise 1: Exploring a Pre-Created Azure Sentinel Workspace

### Task 1: Log in to Azure Portal

1. If you are not login to the azure portal then in the virtual machine (VM) on the left, click on the Azure Portal, as shown below.

   ![](./media/login1.png)
    
1. On the **Sign into Microsoft Azure** tab, you will see the login screen. Enter the following **Email/Username**, and then click on **Next**.

   * Email/Username: <inject key="AzureAdUserEmail"></inject>

     ![](./media/login2.png)

1. Enter the following **Password** and click on **Sign in**. 
   
    * Password: <inject key="AzureAdUserPassword"></inject>

      ![](./media/Lab-01-task1-password.png) 
    
1. First-time users are often prompted to Stay Signed In. If you see any such pop-up, click on No.
   
1. If a **Welcome to Microsoft Azure** popup window appears, click on Maybe Later to skip the tour.

### Task 2: Explore Sentinel workspace

 Sentinel Workspace is a centralized platform by Microsoft Azure designed for managing, investigating, and responding to security threats across your organization's cloud services.

1. In the Search bar of the Azure portal, type *Microsft Sentinel (1)*, then select **Microsoft Sentinel (2)**.

   ![](./media/09.png)

1. Select the pre-created Sentinel **loganalyticworkspace** from the available list.

   ![](./media/Lab01-task2-loganalyticworkspace.png) 

1. Explore the Overview page to obtain a summary of the workspace, including information on incidents, alerts, health, and other essential details.

   ![](./media/lab01-task2-overview.png)

1. Navigate to the **Data connectors** section in the left sidebar under **Configuration** and examine the currently configured connectors for collecting security-related data.

   ![](./media/xdr21.png)

   >**Note:** The image is for reference only; your version may differ.

1. In the **threat management** section, choose **Incidents** from the left panel to assess detected security incidents and alerts.

1. Click on the **Status (1)** filter beside the search space, and then select the **Select all (2)** checkbox to view all new, active, and closed incidents. Then click on **OK (3)**

   ![](./media/xdr3.png)

1. Select the **Informational (1)** incident from the list, then click on **View full details (2)** to view details and take necessary actions.

   ![](./media/xdr4.png) 

   > **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
   - If you receive a success message, you can proceed to the next task.
   - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
   - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help you out.
 
   <validation step="5cc49b79-e188-4127-b294-65a47ab01d3b" />

### Task 3: Onboard a Device

In this task, you will onboard a device to Microsoft Defender for Endpoint using an onboarding script.

1. If you are not already at the Microsoft 365 Defender portal in your browser, start the Microsoft Edge browser go to (https://security.microsoft.com).

1. On the **Sign into Microsoft Azure** tab, you will see the login screen. Enter the following **Email/Username**, and then click on **Next**.

   * Email/Username: <inject key="AzureAdUserEmail"></inject>

     ![](./media/login2.png)

1. Enter the following **Password** and click on **Sign in**. 
   
   * Password: <inject key="AzureAdUserPassword"></inject>

     ![](./media/Lab-01-task1-password.png) 

      >**Note:** Take a moment to allow the option panel to fully load on the security portal.

1. Navigate to **Settings (1)** in the left menu bar, and then, on the Settings page, choose **Endpoints (2)**.

    ![](./media/settings-01.png)

   >**Note:** If you encounter any issues while opening the endpoint, please use the following URL and open it in a new browser tab to access the endpoint resource: **https://security.microsoft.com/securitysettings/endpoints/onboarding**
   
1. Navigate to the **Onboarding** option in the *Device Management section.*

   ![](./media/xdr22.png)

1. Under **Select operating system to start onboarding process:** make sure to select the **Windows Server 2019, 2022, and 2025**.

   ![](./media/windows-server-2019-2025.png)

1. In the **1. Onboard a device** section, ensure that **Local Script (for up to 10 devices) (2)** is visible in the Deployment method drop-down, then click the **Download onboarding package (3)** button.

    ![](./media/lab01-task3-localscript.png) 

1. In the *Downloads* pop-up, use your mouse to select the **WindowsDefenderATPOnboardingPackage.zip** file, and then click on the folder icon for **Show in folder**. **Hint:** If you can't locate it, the file should be in the 'c:\users\admin\downloads' directory.

    ![](./media/xdr5.png)

1. Right-click on the downloaded zip file, choose **Extract All...**, ensure that **Show extracted files when complete** is checked, and then click **Extract**.

    ![](./media/lab01-task3-zipfile.png) 

1. Right-click on the extracted file **WindowsDefenderATPLocalOnboardingScript.cmd** and choose **Properties**. Tick the **Unblock (1)** checkbox located in the bottom right of the Properties window, and then click **OK (2)**.

    ![](./media/xdr7.png) 

1. Once again, right-click on the extracted file **WindowsDefenderATPLocalOnboardingScript.cmd** and opt for **Run as Administrator**. **Hint:** If the Windows SmartScreen window appears, click on **More info**, and then select **Run anyway**.
    
1. When the "User Account Control" window appears, select **Yes** to allow the script to run, answer **Y** to the question presented by the script, and press **Enter**. Once complete, you should see a message in the command screen that says *Successfully onboarded machine to Microsoft Defender for Endpoint*.

1. Press any key to continue. This action will close the Command Prompt window.

    ![](./media/SC-200-img25.png)

1. Back on the Onboarding page within the Microsoft 365 Defender portal, navigate to the **2. Run a detection test** section, and copy the detection test script by clicking the **Copy** button.

    ![](./media/lab01-task3-runscript.png) 

1. In the Windows search bar of the virtual machine, type **CMD (1)**, and choose **Run as Administrator (2)** from the right pane for the Command Prompt app.

    ![](./media/xdr6.png) 

1. When the "User Account Control" window appears, select **Yes** to allow the app to run. 

1. Paste the script by right-clicking in the **Administrator: Command Prompt** window and press **Enter** to run it. **Note:** The window closes automatically after running the script.

1. In the Microsoft 365 Defender portal, navigate to **Endpoints** and select **Onboarding**. If the status is **Incomplete**, proceed with the next task and check back later, as it may take 24–48 hours to update.
    
## Review
In this lab you have completed the following tasks:
 - Log in to Azure Portal
 - Explore Sentinel workspace
 - Onboard a Device
