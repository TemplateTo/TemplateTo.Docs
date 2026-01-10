# Getting Started in 5 Minutes

This quick tutorial will get you up and running with TemplateTo in just 5 minutes. By the end, you'll have created your first PDF template and generated a document.

## Step 1: Create Your Account (1 minute)

1. Go to [app.templateto.com/auth/signup](https://app.templateto.com/auth/signup)
2. Enter your email and create a password
3. Verify your email address
4. Select or create your first workspace

!!! tip
    You can create multiple workspaces to organize different projects or clients.

## Step 2: Create Your First Template (2 minutes)

1. Click **"Create Template"** from your dashboard
2. Give your template a name (e.g., "My First Invoice")
3. You'll see the Template Editor open with a blank canvas

### Add Your First Element

1. From the **Elements panel** on the left, find the **Text** element
2. Either:
   - Double-click the Text element, or
   - Drag and drop it onto the canvas
3. Click on the text element to edit it
4. Type your content (e.g., "Invoice #001")

!!! info
    The editor auto-saves your work, but you can also click the Save button in the top bar.

## Step 3: Add Dynamic Content (1 minute)

Let's make your template dynamic by adding variables:

1. Click the **Data** tab in the left panel
2. Add a new variable:
   - Name: `customerName`
   - Type: Text
   - Default Value: "John Doe"
3. In your text element, type: `Hello {{customerName}}!`
4. Click **Preview** to see your variable in action

## Step 4: Generate Your First PDF (1 minute)

1. Click the **Generate** button in the top bar
2. You'll see a form with your variable fields
3. Enter a value for `customerName` or use the default
4. Click **Generate PDF**
5. Your PDF will download automatically!

## What's Next?

Congratulations! You've created your first dynamic PDF template. Here are some next steps:

- **[Explore more elements](elements.md)**: Add images, tables, QR codes, and more
- **[Learn about variables](working-with-variables.md)**: Create complex data structures
- **[Style your template](template-themes.md)**: Customize colors, fonts, and layouts
- **[Automate generation](../integrations/index.md)**: Use our API or integrations

### Quick Video Tutorial

Want to see it in action? Watch this 5-minute video:

<iframe width="560" height="315" src="https://www.youtube.com/embed/Aquy1eLA7cM?si=fkynQyw6r7OuNtzY" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>