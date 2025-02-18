---
title: AppPartConfig element (AppPartConfigDefinition)
manager: soliver
ms.date: 9/16/2015
ms.audience: Developer
ms.topic: reference
ms.prod: sharepoint
ms.localizationpriority: medium
ms.assetid: 95395db7-c2c2-a74a-3e56-7555adc768b7
---

# AppPartConfig element (AppPartConfigDefinition)

**Applies to**: SharePoint Add-ins | SharePoint Foundation 2013 | SharePoint Server 2013

The top level node of an app part configuration file.

> [!NOTE] 
> The string `app` appears as part of or all of some element, attribute, and file names because SharePoint Add-ins were originally called "apps for SharePoint." To ensure backward compatibility, the schemas have not been changed.

## Element information

|   |   |
|---|---|
| **Element type**  | AppPartConfigDefinition |
| **Namespace**  | `http://schemas.microsoft.com/sharepoint/2012/app/partconfiguration` |
| **Schema file**  | apppartconfig.xsd |

## Definition

```XML
    <xs:element name="AppPartConfig" type="AppPartConfigDefinition"></xs:element>
```

## Elements and attributes

If the schema defines specific requirements, such as **sequence**, **minOccurs**, **maxOccurs**, and **choice**, see the definition section.

### Parent elements

None.

### Child elements

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><p>Element</p></th>
<th align="left"><p>Type</p></th>
<th align="left"><p>Description</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="id-element-apppartconfigdefinition-complextypeapppartconfigdefinition.md">Id</a></p></td>
<td align="left"><p><a href="guid-simpletype-apppartconfigdefinition.md">GUID</a></p></td>
<td align="left"><p>The unique identifier of the app part.</p></td>
</tr>
</tbody>
</table>

### Attributes

None.

<br/>

<br/>







