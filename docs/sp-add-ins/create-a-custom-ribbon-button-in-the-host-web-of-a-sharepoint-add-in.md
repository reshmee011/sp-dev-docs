---
title: Create a custom ribbon button in the host web of a SharePoint Add-in
description: Prepare the host web, add a ribbon custom action, inspect the add-in web feature, and run and test the add-in.
ms.date: 12/04/2017
ms.prod: sharepoint
ms.localizationpriority: medium
---


# Create a custom ribbon button in the host web of a SharePoint Add-in

This is the ninth in a series of articles about the basics of developing SharePoint-hosted SharePoint Add-ins. You should first be familiar with [SharePoint Add-ins](sharepoint-add-ins.md) and the previous articles in this series, which you can find at [Get started creating SharePoint-hosted SharePoint Add-ins | Next steps](get-started-creating-sharepoint-hosted-sharepoint-add-ins.md#next-steps).

> [!NOTE]
> If you have been working through this series about SharePoint-hosted add-ins, you have a Visual Studio solution that you can use to continue with this topic. You can also download the repository at [SharePoint_SP-hosted_Add-Ins_Tutorials](https://github.com/OfficeDev/SharePoint_SP-hosted_Add-Ins_Tutorials) and open the BeforeRibbon.sln file.

All SharePoint Add-ins can be run from the **Site Contents** page of the host web by selecting the add-in's tile. The functionality of a SharePoint Add-in can also be exposed on the host web through custom actions, which are custom ribbon buttons or custom menu items. In this article you add a button to the ribbon on a host web.

## Prepare the host web

To add the button to the ribbon of a calendar on the host web, take the following steps in the UI of your SharePoint developer site.

1. From the home page of the site, select **Site Contents** > **add and add-in** > **Calendar**.

2. In the **Adding Calendar** dialog, for the **Name**, enter **Employee Orientation Schedule**, and then select **Create**.

3. When the calendar opens, put the cursor on any date until the **Add** link appears on the date, and then select **Add**.

4. In the **Employee Orientation Schedule - New Item** dialog, for the **Title**, enter **Orient Cassi Hicks**. Leave the other fields at their defaults, and then select **Save**.

   The calendar should look similar to the following:

   *Figure 1. Custom calendar*

   ![A calendar named Employee Orientation Schedule with an item on June 1 that says "Orient Cassie Hicks"](../images/d2066862-41c1-424d-9bfb-b6c5342bcf2c.PNG)

> [!IMPORTANT]
> The next procedure requires that the calendar be visible in the UI of Visual Studio, but it won't be if Visual Studio was open when you created the calendar. Before you continue, close Visual Studio, and sign out of any browser windows and PowerShell consoles where you are signed in to your developer site.

## Add a ribbon custom action

1. In **Solution Explorer**, right-click the **EmployeeOrientation** project, and select **Add** > **New Item** > **Office/SharePoint** > **Ribbon Custom Action**. Name it **RunOrientationAdd-in**, and then select **Add**.

2. The Create Custom Action for Ribbon Wizard asks you a series of questions. Give the answers from the following table, and then select **Finish**.

    |**Property question**|**Answer**|
    |:-----|:-----|
    |Where do you want to expose the custom action?|Select **Host Web**.|
    |Where is the custom action scoped to?|Select **List Instance** (*not* List Template).|
    |Which particular item is the custom action scoped to?|Select **Employee Orientation Schedule**.|
    |Where is the control located?|Do not use the drop-down selections. Instead, enter `Ribbon.Calendar.Events.Actions.Controls._children`. (The third part, **Events**, identifies the tab of the ribbon, and the fourth part, **Actions**, identifies the button group.)|
    |What is the text on the menu item?|Enter **Employee Orientation**.|
    |Where does the custom action navigate to?|Do not use the drop-down selections. Instead, enter  `~appWebUrl/Lists/NewEmployeesInSeattle`. This is the list view page for the list, which is on the add-in web, so the ribbon button on the host web opens a page on the add-in web.|


## Inspect the add-in web feature

In **Solution Explorer**, expand the **Features** folder, and select the **NewEmployeeOrientationComponents** feature. The Feature designer opens.

Notice that the custom action that you created, **RunOrientationAdd-in**, is listed in **Items in the solution**, but not in **Items in the feature**. This is because the feature is deployed to the add-in web, but your custom action is deployed to the host web.

When you package the add-in in Visual Studio for deployment to production, or when you select F5 in Visual Studio, the Office Developer Tools for Visual Studio creates a special host web feature, adds the custom action to it, and deploys it to the host web. You should never edit the host web feature; that is why it is not created until packaging time.

*Figure 2. Feature designer*

![The feature designer with columns labeled "Items in the solution" and "Items in the feature". The "Run Orientation Add-in" is in the first, not the second.](../images/49ea0bf0-2cfa-4070-aa65-24b4a9c5e874.PNG)

## Run and test the add-in

1. Use the F5 key to deploy and run your add-in. Visual Studio makes a temporary installation of the add-in on your test SharePoint site and immediately runs the add-in.

2. The default page of the SharePoint Add-in opens. Go to the home page of your developer site (which is the host web). There is a breadcrumb link to it in the upper-left corner of the page.

3. On the host web's home page, select **Site Contents**, and on the **Site Contents** page, select the **Employee Orientation Schedule** calendar (not the **Employee Orientation** add-in).

4. When the calendar opens, select the event **Orient Cassie Hicks**. If the **Events** tab on the ribbon doesn't open automatically, open it. It should look similar to the following:

   *Figure 3. Events ribbon tab with custom button*

   ![The Events ribbon with a custom button named "Employee Orientation"](../images/916ecbba-11ff-45b6-a8e9-ba717ae6fe0b.png)

5. In the **Actions** group on the ribbon, select **Employee Orientation**. The list view page for **New Employees in Seattle** opens.

6. To end the debugging session, close the browser window or stop debugging in Visual Studio. Each time that you select F5, Visual Studio will retract the previous version of the add-in and install the latest one.

7. You will work with this add-in and Visual Studio solution in other articles, and it's a good practice to retract the add-in one last time when you are done working with it for a while. Right-click the project in **Solution Explorer** and select **Retract**.

## Next steps
<a name="Nextsteps"> </a>

In the next article in this series, you'll add JavaScript to the SharePoint Add-in and access SharePoint data with SharePoint's JavaScript object model: [Use the SharePoint JavaScript APIs to work with SharePoint data](use-the-sharepoint-javascript-apis-to-work-with-sharepoint-data.md).




