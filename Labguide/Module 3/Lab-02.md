# Lab 04 - Create Detections

## Lab scenario

You are a Security Operations Analyst working at a company that implemented Microsoft Sentinel. You are going to work with Log Analytics KQL queries and from there, you will create custom analytics rules to help discover threats and anomalous behaviors in your environment.

Analytics rules search for specific events or sets of events across your environment, alert you when certain event thresholds or conditions are reached, generate incidents for your SOC to triage and investigate, and respond to threats with automated tracking and remediation processes.

## Lab objectives
In this lab, you will understand the following:

- Task 1: Persistence Attack Detection
- Task 2: Privilege Elevation Attack Detection

### Task 1: Persistence Attack Detection

In this task, you will create a detection for the first attack of the previous exercise.

1. In the Search bar of the Azure portal, type **Microsft Sentinel (1)**, then select **Microsoft Sentinel (2)**.

   ![](../media/l1801.png)
   
1. Select the Microsoft Sentinel Workspace you created earlier.

    ![](../media/l1802.png)

1. Select **Logs** from the **General** section.

   ![](../media/cor_r_g_7.png)

    >**Note:** You might see some popup after clicking on **Logs**. close all Popups by clicking on **X** Icon.

    ![](../media/l1803(N).png)

1. On the **Logs** page, click **Simple mode (1)** and select **KQL mode (2)**.

   ![](../media/corr_t_f_6.png)

1. In the query editor, enter the following KQL statement **(1)** and click **Run (2)** to retrieve the records that start with the specified EventId. Review the results in the table below **(3)**.

    ```KQL
    SecurityEvent 
    | where Activity startswith "4624" 
    ```
   ![](../media/lab1-05-l16.png)

    >**Note:** It may take **5 to 10 minutes** for the SecurityEvent data to become available after onboarding or activity generation. 

    >**Note:** The number of output rows and values may differ depending on your environment and data availability.

1. In the query editor, enter the following KQL statement **(1)** to project entities for investigation and click **Run (2)**. Review the results in the table below **(3)**.

    ```KQL
    SecurityEvent 
    | where Activity startswith "4624" 
    | extend timestamp = TimeGenerated, HostCustomEntity = Computer, AccountCustomEntity = SubjectUserName
    ```

   ![](../media/lab1-05-l17.png)

1. In the **Logs** window, click the ellipsis **(1)** in the command bar, select **New alert rule (2)**, and then select **Create Microsoft Sentinel alert (3)**.
   
   ![](../media/cor_r_g_11.png)

1. In the **Analytics rule wizard** on the **General** tab: 
 
    - Enter **Startup RegKey (1)** in the *Name* field.  
    - Type **Startup RegKey in c:\temp (2)** in the *Description* field.  
    - Select **High (3)** for **Severity**.  
    - Choose **Persistence (4)** for **MITRE ATT&CK**.  
    - Ensure **Status** is set to **Enabled (5)**.  
    - Click **Next: Set rule logic > (6)**.  

        ![](../media/cor_r_g_12.png)

1. On the *Set rule logic* tab, the *Rule query* should be populated already with your KQL query, under **Alert enhancement** expand **Entity mapping** and select **+ Add New Entity**.

    |Entity|Identifier|Data Field|
    |:----|:----|:----|
    |Account|FullName|AccountCustomEntity|
    |Host|Hostname|HostCustomEntity|

   ![](../media/cor_r_g_13.png)

   ![](../media/cor_r_g_14.png)

1. If **Hostname** isn't selected for *Host* Entity, select it from the drop-down list.

1. For *Query scheduling* set the following:

    |Setting|Value|
    |---|---|
    |Run Query every|5 minutes|
    |Lookup data from the last|1 Days|

   ![](../media/cor_r_g_15.png)

    >**Note:** We are purposely generating many incidents for the same data. This enables the Lab to use these alerts.

1. Leave the rest of the options with the defaults. Select **Next: Incident settings>** button.

1. For the *Incident settings* tab, leave the default values and select **Next: Automated response >** button.

1. On the *Automated response* tab, leave everything as default and select  **Next: Review + Create** button.
  
1. On the **Review and create** tab, select the **Save** button to create the new Scheduled Analytics rule.

   ![](../media/cor_r_g_16.png)

### Task 2: Privilege Elevation Attack Detection

In this task, you will create a detection for the second attack of the previous exercise.

1. In the Microsoft Sentinel portal, select **Logs** from the General section in case you navigated away from this page.

1. **Run** the following KQL Statement to identify any entry that refers to administrators:

    ```KQL
    search "administrators" 
    | summarize count() by $table
    ```

    ![](../media/lab1-05-l18.png)

