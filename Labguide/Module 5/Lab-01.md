# Lab 07 - Mitigate threats using Microsoft 365 Defender [READ ONLY]

> **Note:** `This lab is provided in a read-only format for the incident investigation section, as it may take approximately 24–48 hours for incidents to appear in the Microsoft Defender portal after alert generation. If incidents are not available within the lab timeframe, you can still follow the documented steps to understand the incident management workflow and portal experience. Alternatively, navigate to the Incidents & Alerts > Alerts page to verify whether any related alerts have been generated during the lab session.`


## Lab scenario

You're a Security Operations Analyst working at a company that implemented Microsoft Defender XDR solutions. You need to see the alerts in an incident to see the incident's full impact do a root cause investigation and mitigate these alerts using M365 Defender tools.

## Lab objectives

In this lab, you will perform the following in the M365 Defender portal:

- Task 1: Manage Incidents
- Task 2: Investigate Alerts

### Task 1: Manage Incidents

In this task, you will manage the incidents in the M365 Defender portal.

1. Navigate back to Microsoft 365 Defender portal in your Microsoft Edge browser.

1. From the sidebar menu, under **Incidents and Alerts**, select **Incidents**. Click on the incident **Created**.

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

### Task 2: Investigate Alerts

In this task, you will investigate and mitigate the alerts through recommendations by Microsoft Defender.

1. In the Microsoft Defender portal, navigate to the **Alerts** tab from the sidebar menu.

    ![Lab overview.](../media/lab10-task2-alerts.png)

    >**Note:** It may take 24-48 hours for alerts to appear in the Defender portal. If they are not generated within the lab timeframe, you can go through the steps.

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
