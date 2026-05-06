# Lab 07 - Mitigate threats using Microsoft 365 Defender 

## Estimated Duration: 60 minutes

You're a Security Operations Analyst working at a company that implemented Microsoft Defender XDR solutions. You need to see the alerts in an incident to see the incident's full impact do a root cause investigation and mitigate these alerts using M365 Defender tools.

## Lab objectives

In this lab, you will perform the following in the M365 Defender portal:
- Task 1: Onboard a Device 
- Task 2: Manage Incidents
- Task 3: Investigate Alerts

### Task 1: Onboard a Device

In this task, you will onboard a device to Microsoft Defender for Endpoint using an onboarding script.

1. Open a new tab and navigate to the **Microsoft Defender** portal by following the link:

    ```
    https://security.microsoft.com
    ```

1. If a pop-up introducing the improved security center appears after signing in, click the **X** in the top-right corner to skip the tour.

   ![](../media/t3_g_e1_1.png)

1. Navigate to **System (1)** in drop down select **Settings (2)** in the left menu bar, and then, on the Settings page, choose **Endpoints (3)**.

   ![](../media/l1211.png)

   > **Note:** The **Endpoints** option under **Settings** may take a few moments to appear after the initial setup.  
   > If you don't see it navigate to [https://security.microsoft.com/securitysettings/endpoints/integration](https://security.microsoft.com/securitysettings/endpoints/integration) to go to the Endpoints page.

1. Navigate to the **Onboarding (1)** option in the Device Management section and on the **Endpoints** page and  and configure the following: 
   - Select **Windows Server 2019, 2022, and 2025 (2)** from the operating system drop-down.  
   - In the **Connectivity type** drop-down, select **Standard (3)**.  
   - In the **Deployment method** drop-down, choose **Local Script (for up to 10 devices) (4)**.  
   - Click **Download onboarding package (5)**.

      ![](../media/t3_g_e1_2.png) 
   > **Note:** Device onboarding can also be initiated from the **Assets** section on the left menu bar. Expand 'Assets' and choose 'Devices.' On the Device Inventory page, with 'Computers & Mobile' selected, scroll down to find the option for **Onboard devices.** Clicking on this option will direct you to the **Settings > Endpoints** page.

1. In the **Downloads** pop-up:  
   - Select the **WindowsDefenderATPOnboardingPackage.zip** file.  
   - Click the folder icon to choose **Show in folder**.  
   - **Hint:** If you cannot locate the file, check the `C:\Users\admin\Downloads` directory.

      ![](../media/l1213.png)

1. Right-click on the downloaded zip file, choose **Extract All...**, ensure that **Show extracted files when complete** is checked, and then click **Extract**.

   ![](../media/t3_g_e1_4.png) 

1. Right-click on the extracted file `WindowsDefenderATPLocalOnboardingScript.cmd` and choose **Properties**. Tick the **Unblock** checkbox located in the bottom right of the Properties window, and then click **OK**.

   ![](../media/sc200-mod2-unblock.png) 

1. Once again, right-click on the extracted file **WindowsDefenderATPLocalOnboardingScript.cmd** and opt for **Run as Administrator**.

   ![](../media/l1216.png)

   > **Hint:** If the Windows SmartScreen window appears, click on **More info**, and then select **Run anyway**.

1. When the "User Account Control" window appears, select **Yes** to allow the script to run, answer **Y** to the question presented by the script, and press **Enter**. Once complete, you should see a message in the command screen that says *Successfully onboarded machine to Microsoft Defender for Endpoint*.

1. Press any key to continue. This action will close the Command Prompt window.

   ![](../media/SC-200-img25.png)

1. Back on the Onboarding page within the Microsoft Defender XDR portal, navigate to the "2. Run a detection test" section, and copy the detection test script by clicking the **Copy** button.

   ![](../media/l1219.png) 

1. In the Windows search bar of the virtual machine, type **cmd (1)**, right-click **Command Prompt (2)**, and select **Run as administrator (3)**.

   ![](../media/t3_g_e1_5.png) 

1. When the "User Account Control" window appears, select **Yes** to allow the app to run. 

1. Paste the script by right-clicking in the **Administrator: Command Prompt** window and press **Enter** to run it.

   ![](../media/t3_g_e1_6.png) 

   > **Note:** The window closes automatically after running the script.

1. In the Microsoft Defender XDR portal, navigate to the left-hand menu, and under the **Assets (1)** area, select **Devices (2)**. If the device is not shown, proceed with the next task and return to check it later. It can take up to 60 minutes for the first device to be displayed in the portal.

   ![](../media/l1223.png) 

   > **Note:** If you have completed the onboarding process and don't see devices in the Devices list after an hour, it might indicate an onboarding or connectivity problem.

<validation step="31d7f5a4-6b5f-4ac5-9ec2-b0a6150b59eb" />

