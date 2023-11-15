# Elements

TemplateTo comes with a number of elements designed to speed up the creation of templates.

Elements fall into 4 categories. 

| Item     | Description                                                                                                                      |
| -------- | -------------------------------------------------------------------------------------------------------------------------------- |
| Layout   | These elements allow you to more easily compose complex document layouts                                                         |
| Basic    | These elements include Text, Image and header elements, these are easy to work with and usually content focused                  |
| Advanced | Use advanced elements if you are comfortable with custom code                                                                    |
| Logic    | Logic components allow you to show or hide sections of your template based on data based to your template at point of generation |
|          |                                                                                                                                  |

## Layout elements

![Layout elements](../images/ae40d3afb2c0245820db99c65e56c539ac18e0b324044ec3afa28ad13a729d61.png)  

| Item            | Description                                                                        |
| --------------- | ---------------------------------------------------------------------------------- |
| 1 Column        | This provides a single column layout                                               |
| 2 Column, 50/50 | Two columns initial set to 50% width each. Adjust the width with the anchor points |
| 3 Column        | A Three column layout with each column set to 33% width. Widths also adjustable    |
| 2 Column, 20/80 | Another two column layout, this time with an 20/80 split. Widths also adjustable   |
|                 |                                                                                    |

!!! tip
    You can nest layouts within one another.

### Video: Working with layouts
<iframe width="560" height="315" src="https://www.youtube.com/embed/mLaeH9AtGHk?si=sc8KRI_GDCUJGfU8" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

## Basic elements

![Basic elements](../images/bbf0ba121a3005bc1b2beb8fa1205296418606f54be4a3f9388424c05ee92a11.png)  

| Item                                                       | Description                                            |
| ---------------------------------------------------------- | ------------------------------------------------------ |
| [Text](/getting-started/elements/#text)                    | Simple WYSIWYG text element                            |
| [Image](/getting-started/elements/#image)                  | Add images to your template                            |
| [Content Block](/getting-started/elements/#content-blocks) | Content blocks are reusable content. See more details. |
| [Page Break](/getting-started/elements/#page-break)        | Add a Page Break to your template                      |
| [Header](/getting-started/elements/#header)                | Simply adds a heading section                          |
| [Table](/getting-started/elements/#table)                  | Add a table to the editor                              |
|                                                            |                                                        |

### Text

This element puts a WYSIWYG editor onto your template, you can style the text within the element using the tools in the [Editor window](/getting-started/editor-overview/#editor-window)

### Image

Add images to your template with this element. Either drag images into the upload area of navigate to your image.

![Adding an image](../images/AddImage.gif)

### Content Blocks

Content Blocks are reusable content that can be used in multiple templates.

!!! info
    These are like mini-templates. You can use a lot of the same components we are discussing here within the content block

    Go to the [Content Block area of the admin](https://app.templateto.com/templates/blocks) to create Content Blocks.

### Page Break

As you would expect in word or any other text editing software, this breaks the content that comes after onto a new page.

![Example page with a Page Break](../images/7217146102004a8a8a2287bdc448ee3b07a6f595a206779169d9f4a5facf0fd8.png)  

1. A Page Break element fills the remaining space on the page. As you change content above the size of the Page Break element will adjust so you can see how much space is left.
2. This text will be on the next page (page 2 in this example).

### Header

The header element adds a heading to the template, you can use the custom header menu to change the heading.

![Example Header with menu](../images/91520863689cbb7119822e09466084ad9b71faa8e88ebb4e1ef590348646c975.png)  

1. Custom Header editor bar.

### Table

The table element, as you might expect, allows you to add a table to your template. 

#### Table settings

Table elements have settings you can change from the properties panel:
![Table settings panel](../images/e9152755dddcb7487da56636928011f860fcea6057125e950d65b6dcbce1b828.png)  

#### Table cell options

![Working with tables](../images/WorkingWithTables.gif) 

The video shows how to work with some of the table cell option to add column and rows as well as merging and unmerging cells. 

##### Cell column menu

![cell column options](../images/079c9789c165565465fe552b37fd18b2ed3057a7cc336a07f08b2464205b7e8d.png)  

##### Cell row menu

![cell row options](../images/fb36c0a92dce474355a0b2e3977fcb84c12cfbdd2d4585e5b17138f10964b4a1.png)  

#### Merging and un-merging cells

To merge cells use the appropriate option from the cell menus above. 

To un-merge cells, click on the a merged cell, there is an additional option on the cell toolbar now, it looks like four blocks *(see below)*. 

![The un-merge toolbar button](../images/d89d39440670c2943d7958f8a10a988d6ad429fb6553e43ef63335b2f5c93813.png)  


## Advanced elements

![Advanced elements selection](../images/cedabe6f25059173a33f9e16aa645d194f9aa78c4344002e796bf17de5ccd8be.png)  

### HTML

To add some custom HTML to your template simply click or drag this element into the editor. Double click to open, you can add your html within the editor window. 

!!! warning
    If script tags are entered the custom html wont be saved. 
    We currently don't support running script tags within a template.

## Logic

![Logic elements](../images/711a7c76ddedb7d82f79049d03d187a09dd4ecfe82e5483f4c44365ad616cdcf.png)  

### Condition

!!! note
    It might be helpful to have data added to your template when working with the Condition element. [Learn more here](/getting-started/working-with-variables)

The condition element allows you to show a section only when specified conditions are met. 

The following conditions are available:

![Logical conditions available](../images/011616d481663bbb5e6507dc24ecd7b4d6080559a2ba68db61b6b26417bd3c57.png)  

The following clip shows how to work with the condition element. 

![Example working with Condition element](../images/WorkingWithConditions_Gif.gif)

!!! note
    You can put any content you want within a condition element. Which makes them very powerful.