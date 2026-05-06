# Lab 06 - Threat Hunting using Notebooks with Microsoft Sentinel

## Estimated Duration: 120 minutes

You are a Security Operations Analyst working at a company that implemented Microsoft Sentinel. You have received threat intelligence about a Command and Control (C2 or C&C) technique.  You need to perform a hunt and watch for the threat.

## Lab objectives
In this lab, you will perform the following:
- Task 1: Connect the Windows security event connector
- Task 2: Enable Microsoft Defender for Cloud
- Task 3: Persistence Attack with Registry Key Add
- Task 4: Command and Control Attack with DNS
- Task 5: Privilege Elevation Attack with User Add
- Task 6: Create a hunting query
- Task 7: Create a NRT query rule
- Task 8: Create a Search
- Task 9: Explore Notebooks

### Task 1: Connect the Windows security event connector

In this task, you'll configure the connector to ensure seamless log transmission and enhance your security monitoring capabilities.

1. Open a new tab and navigate to the **Microsoft Defender** portal by following the link:

    ```
    https://security.microsoft.com
    ```

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

In this task, you will create a temporary folder and a batch file using Command Prompt, then simulate program persistence by adding the file to the Windows startup process via the registry.

>**Note:** Perform this task in your LAB-VM (svm).

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

### Task 6: Create a hunting query

> **⚠ Important Usage Guidance:** Microsoft Defender for Office 365 may take some time to load certain results or complete specific labs from the backend. This is expected behavior. If the data does not appear after a couple of refresh attempts, proceed with the next lab and return later to check the results.

1. In the Search bar of the Azure portal, type **Microsoft Sentinel (1)**, then select **Microsoft Sentinel (2)**.

   ![](../media/l1801.png)
   
1. Select the Microsoft Sentinel Workspace you created earlier.

   ![](../media/l1802.png)

1. Select **Logs** from the *General* section.

   ![](../media/cor_r_g_7.png)

    >**Note:** You might see some popup after clicking on **Logs**. Close all popups by clicking on the **X** icon.

    ![](../media/l1803(N).png)

1. Shift **Simple mode (1)** to **KQL mode (2)** and enter the following KQL Statement in the New Query 1 space:

   ![](../media/l1903.png)

   >**Important:** Please paste any KQL queries first in Notepad and then copy from there to the New Query 1 Log window to avoid any errors.

    ```KQL
    let lookback = 2d; 
    SecurityEvent 
    | where TimeGenerated >= ago(lookback) 
    | where EventID == 4688 and Process =~ "powershell.exe"
    | extend PwshParam = trim(@"[^/\\]*powershell(.exe)+" , CommandLine) 
    | project TimeGenerated, Computer, SubjectUserName, PwshParam 
    | summarize min(TimeGenerated), count() by Computer, SubjectUserName, PwshParam 
    | order by count_ desc nulls last 
    ```

1. Review the different results. You have now identified PowerShell requests that are running in your environment.

    ![](../media/lab9xdr1.png)

1. Select the checkbox of the results that shows the **demouser** SubjectUserName.

1. In the middle command bar, select the **Add bookmark** button.

   ![](../media/l1907.png)

1. Select **+ Add new entity** under Entity mapping.

1. For *Entity* select **Host**, then **HostName** and **Computer** for the values.

1. In Tactics and Techniques, select **Command and Control** from the dropdown that appears.

1. Go back to the Add bookmark blade, and then select **Create**. We will map this bookmark to an incident later.

   ![](../media/l1911.png)

1. Close the Logs window by selecting the **X (1)** in the top-right of the window and select **OK (2)** to discard the changes. 

   ![](../media/l1912.png)

1. On the **Microsoft Sentinel** page, under **Threat management (1)**, select **Hunting (2)**, then click the **Queries (3)** tab.

   ![](../media/ex3_g_tr_6.png)

1. Select the **Queries (1)** tab and then **+ New Query (2)** from the command bar.

   ![](../media/t3_g_e2_21.png)

1. In the **Name** field, enter **PowerShell Hunt (1)**, and in the **Query** field, paste the provided KQL query **(2)**.

    - For the Custom query enter the following KQL statement:

    ```KQL
    let lookback = 2d; 
    SecurityEvent 
    | where TimeGenerated >= ago(lookback) 
    | where EventID == 4688 and Process =~ "powershell.exe"
    | extend PwshParam = trim(@"[^/\\]*powershell(.exe)+" , CommandLine) 
    | project TimeGenerated, Computer, SubjectUserName, PwshParam 
    | summarize min(TimeGenerated), count() by Computer, SubjectUserName, PwshParam 
    | order by count_ desc nulls last 
    ```

   ![](../media/t3_g_e2_22.png)

