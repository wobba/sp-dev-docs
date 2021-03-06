---
title: Customize default site designs in SharePoint
description: Customize the default site designs in either the SharePoint Team site or Communication site template.
ms.date: 04/20/2018
---

# Customize a default site design

SharePoint contains several site designs already available in the SharePoint Online site templates. These are the default site designs. You can modify them by using PowerShell or the REST APIs to control the entire site provisioning experience. For example, you can ensure that your company theme is applied to every site that gets created, or you can make sure a logging mechanism always runs regardless of which site design is chosen.

## Apply a site design to the default site designs

To customize the default site designs, apply a new one with the PowerShell **Add-SPOSiteDesign** cmdlet or the **CreateSiteDesign** REST API. Specify the **IsDefault** switch to apply the site design as the default. 

The WebTemplate ID for a group-connected Team site is 64; for a Communication site it is 68.

The following example shows how to use the **IsDefault** switch to apply the Contoso company theme to the default site designs. The site script referenced by ID contains the JSON script to apply the correct theme.

```powershell
C:\> Add-SPOSiteDesign `
  -Title "Contoso company theme" `
  -WebTemplate "68" `
  -SiteScripts "89516c6d-9f4d-4a57-ae79-36b0c95a817b" `
  -Description "Applies standard company theme to site" `
  -IsDefault
```

<br/>

```javascript
RestRequest("/_api/Microsoft.Sharepoint.Utilities.WebTemplateExtensions.SiteScriptUtility.CreateSiteDesign", {info:{Title:"Contoso company theme", Description:"Applies standard company theme to site", SiteScriptIds:["89516c6d-9f4d-4a57-ae79-36b0c95a817b"],  WebTemplate:"68", IsDefault: true}});
```

### Which default site designs are updated?

In the previous example, the **WebTemplate** value of `"68"` refers to the SharePoint Online Communication site template. That template contains the following default site designs:

- Topic
- Showcase
- Blank

When you apply a new site design, it updates all three default site designs at the same time.

The SharePoint Online Team site template contains only one default site design named **Team**. In this case, when you apply a default site design, only the **Team** site design is updated.

## Restore the default site designs

To restore a site design to the defaults, remove the site design that you applied. In the previous example, if the site design created had the ID `db752673-18fd-44db-865a-aa3e0b28698e`, you would remove it as shown in the following example.

```powershell
C:\> Remove-SPOSiteDesign db752673-18fd-44db-865a-aa3e0b28698e
```

<br/>

```javascript
RestRequest("/_api/Microsoft.Sharepoint.Utilities.WebTemplateExtensions.SiteScriptUtility.DeleteSiteDesign", {id:"db752673-18fd-44db-865a-aa3e0b28698e"});
```

> [!NOTE]
> If you're not sure which site design is the default, run the **Get-SPOSiteDesign** cmdlet. It will list all site designs, and indicates which ones are defaults.

## See also

- [SharePoint site design and site script overview](site-design-overview.md)