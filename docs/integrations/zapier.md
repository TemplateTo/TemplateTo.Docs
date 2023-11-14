# Zapier

This guide expects you to have some experience with Zapier. 

## A trigger

In Zapier you will need to setup a trigger, this is an action that wil start your Zap. In our example we will use a google sheet.

## Configure TemplateTo

![TemplateTo Selection](../images/9fc074602359cde7c24bd27f689b97bd7470432d0cb071b636048b1f0a67e0a9.png)  

Type "TemplateTo" in the search and select the option marked with latest.

### App & event

![App & event configuration screen](../images/a1f26ad2243c59d6b40df01ea623931edb326e0e2912071d942c0d697374e035.png)  

In the event select "Create File", click Continue.

### Account

Create a connection, you will then get a pop-up, in the pop-up set your TemplateTo API key. 

!!! tip
    You can manage your api keys in the [admin here](https://app.templateto.com/generate/api-keys)

![Auth configuration screen](../images/6a38f80849e861c22abceb0e9009420262b94704b9de77db5dc4c9676ba7257c.png)  

Click continue

### Action

Here we configure the template to be used and the data to be passed.

![Action settings page](../images/19d178306edf2fda9aa9a6ec2f6fdec7388651c78e31068c95a36ea0570330d2.png)  

| Item                         | Description                                                                                                                                                                                                                                         |
| ---------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1. Template                  | Select your template from the dropdown list                                                                                                                                                                                                         |
| 2. Enable advanced input     | If you want to structure your own JSON then set to true, if you want to provide name value pairs, leave as false                                                                                                                                    |
| 3. Template data             | Either name value pairs or custom JSON depending on the previous option. If name value pairs you can select based on data available in your trigger. ![Insert data](../images/74303ce5bf4f56cca2a9115a5b6a38319b971bd57bb1bc60ffc5d3abc024d3b3.png) |
| 4. Preserve Line Breaks      | If true will keep line breaks, also known as paragraph breaks from the input.                                                                                                                                                                       |
| 5. Document type to generate | Either PDF or Txt. |

Once all options are complete, click continue

### Test

Test that your integration is working correctly. 

Once the test has run you should be provided a link to your file like this

![Test result](../images/73c459dc969a39c4182efc360da8dc1a77d7e652cd04c37afe81aa2d285e3cd7.png)  

You can use this file in future steps, perhaps you want to send the file via an email or save the file to box or google drive, your options are vast.