1. The result might show events from different tables, but in our case, we want to investigate the SecurityEvent table. The EventID and Event that we are looking at is "4732 - A member was added to a security-enabled local group". With this, we will identify adding a member to a privileged group. **Run** the following KQL query to confirm:

    ```KQL
    SecurityEvent 
    | where EventID == 4732
    | where TargetAccount == "Builtin\\Administrators"
    ```

   ![](../media/lab1-05-l19.png)

1. Expand the row to see all the columns related to the record. The username of the account added as Administrator does not show. The issue is that instead of storing the username, we have the Security IDentifier (SID). **Run** the following KQL to match the SID to the username that was added to the Administrators group:

    ```KQL
    SecurityEvent 
    | where EventID == 4732
    | where TargetAccount == "Builtin\\Administrators"
    | extend Acct = MemberSid, MachId = SourceComputerId  
    | join kind=leftouter (
        SecurityEvent 
        | summarize count() by TargetSid, SourceComputerId, TargetUserName 
        | project Acct1 = TargetSid, MachId1 = SourceComputerId, UserName1 = TargetUserName) on $left.MachId == $right.MachId1, $left.Acct == $right.Acct1
    ```

    ![](../media/lab1-05-l20.png)

1. Extend the row to show the resulting columns, in the last one, we see the name of the added user under the *UserName1* column we *project* within the KQL query. It is important to help the Security Operations Analyst by providing as much context about the alert as you can. This includes projecting Entities for use in the investigation graph. **Run** the following query:

    ```KQL
    SecurityEvent 
    | where EventID == 4732
    | where TargetAccount == "Builtin\\Administrators"
    | extend Acct = MemberSid, MachId = SourceComputerId  
    | join kind=leftouter (
        SecurityEvent 
        | summarize count() by TargetSid, SourceComputerId, TargetUserName 
        | project Acct1 = TargetSid, MachId1 = SourceComputerId, UserName1 = TargetUserName) on $left.MachId == $right.MachId1, $left.Acct == $right.Acct1
    | extend timestamp = TimeGenerated, HostCustomEntity = Computer, AccountCustomEntity = UserName1
    ```

   ![](../media/lab1-05-l21.png)

1. Now that you have a good detection rule, in the Logs window, in the **Logs** window, click the ellipsis **(1)** in the command bar, select **+ New alert rule (2)** in the command bar and then select **Create Microsoft Sentinel alert (3)**.

    ![](../media/cor_r_g_11.png)

1. You will be navigated to **Analytics rule wizard** page. In the *General* tab provide the values:

    |Setting|Value|
    |---|---|
    |Name|**SecurityEvent Local Administrators User Add**|
    |Description|**User added to Local Administrators group**|
    |Severity|**High**|
    |MITRE ATT&CK|**Privilege Escalation**|
  
1. Select **Next: Set rule logic >** button. 

   ![](../media/6-5.png)

1. On the *Set rule logic* tab, the *Rule query* should be populated already with your KQL query, and add the details for entities under *Alert enhancement - Entity mapping* by selecting **+ Add new entity**.

    |Entity|Identifier|Data Field|
    |:----|:----|:----|
    |Account|FullName|AccountCustomEntity|
    |Host|Hostname|HostCustomEntity|

1. For *Query scheduling* set the following:

    |Setting|Value|
    |---|---|
    |Run Query every| **5 minutes (1)** |
    |Lookup data from the last| **1 Days (2)**|

    >**Note:** We are purposely generating many incidents for the same data. This enables the Lab to use these alerts.

1. Leave the rest of the options with the defaults. Select **Next: Incident settings> (3)** button.

   ![](../media/6-6.png)

1. For the *Incident settings* tab, leave the default values and select **Next: Automated response >** button.

1. On the *Automated response* tab under *Automation rules*, select **+ Add new (1)** and Use the settings in the table to configure the automation rule Select **Apply**.

   |Setting|Value|
   |:----|:----|
   |Automation rule name|**SecurityEvent Local Administrators User Add (2)**|
   |Trigger|**When incident is created (3)**|
   |Actions |**Run playbook (4)**|
   |Playbook |**PostMessageTeams-OnIncident (5)**|

   >**Note:** You have already assigned permissions to the playbook, so it must be available if not click on manage permissions and select it manually and it will be available by now

   ![](../media/lab1-05-l22.png)

1. Select the **Next: Review + create >** button.
  
    ![](../media/lab1-05-l23.png)

1. On the *Review + create* tab, select the **Save** button to create the new Scheduled Analytics rule.

## Summary

In this lab, you created custom analytics rules in Microsoft Sentinel to detect persistence and privilege escalation attacks using KQL queries. Next, you will investigate an incident triggered by these detections. Click **Next** to continue with the lab.

## You've successfully completed this lab. Click **Next** to continue with the lab.

![](../media/page_no_6.png)