# Template settings

TemplateTo gives you a lot of control over your templates. Allowing you to configure many aspects.

![Settings button](../images/a5e1ca4966568a3363fb78f94b225e2be86c05eb96c779b9bbfc3f2747105c36.png)  

Click the settings button to open the template settings panel.

## General settings
![General template settings](../images/db6839960bc3dff66ae16e61fd97df2c4e5fe67b5297d1994fd9e439524040e3.png)

In General settings you can change the templates name, Enable Html editing and Enable data logging. 

Changing the template name is obvious, however the other two options might be less so, so lets go into more details.

### Enable HTML Editing

If you enable this option then your editor will have another option next to the css option where you can change the HTML of the template. 

![picture 13](../images/3ef60ec07e2a97795e26def9a7ec5be948107cb9595cdcb4804f02977a725255.png)  
!!! warning
    Combining "Advanced" & "Logic" components with direct HTML editing may result in undesirable outcomes.
    
    If you want to work with the HTML directly, you may be better served to work exclusively with the HTML editor.

### Enable Data Logging

When you are integrating with a template for the first time it can be hard to understand why your output isnt what you expect, in our experience this is often due to input data not matching your expected format, so to help debug these types of issues we added this option. When this is enabled you will get additional information. 

![When Enable Data logging is disabled](../images/794654ae1655b6f155c47604496e2508d5d4a16efef0412996af34e557f9be4b.png)  
This is what is logged when the setting is disable, in this case we dont store any of the data sent to use for document generation. The data only lives in memory and is forgotten when the request is complete. 

![When Enable Data Logging is Enabled](../images/dfccab35ad0dc2f3daf5b1ef451ee02526bf6ffb30d52350c35e11fe27e51672.png)  
In this example we have enabled the data logging. As you can see we now store the request and data input. 


!!! note
    Consumption quantity in these examples is 0 as the generations were triggered via the editors preview function. 

!!! warning
    Due to data being saved it is recommended that once you have finished debugging you disable this option.

### Disable PageBreak Calculation

When this setting is off the application calculates where you can expect page breaks to fall when you print your document. 

If you don't want this behaviour then you can disable it for the template. 

This can be useful in some scenarios. As this calculation isn't perfect. 

### Culture name

If you want your template to render in a culture other than `en-US` you can provide the culture code here. When you use date and number formatting the culture name given here will be used.

### Enable Theme

This will apply the theme file marked as default to the template. This will take affect both in the template editor and in the generated template.

[Theme documentation](../template-themes)

## PDF Settings

Numerous settings you can apply to your PDF. 

### Paper size

Change the size of your page to suit your needs.

![Paper size options](../images/45f42f63e96e8dde37e2d79fa1272e579cad886472a72a21ee74483d6d6f3d38.png)  

If you want a custom size, select "custom" and you will then be given two fields to enter the custom size you want. 

!!! note
    The custom size should be provided in mm (millimeters).

### Orientation

As you would expect, this changes the documents orientation, if enabled the document will be landscape.

### Print background

Enable to print background graphics. This is enabled by default.

### Display Header / Footer

Enable if you want to have a header and footer on your documents pages. 
![Enable Headers & footers](../images/6a7ab088c0dcfa1d77790f8f013f2357ebd966a9d70baa807f7b4aa9353e63a1.png)  

You can select a content block for each the header or the footer. 

![Select header / footer content blocks](../images/3260c022f378f82841120f3b08773ee79c2d0eefeec4923e47403978eb3b217e.png)  

### PDF Password protection

![Enabling Password protection](../images/5e19c35586294157ce352d07919a607f644d81596004fd5770d02ad15472b0c9.png)  

Once you enable password protection for your pdf, you will get the following options:
![Password protection options](../images/8e3c20b1eeaa4e9839e17aaf958cb221f0bb6d9decc57302021ffe46096adc67.png)  

### Other options


| Field                  | Description                                                                                                                  |
| ---------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| Password               | The password field accepts plain text as well as **variables** from your template, this includes **variables created with filters**. |
| Permit Print           | Permits printing of the document                                                                                             |
| Permit modify document | Permits modifying the contents of the document by operations other than those controlled by PermitAnnotations                |
| Permit extract content | Permits copying or extracting text and graphics from the document.                                                           |
| Permit annotations     | Permits adding or modifying text annotations                                                                                 |
|                        |                                                                                                                              |

## Margins  

![Margins settings](../images/9b12878689f112de6c9b3f90b6c44fba1c360b893ec8eb6e4288fa2af23186e2.png)  

Set the required margins for you documents. 

!!! note
    Your headers and footers get rendered in the margins, so ensure to take that into account when setting this up.

## Cover page

![Cover page options](../images/f29bb89daab848ad0bd988bf2eb4c8edde1a83e6725df445c6102000943dd92f.png)  

The cover page options are a repeat of some of the options that you can set on the rest of the template. However these settings are only applied to the first page of you document. 
