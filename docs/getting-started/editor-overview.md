# Editor overview

The TemplateTo editor is powerful, but also simple to use. To achieve this some of the more advanced features are slightly obscured, however if you want to use them they are still easily available. 

In this overview we will look at a lot of the features on offer within TemplateTo.

## The full editor window
![picture 22](../images/9dc67a70ed8c955b5511af3021a4b18ed19d8d6b7f9f064aff365859a70f49cf.png)  

## Top bar

The top bar has a number of buttons within it, lets go over what each one does. 
![picture 0](../images/ebc3de589cb7fa82cfd33fd8e718c0d76ecaa00b7f4c26934f4e68890fd9afaf.png)  

From left to right:

| Item                     |                                                                                              | Description                                                                                                   |
| ------------------------ | -------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| TemplateTo Logo          | ![TemplateTo Logo](../images/096b1ac00e41d54fa523191ecf74b201b5997edd6da997dd114bbf2a53dcf91e.png){: .off-glb } |                                                                                                               |
| Template name            | ![Template name image](../images/3a04e6fe1d3a25086c5dcebfabf52dca7dd2e3fe55ee5ef1acc884928d72e9c2.png){ .off-glb } | The name of the template currently being edited                                                               |
| Generation type selector | ![Generation type selector](../images/3126bb64d5df09a64dd426b5c8960c3f66173af4470a28ca78e45af02ed93755.png){ .off-glb } | Used by the Preview button, it indicates if the preview should be in txt or pdf.                              |
| Preview button           | ![Preview template button](../images/98ee44db888a141e68e7c9f2de99702c5db267766579cca6309f31faf089e8e2.png){ .off-glb } | Opens a preview of the current template, opens in another tab. Will automatically update when you save changes |
| Template Settings button | ![Open template settings button](../images/6410ddc5865d50af3408863e1e8dba158bfdd9512e25f8adc1b69edde0b2e99a.png){ .off-glb } | Opens a window with several settings, things like Margins, headers/footers, template name.                    |
| Save button              | ![Save template button](../images/aad6f694f07122f85ac18cfa5f089d187fdbff6653b273ac7c8e434b9ada35db.png){ .off-glb } | Saves changes to the template                                                                                 |
| Generate button          | ![Generate button link](../images/e7424cfeda9168acbeba4205efad672b9961b2e223ac235348fba73d69ca357d.png){ .off-glb } | Takes you to the generation page, this allows you to create a generated template from within the UI.          |
|                          |                                                                                              |                                                                                                               |

## Editor function menu

The editor function menu gives you access to Properties, Elements, Data and CSS as well as allowing you to undo/redo actions, import and export your templates.
![Editor function menu](../images/3277ec1847ed2811dd59560726e8d8aef847d1051cb197c12bcc69f7902274fc.png)

| Item          |                                                                                               | Description                                                                                                                                                            |
| ------------- | --------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Undo          | ![Undo button](../images/7b214dd77926ffd3c719d9b42db3dcc5bb9f0023b9ff8d40aca31af124a68ce4.png){ .off-glb }  | Will undo the last action made within the editor window                                                                                                                |
| Redo          | ![Redo button](../images/afb8a56b201557f767051464b94de795a23a9f11eb39cfcb608c884e186b8270.png){ .off-glb } | Will redo an undone action                                                                                                                                             |
| Outline       | ![Outline toggle](../images/6db522122477ca3e5c04fad0b9d5498f90bb08fff5e8b6d1f507975912b186b5.png){ .off-glb } | Toggles the grey dotted lines on an off within the editor window                                                                                                       |
| Import/Export | ![import/export button](../images/2a0427210ba2f13cbf8b5f6351eaf78fe0d2573b44a044f6a54466604edbad12.png){ .off-glb } | Opens a dialogue so you can import a template or export the current template                                                                                           |
| Elements      | ![Elements button](../images/d19cd42f9f1cccb31644bc7b54cd7ea87186b6e578cda2bc1201fcd4926f2d1c.png){ .off-glb } | Will Show the available components in the right hand panel of the editor                                                                                               |
| Properties    | ![Properties button](../images/2bf2f5b68a8426b5e1a3ca2af3eaa57bc5d7d658faafbb6507f0c8ba3fe53ddb.png){ .off-glb } | Opens the Properties and settings panel for the selected element, allows you to add element styling                                                                    |
| Data          | ![Data button](../images/1368a78fe9aa86e25dee2e8da3b3279b794f20eda4b146f3fd1526047e7cd192.png){ .off-glb } | Opens the data panel, this allows you to add variables to the editor, these can be added to your template and teh value can be used to test your templates output.     |
| CSS           | ![Css button](../images/692cbb63f0d6e7beffa4f5ba4e48011a234493463c39ffb46e47dfcbf89e6b9d.png){ .off-glb } | Opens the CSS panel so yu can make changes to the template. This is an advanced feature, most user wont need to change css directly, the properties panel can be used. |
|               |                                                                                               |                                                                                                                                                                        |

## Editor window

This is an example of an editor window with a few Elements added.

![Example editor window](../images/559c799cd24a29ed8a888981c5030f37448778e7c47a05163afc0f5cb904451d.png)  

| Item | Description                                                                                                                                                                                                                                                                                     |
| ---- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1    | This is the text editor bar, this becomes active when you are working with text or heading elements. You can select text within the element and style is with these options. If you want to apply a style to the whole element you might be better off using the properties window to do that. |
| 2    | The blue border indicates which element you currently have selected within the editor.                                                                                                                                                                                                          |
| 3    | This is the element toolbar, each element has at least a remove and move option, some elements have additional options as well.                                                                                                                                                                 |
| 4    | This dotted line represents the outline, it is active by default but can be turned off using the Outlines toggle in the Editor function bar.                                                                                                                                                    |
| 5    | This line shows an estimate of the end of a pages content. Content beyond this line will appear on a new page, you can use the badge (where the text is displayed) to move the line to adjust the end of a page based on preview. This is help give a visual indication, if you need to define the end of a page use the Page Break element. |


## Template settings window

To open the template settings click on the __Template Settings__ button. 

![Opening Template Settings Window](../images/TemplateSettingsOpening.gif){ .off-glb } 

Within the window there are a few options available to you. 

![Annotated image of the settings window](../images/b562d6b3ed6a230a18410a255827333336abbeafee62e8d9a36f909e48f3c3fc.png)  

| Item | Description                                                                                                                                                                                                                                                           |
| ---- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1    | Edit the template name.                                                                                                                                                                                                                                              |
| 2    | Enable HTML editing, this gives lots of flexibility, but it does come with risks, so proceed at your own risk.                                                                                                                                                        |
| 3    | Enabling data logging will make TemplateTo persist lots of information about template generations, you can see exactly what data is being passed in and you can see data about the request, this is very useful when integrating, however it should be off once live. |
| 4    | Select the page size you desire, this will change the editor window to match your selection.                                                                                                                                                                          |
| 5    | You can add a Headers and Footers to your template, we achieve this by linking contentblocks with your template. when you turn this option on you can provide the contentblocks you want to use.                                                                      |
| 6.   | This allows you to configure the margins for your template.                                                                                                                                                                                                           |
|      |                                                                                                                                                                                                                                                                       |
