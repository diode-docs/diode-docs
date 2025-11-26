---
_schema: default
title: Markdown Editing Basics
nav_title: Markdown Editing
nav_section: Features
weight: 205
draft: false
---
Diode Collab has a <a href="https://www.markdownguide.org/getting-started/" target="_blank" rel="noopener"><strong>markdown</strong></a> viewer that automatically renders markdown file formatting for the file [**set as the Home Page**](https://app.docs.diode.io/docs/using/create-a-home-page-for-your-zone/) and for markdown files in the File Details and Web Link file previews.

For a complete guide to markdown editing, see markdown.org's <a href="https://www.markdownguide.org/cheat-sheet/" target="_blank" rel="noopener"><strong>cheat sheet</strong></a> and <a href="https://www.markdownguide.org/extended-syntax" target="_blank" rel="noopener"><strong>other resources</strong></a> there.

Here are some Diode specific tips:

#### **Display an Image**

* Description: Display an image
* Syntax: `![Name of Image](/Path/to/image.png)`
* Example: `![My Logo](/Brand/mylogo.png)`
* Notes
  * Only .png and .jpg files are supported
  * Zone file/folder paths must start with "/"
  * Paths can be external / web URLs

&nbsp;

#### **Display an Image with Link**

* Description: Display an image that, when clicked, will redirect to another page in the app or external website
* Syntax: `[![Name of Image](/Path/to/image.png)](https://linkurl)`
* Example: `[![My Logo](/Brand/mylogo.png)](https://diode.io)`
* Notes
  * Only .png and .jpg files are supported
  * Zone file/folder paths must start with "/"
  * Paths can be external / web URLs

#### **Link to the Root Directory**

* Description: Insert a hyperlink that, when clicked, redirects the user to the base level (root) file folder view
* Syntax: `[Link Text](./)]`
* Example: `[All the Files](./)`
* Notes
  * The root directory is a unique location that requires the "." at the beginning of the path

#### **Link to a File or Folder**

* Description: Insert a hyperlink that, when clicked, redirects the user to a specific file or folder
* Syntax: `[Link Text](/Path/to/folder/file.ext)`
* Example: `[2022 Financial Projections](/Finance/Forecast/2022/2022_financial_model-v3.xlsx)`
* Notes
  * To link to a folder, just end the path with the folder name

    &nbsp;

---

&nbsp;