1. Under **Entity mapping**, select **Host**, then set **HostName** as the identifier and **Computer (1)** as the value. Click **Create (2)** to save the hunting query.

   ![](../media/t3_g_e2_23.png)

1. Verify that a notification appears confirming the hunting query **'PowerShell Hunt'** was successfully created.

   ![](../media/t3_g_e2_24.png)

1. Confirm that the newly created hunting query **PowerShell Hunt** is now visible in the **Queries** list under Microsoft Sentinel Hunts.

   ![](../media/t3_g_e2_25.png)

1. On the **PowerShell Hunt** details page, review the query configuration and click **View results** to see the hunting query output.

   ![](../media/t3_g_e2_26.png)

1. On the **Hunting** page, select **PowerShell Hunt (1)** from the list of queries, click on the vertical ellipsis menu, and choose **Add to livestream (2)** to monitor the query in real-time.

   ![](../media/t3_g_e2_27.png)

1. On the **Bookmarks (1)** tab, select the checkbox for the relevant bookmark entry **(2)**, then click **Investigate (3)** to analyze the bookmarked event in detail.

   ![](../media/t3_g_e2_28.png)

1. On the **Bookmarks** tab, click the **More actions (1)** icon for the relevant bookmark and select **Add to existing incident (2)** from the menu.

   ![](../media/t3_g_e2_29.png)

1. On the **Adding bookmark(s) to an existing incident** pane, select the incident **Multi-stage i... (1)** and click **Add (2)**.

   ![](../media/t3_g_e2_30.png)

### Task 7: Create a NRT query rule

In this task, instead of using a LiveStream, you will create a NRT analytics query rule. NRT rules run every minute and look back one minute. The benefit of NRT rules is they can use the alert and incident creation logic.

1. In **Microsoft Sentinel**, under **Configuration (1)** select **Analytics (2)**, then click **+ Create (3)** and choose **NRT query rule (4)**.

   ![](../media/l19NRT05.png)

1. On the **General** tab:  
    - Enter **NRT PowerShell Hunt (1)** in the **Name** field.  
    - Enter **NRT PowerShell Hunt (2)** in the **Description** field.  
    - Set **Severity** to **High (3)**.  
    - Set **MITRE ATT&CK** to **Command And Control (4)**.  
    - Ensure **Status** is set to **Enabled (5)**.  
    - Click **Next: Set rule logic > (6)**. 

      ![](../media/ex3_g_tr_8.png) 

1. For the *Rule query* enter the following KQL statement:

    ```KQL
    let lookback = 2d; 
    SecurityEvent 
    | where TimeGenerated >= ago(lookback) 
    | where EventID == 4688 and Process =~ "powershell.exe"
    | extend PwshParam = trim(@"[^/\\]*powershell(.exe)+" , CommandLine) 
    | project TimeGenerated, Computer, SubjectUserName, PwshParam 
    | summarize min(TimeGenerated), count() by Computer, SubjectUserName, PwshParam
    ```

1. Under Entity mapping select:
     
    - Select **+ Add new entity** under Entity mapping.
    - For the Entity type drop-down list select **Host**.
    - For the Identifier drop-down list select **HostName**.
    - For the Value drop-down list select **Computer**.

      ![](../media/ex3_g_tr_9.png)

1. On the **Incident settings** page, keep incident creation **Enabled**, leave alert grouping **Disabled**, and click **Next: Automated response >**.

   ![](../media/ex3_g_tr_10.png)

1. Click **Next: Review + create >**.  

   ![](../media/ex3_g_tr_11.png)

1. On the Review and Create tab, select the **Save** button to create and save the new Scheduled Analytics rule.

   ![](../media/ex3_g_tr_12.png)

### Task 8: Create a Search job

In this task, you will use a Search job to look for a C2.

> **⚠ Important Usage Guidance:** Microsoft Defender for Office 365 may take some time to load certain results or complete specific labs from the backend. This is expected behavior. If the data does not appear after a couple of refresh attempts, proceed with the next lab and return later to check the results.

