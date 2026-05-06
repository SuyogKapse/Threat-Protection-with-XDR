# Lab 03 - Conduct attacks

### Estimated Duration: 90 minutes

In this lab, you will simulate various attacks to detect and investigate in Microsoft Defender. Tasks include connecting Windows security events, enabling Microsoft Defender for Cloud, performing attacks such as persistence with registry keys, command and control via DNS, privilege escalation with user addition, and creating a playbook in Microsoft Sentinel.

## Lab objectives
 In this lab, you will perform the following:
- Task 1: Connect the Windows security event connector
- Task 2: Enable Microsoft Defender for Cloud
- Task 3: Persistence Attack with Registry Key Add
- Task 4: Command and Control Attack with DNS
- Task 5: Privilege Elevation Attack with User Add
- Task 5: Playbook Creation

### Task 1: Connect the Windows security event connector

In this task, you will configure Microsoft Sentinel to monitor Windows security events by installing **Windows Security Events** and setting up the **Security Events Via Legacy Agent** connector. You'll install the agent on an Azure VM, select **All Events** to stream, and verify the connection.

1. Open a new tab and navigate to the **Microsoft Defender portal** by following the link:

    ```
    https://security.microsoft.com
    ```

1. If a pop-up introducing the improved security center appears after signing in, click the **X** in the top-right corner to skip the tour.

   ![](../media/t3_g_e1_1.png)

1. On the **Microsoft Defender** portal, in the left navigation pane:
   - Select **Microsoft Sentinel (1)**.
   - Expand **Content management (2)** and select **Content hub (3)**.

      ![](../media/t3_g_e1_22.png)

1. On the **Content hub** page, in the search bar, type **Windows Security Events (1)**, then select the checkbox for **Windows Security Events (2)** from the results.

   ![](../media/cor_r_g_3.png)

1. On the **Windows Security Events** solution page, click **Install**.

   ![](../media/cor_r_g_4.png)

1. After receiving the notification of a successful installation, return to the Data Connector page and click on the refresh button to ensure that the changes take effect.

1. You should observe two options: **Security Events Via Legacy Agent** and **Windows Security Event Via AMA**.

1. On the left navigation pane, expand **Configuration (1)**, select **Data connectors (2)**, and in the search bar, type **Security Events via Legacy Agent (3)**. From the results, select **Security Events via Legacy Agent (4)**.

   ![](../media/cor_r_g_5.png)

1. On the **Security Events via Legacy Agent** page, click **Open connector page**.

   ![](../media/cor_r_g_6.png)

1. In the configuration section, opt for **Install Agent on Azure Windows Virtual Machine (1)**, and then choose **Download & Install Agent for Azure Windows Virtual Machines (2)**.

   ![](../media/t3_g_e2_7.png)

1. Select the **svm-<inject key="DeploymentID" enableCopy="false" />** virtual machine.

   ![](../media/t3_g_e2_8.png)

1. On the virtual machine page, click **Connect** to link the VM to Log Analytics.

   ![](../media/t3_g_e2_9.png)

1. Select the **Virtual Machine** link from the top.

   ![](../media/t3_g_e2_10.png)

1. On the virtual machine page select the **s2vm-<inject key="DeploymentID" enableCopy="false" />** virtual machine.

   ![](../media/t3_g_e2_11.png)

1. On the second virtual machine page, click **Connect** to link the VM to Log Analytics.

   ![](../media/t3_g_e2_12.png)

1. Select the **Virtual Machine** link from the top.

   ![](../media/t3_g_e2_13.png)

1. Verify that both virtual machines **s2vm-<inject key="DeploymentID" enableCopy="false" />** and **svm-<inject key="DeploymentID" enableCopy="false" />** display **This workspace (1)** under the **Log Analytics Connection** column, then click **Security Events via Legacy Agent (2)** in the breadcrumb to return to the connector page.

   ![](../media/t3_g_e2_14.png)

1. In the **Instructions** section, under **Select which events to stream**, choose **All Events (1)** and click **Apply changes (2)**.

   ![](../media/t3_g_e2_15.png)

1. Click on Apply Changes now. If you refresh the data connector page, you can see the status Connected for **Security Events Via Legacy Agent**.

<validation step="13a30bee-c2ea-4d84-aff5-94791bc2ec08" />

> **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
> - Hit the Validate button for the corresponding task.
> - If you receive a success message, you can proceed to the next task.
> - If not, carefully read the error message and retry the step, following the instructions in the lab guide. 
> - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help you out.

### Task 2: Enable Microsoft Defender for Cloud

In this task, you will enable and configure Microsoft Defender for Cloud.