> **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
> - Hit the Validate button for the corresponding task.
> - If you receive a success message, you can proceed to the next task.
> - If not, carefully read the error message and retry the step, following the instructions in the lab guide. 
> - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help you out.

### Task 2: Manage Incidents

In this task, you will manage the incidents in the M365 Defender portal.

1. If you are not already at the Microsoft 365 Defender portal in your Microsoft Edge browser, go to (https://security.microsoft.com). 

1. In the **Sign in** dialog box, copy and paste * Email/Username: <inject key="AzureAdUserEmail"></inject> and then select Next.

1. In the **Enter password** dialog box, copy and paste * Password: <inject key="AzureAdUserPassword"></inject> and then select **Sign in**.

1. From the sidebar menu, under **Incidents and Alerts**, select **Incidents**. Click on the incident **Created**.

    >**Note:** It may take 24-48 hours for incidents to appear in the Defender portal. If they are not generated within the lab timeframe, you can follow the steps below for an overview on how to manage incidents. Alternatively, you can visit the **Alerts** page under **Incidents and Alerts** to check for any alerts related to the incident, as alerts might be generated during the lab timeframe.

1. To manage an incident, click on **Manage Incident** to edit the details of this incident.

    ![Lab overview.](../media/lab10-task1-manage.png) 

1. Here, you can edit the name of the incident, add tags, assign it to an existing group or a user, change the status, classify the incident as required, and even add comments.

    ![Lab overview.](../media/lab10-task1-manage01.png)

1. In the incident, the **Attack Story** tab provides a summary of the alerts and the incident graph on how these alerts are mapped.

    ![Lab overview.](../media/lab10-task1-attackstory.png)

1. You can further investigate these alerts by navigating to the **Alerts** tab.

    ![Lab overview.](../media/lab10-task1-alerts.png)

1. You can also see the devices and users affected by this incident in the **Assets** tab. You can verify that the affected device is **svm-<inject key="DeploymentID" enableCopy="false" />** and the user is **demouser**.

   ![Lab overview.](../media/lab10-task1-assests.png)

1. The **Evidence & Responses** tab shows the initial evidence investigated by Microsoft Defender which includes the processes, IP addresses, and registry values.

    ![Lab overview.](../media/lab10-task1-evidences.png)

1. The **Summary** tab gives us a summarized report of the incident including active alerts & their category, incident information, scope, and much more.

    ![Lab overview.](../media/lab10-task1-summary.png)

### Task 3: Investigate Alerts

In this task, you will investigate and mitigate the alerts through recommendations by Microsoft Defender.

1. In the Microsoft Defender portal, navigate to the **Alerts** tab from the sidebar menu.

    ![Lab overview.](../media/lab10-task2-alerts.png)

1. You can click on any of these alerts to view the full details. Click on the alert named **Suspicious PowerShell command line**.

1. Click on **Maximize** to view the full alert details.

    ![Lab overview.](../media/lab10-task2-alerts-max.png)

1. Click on the drop-down for the first suspicious behavior to fully investigate the root cause for this activity.

    ![Lab overview.](../media/lab10-task2-alerts-max01.png)

1. You can see that this suspicious behavior was reported when the user ran a certain command. 

    ![Lab overview.](../media/lab10-task2-alerts-max02.png)

1. Click on the ellipses and then select **Go Hunt**. This will redirect you to a new tab of **Advanced Hunting** where you can run the query and get the results.

    ![Lab overview.](../media/lab10-task2-alerts-hunt.png)

    ![Lab overview.](../media/lab10-task2-alerts-hunt01.png)

1. You can also investigate the alert further by navigating back to the alerts and clicking on **Deep analysis**.

    ![Lab overview.](../media/lab10-task2-alerts-deep-analysis.png)

1. You will be redirected to a new tab. Click on **Submit** to get the detailed analyzed file.

    ![Lab overview.](../media/lab10-task2-alerts-deep-analysis01.png)

1. This process will take some time, after which you can see the deep analysis of the alert and further investigate it.

    ![Lab overview.](../media/lab10-task2-alerts-deep-analysis02.png)

1. Microsoft Defender also provides recommendations to mitigate the alerts. On the alert details page, click on the **Recommendations** tab to view all the recommendations.

    ![Lab overview.](../media/lab10-task2-alerts-recommendations.png)

## Summary

In this lab, you utilized Microsoft 365 Defender to onboard devices, manage incidents, and investigate alerts. You navigated the Microsoft 365 Defender portal to onboard endpoints, reviewed and managed security incidents, and analyzed alerts to identify potential threats. By executing these tasks, you successfully mitigated security risks and enhanced your organization's security operations, demonstrating effective use of Microsoft Defender tools in real-world scenarios.

## You have successfully completed all the labs.