1. In Microsoft Defender Portal, on the **Search** page, select **Search (1)** from the left menu, enter **reg.exe (2)** in the search box, and click **Start**.

   ![](../media/ex3_g_tr_14.png)

1. In the **Logs** window, click the ellipsis icon **(1)** at the top right and select **Search job (2)**.

   ![](../media/ex3_g_tr_15.png)

1. On the **Run a search job** window, select **Last 24 Hours (1)**, enter **Newtable (2)** as the name, and click **Run search job (3)**.

   ![](../media/ex3_g_tr_16.png)

1. The query results display in the **Logs** window showing the retrieved event details.

   ![](../media/ex3_g_tr_17.png)

1. Close the *Logs* window by selecting the **X** in the top-right of the window and select **OK** to discard the changes. 

1. On the **Newtable_SRCH** panel, click **Restore** to investigate the retrieved data.

   ![](../media/ex3_g_tr_18.png)
 
1. On the **Restoration** window, review the settings and click **Restore** to begin the process.

   ![](../media/ex3_g_tr_19.png)

1. Review the options available and then select the **Cancel** button.

    >**Note:** If you were running the job, the restore would run for a couple of minutes and your data would be available in a new table.
 
### Task 9: Explore Notebooks

In this task, you will explore using notebooks in Microsoft Sentinel.

1. In the Microsoft Sentinel Workspace, select **Notebooks (1)** under the *Threat management* area.

1. Next, you need to create an AzureML Workspace. Select **Configure Azure Machine Learning (2)** and then select the **Create new Azure ML workspace (3)** button in the command bar.

    ![Picture 1](../media/img-01-010.png) 

1. On the **Azure Machine Learning** page, enter the following details: 

    - **Subscription: (1)** Keep it as default
    - **Resource group:** Select **Create new (2)**. Enter **RG-MachineLearning (3)** for the Name and select **OK (4)**.

        ![Picture 1](../media/img-01-102.png)

1. In the **Workspace details** section do the following:

     - **Name:** **aml-workspace-<inject key="DeploymentID" enableCopy="false" /> (5)**
     - **Region: (6)** Keep it as default
     - The Container registry option can remain as **None**.
     - At the bottom of the page, select **Review + Create (7)**.

        ![Picture 1](../media/img-01-103.png)

1. When you see the **Validation passed** message, select **Create**. 

    ![Picture 1](../media/img-01-104.png)

    >**Note:** It may take a few minutes to deploy the Machine Learning workspace.

1. After the *Your deployment is complete* message appears, return to the **Microsoft Sentinel** portal.

1. Select **Notebooks (1)** again and then select the **Templates (2)** tab from the middle command bar.

    ![Picture 1](../media/img-01-105.png)

1. On the **Templates** page, select **A Getting Started Guide for Microsoft Sentinel ML Notebooks**.

    ![Picture 1](../media/img-01-106.png)

   >**Note:** If you face any issues or do not see any popup after clicking on opening the **A Getting Started Guide for Microsoft Sentinel**. please select the **MyNotebooks** Tab beside the Notebook tab and click on the Notebook( the Azure ML Workspace you created earlier) this might be named as **Untitled** then start performing the lab from the lab guide step **13**.

1. On the right pane, scroll down and select **Create from template** button. Review the default options and then select **Save**.

    ![Picture 1](../media/img-01-107.png)

    ![Picture 1](../media/img-01-108.png)

1. Once the saving is done, select the **Launch notebook** button. This will take you to the Microsoft Azure Machine Learning Studio.

     ![Picture 1](../media/lab09-task04-launch.png)
   
1. Select **X** if an informational window appears in the Microsoft Azure Machine Learning Studio.

<validation step="45086f11-29f0-4daa-ae93-6cd87c02fee4" />

> **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
> - Hit the Validate button for the corresponding task.
> - If you receive a success message, you can proceed to the next task.
> - If not, carefully read the error message and retry the step, following the instructions in the lab guide. 
> - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help you out.

## Summary

In this lab, you set up an Azure Sentinel workspace and integrated Microsoft Defender for Cloud to enhance security operations. You connected data sources, simulated attacks, and used KQL to create custom detections. You also automated incident responses with Logic Apps playbooks, demonstrating a comprehensive approach to threat detection and response. 

## You've successfully completed this lab. Click **Next** to continue with the lab.