1. Go to the [Azure Portal](https://portal.azure.com), search and navigate to **Microsoft Defender for Cloud**.

   ![](../media/gg-1-1.png)

1. When prompted, click **Enable** to activate Defender CSPM followed by **Accept**.

   ![](../media/gg-1-2.png)

   > **Note:** If you don’t see the pop-up prompt, simply continue and follow the lab guide steps as shown below.

   >**Note:** This enables advanced posture capabilities like attack path analysis and permissions management.

1. In the **Microsoft Defender for Cloud** page, under **Management**, select **Environment settings (1)**, scroll down, expand **Azure** and **Tenant Root Group**, then select **Subscription (2)**.

   ![](../media/gg-1-3.png)

1. On the **Settings | Defender plans** page, turn **On (1)** the toggle for **Foundational CSPM** and **On (2)** for **Servers** under Cloud Workload Protection, then click **Save (3)**.

   ![](../media/l1204.png)

1. Click **Environment settings** in the top to return to the environment settings page.

   ![](../media/t3_g_e2_17.png)

1. On the **Environment settings** page, expand **Azure (1)**, then expand **Subscription** and select **loganalycticworkspace (2)**.

   ![](../media/t3_g_e2_18.png)

1. On the **Select Defender plan** page, turn **On (1)** the toggles for **Foundational CSPM** and **Servers**, then click **Save (2)**.

   ![](../media/t3_g_e2_19.png)

1. Close the Defender plans page by selecting the 'X' in the upper right corner of the page to return to the **Environment settings**.

### Task 3: Persistence Attack with Registry Key Add 

>**Note:** Perform this task in your LAB-VM (svm).

In this task, you will create a temporary folder and a batch file using Command Prompt, then simulate program persistence by adding the file to the Windows startup process via the registry.

> **⚠ Important Usage Guidance:** Microsoft Defender for Office 365 may take some time to load certain results or complete specific labs from the backend. This is expected behavior. If the data does not appear after a couple of refresh attempts, proceed with the next lab and return later to check the results.

1. In the search of the taskbar, enter *Command*. A Command Prompt will be displayed in the search results. Right-click on the Command Prompt and select **Run as Administrator**. Select **Yes** in the User Account Control window that allows the app to run.

1. In the Command Prompt, create a Temp folder in the root directory. Remember to press Enter after the last row:

    ```CommandPrompt
    cd \
    mkdir temp
    cd temp
    ```

    >**Note**: If there is any error on temp directory already exists, please perform the next steps.  
   
1. Copy and run this command to simulate program persistence:

    ```CommandPrompt
    REG ADD "HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Run" /V "SOC Test" /t REG_SZ /F /D "C:\temp\startup.bat"
    ```

<validation step="076f54c8-ad3c-4b19-83f8-d277d70aff76" />

> **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
> - Hit the Validate button for the corresponding task.
> - If you receive a success message, you can proceed to the next task.
> - If not, carefully read the error message and retry the step, following the instructions in the lab guide. 
> - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help you out.

### Task 4: Command and Control Attack with DNS

>**Note:** Perform this task in your LAB-VM (svm).

In this task, you will create a PowerShell script that simulates DNS queries to a C2 server and run it in the background using Command Prompt. This script will generate log entries over time for later use in the Threat Hunting lab, allowing DNS resolution errors to occur as expected.

1. Copy and run this command to create a script that will simulate a DNS query to a C2 server:

      ```CommandPrompt
      notepad c2.ps1
      ```

1. Select **Yes** to create a new file and copy the following PowerShell script into *c2.ps1*.

      >**Note:** Pasting into the virtual machine file might not show the full script length. Make sure the script matches the instructions within the *c2.ps1* file.

      ```PowerShell
      param(
        [string]$Domain = "microsoft.com",
        [string]$Subdomain = "subdomain",
        [string]$Sub2domain = "sub2domain",
        [string]$Sub3domain = "sub3domain",
        [string]$QueryType = "TXT",
        [int]$C2Interval = 8,
        [int]$C2Jitter = 20,
        [int]$RunTime = 240
    )
    $RunStart = Get-Date
    $RunEnd = $RunStart.addminutes($RunTime)
    $x2 = 1
    $x3 = 1 
    Do {
        $TimeNow = Get-Date
        Resolve-DnsName -type $QueryType $Subdomain".$(Get-Random -Minimum 1 -Maximum 999999)."$Domain -QuickTimeout
        if ($x2 -eq 3 )
        {
            Resolve-DnsName -type $QueryType $Sub2domain".$(Get-Random -Minimum 1 -Maximum 999999)."$Domain -QuickTimeout
            $x2 = 1
        }
        else
        {
            $x2 = $x2 + 1
        }    
        if ($x3 -eq 7 )
        {
            Resolve-DnsName -type $QueryType $Sub3domain".$(Get-Random -Minimum 1 -Maximum 999999)."$Domain -QuickTimeout
            $x3 = 1
        }
        else
        {
            $x3 = $x3 + 1
        }
        $Jitter = ((Get-Random -Minimum -$C2Jitter -Maximum $C2Jitter) / 100 + 1) +$C2Interval
        Start-Sleep -Seconds $Jitter
    }
    Until ($TimeNow -ge $RunEnd)
    ```

1. In the Notepad menu, select **File** and then **Save**. 

1. Go back to the Command Prompt window, enter the following command and press Enter.

      ```CommandPrompt
      Start PowerShell.exe -file c2.ps1
      ```
    
      ![Lab overview.](../media/cmd.png)
   
      >**Note:** You will see DNS resolve errors. This is expected.

      >**Important**: Do not close these windows. Let this PowerShell script run in the background. The command needs to generate log entries for some hours. You can proceed to the next task and next exercises while this script runs. The data created by this task will be used in the Threat Hunting lab later. This process will not create substantial amounts of data or processing.

### Task 5: Privilege Elevation Attack with User Add

>**Important:** The next steps are done on a different machine which is **s2vm-<inject key="DeploymentID" enableCopy="false" />** than the one you were previously working on. Look for the Virtual Machine name references.

In this task, you will connect to a virtual machine using Remote Desktop from the Azure portal, download the RDP file, and log in with provided credentials. Once connected, you will use Command Prompt to create a temporary folder and simulate the creation of an admin account.

1. In Azure portal, Search for **Virtual machines (1)** and select **Virtual machines (2)**.

   ![VMrdp](../media/vm.png)

1. Select the virtual machine **s2vm-<inject key="DeploymentID" enableCopy="false" />** from the list.
   
   ![VMrdp](../media/vm1.png)

1. At the beginning of the virtual machine page, click on **Connect**, and from the drop-down select **Connect**.

   ![VMrdp](../media/vm2.png)

1. On the **s2vm-<inject key="DeploymentID" enableCopy="false" /> | Connect** page, under **Native RDP**, choose the option to **Download RDP File**.

   > **Note**: If you see the popup on browser while downloading the file, please select the **keep** option to download the file. 

1. Open the downloaded RDP file from the downloads.

   ![VMrdp](../media/vm4.png)

1. Select **Connect** when prompted. You will get a warning that the .rdp file is from an unknown publisher. This is expected. In the Remote Desktop Connection window, select Connect to continue.

   ![VMrdp](../media/vm8.png)
   
1. In the Windows Security window, select **More Choices** and then Use a different account. Enter **Username: (1)** .\demouser and **Password: (2)** <inject key="Labvm Admin Password"></inject> and then select **OK (3)**.

   ![VMrdp](../media/img-01-001.png)

1. Select **Yes** to verify the identity of the virtual machine and finish logging on.

    ![VMrdp](../media/vm7.png)

1. You should now be connected to the virtual machine via Remote Desktop.

1. In the search of the taskbar of your **s2vm-<inject key="DeploymentID" enableCopy="false" />** VM, enter *Command*. A Command Prompt will be displayed in the search results. Right-click on the Command Prompt and select **Run as Administrator**. Select **Yes** in the User Account Control window that allows the app to run.

1. In the Command Prompt, create a Temp folder in the root directory. Remember to press Enter after the last row:

    ```CommandPrompt
    cd \
    ```
    ```CommandPrompt
    mkdir temp
    ```
    ```CommandPrompt
    cd temp
    ```

   >**Note:** If you face any issues in creating the temp folder please check temp folder is already present or not if present please delete the temp folder and perform the above step again.

1. Copy and run this command to simulate the creation of an Admin account. Remember to press Enter after the last row:

    ```CommandPrompt
    net user theusernametoadd /add
    ```
    ```CommandPrompt
    net user theusernametoadd ThePassword1!
    ```
    ```CommandPrompt
    net localgroup administrators theusernametoadd /add
    ```

### Task 6: Playbook Creation

In this task, you will create a playbook in Microsoft Sentinel by selecting the workspace, configuring a playbook named **PostMessageTeams-OnIncident**, enabling diagnostics logs, and then reviewing and creating it without modifying the Logic App designer.

1. Navigate back to the **svm-<inject key="DeploymentID" enableCopy="false" />**.

1. Open **Microsoft Defender** portal, from the left navigation menu select **Microsoft Sentinel (1)**. Expand **Configuration (2)** and select **Automation (3)**.

   ![Lab overview.](../media/img-01-002.png)

1. On the **Automation** page, click on **+ Create (1)** drop-down and select **Logic App playbook (2)** > **Playbook with incident trigger (3)**.

   ![Lab overview.](../media/img-01-003.png)

1. On the **Basic configuration page** enter the **Playbook Name** as **PostMessageTeams-OnIncident**, check the box for enabling diagnostics logs and select your workspace. Select **Next** twice and click on **Create Playbook**.

   ![Lab overview.](../media/img-01-004.png)

   > **Note**: We dont have to perform any action on logic app designer, please go back to your sentinel workspace and follow the next labguide.

## Summary

In this exercise, you simulated various security attacks using Microsoft Sentinel and Microsoft Defender for Cloud to generate incidents for investigation. Next, you will create custom analytics rules to detect persistence and privilege elevation attacks.

## You've successfully completed this lab. Click **Next** to continue with the lab.
