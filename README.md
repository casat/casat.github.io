# CASAT Wiki
wiki.casat.org/casat.github.io

The CASAT wiki is a 'wiki-style' page that was created to house our IT documentation and any other relevant processes. The webiste itself is dynamically generated from the content within this Github repository using the Hugo framework and hosted 'serverless' through Github Pages. If you need to edit or otherwise modify the content of this wiki, it can be edited by any Github account that is part of our CASAT organization. *Any changes should be applied to the 'source' branch*, **do not edit the 'main' branch directly** as this is dynamically created content that is generated after detecting changes to the 'source' branch.

## Editing the Wiki

Since all the relevant information is stored in GitHub there are many options for editing the content. You can edit the files [directly from Github](https://docs.github.com/en/repositories/working-with-files/managing-files/editing-files) which is ok for small edits but you may want a better way when doing more substantial updates. The preferred way to edit the files would be by using a third party text editor such as [Atom](https://atom.io/), or [Visual Studio](https://visualstudio.microsoft.com/) which allow you to checkout a copy of the repository, edit it locally, and then push the updates to the repository. *Make sure you are checking our the correct 'source' branch when using these tools to do your edits.* Any edits to the content in Github will trigger an action that checks the code for errors, if there are errors they must be corrected before the website will rebuild. You can check the status of this by clicking on the 'commits' tag right under the 'Code' button on this page.

### Editing Hugo Files

[Hugo](https://gohugo.io/) is a framework that uses Markdown files to generate html files, these frameworks are referred to as 'static site generators'. The important things to remember when updating the content is to match the existing file structure as much as possible as Hugo builds the webpages/folders from the folder structure, and to pay attention to the 'Front Matter' on the pages. Front Matter is what Hugo calls the 'Header' type section of each Markdown document, the area delinated with the ---. And again only make changes to the 'source' branch in Github, the 'main' branch will be automatically generated. Otherwise you can find more information about [Markdown syntax here](https://www.markdownguide.org/basic-syntax/) and more information about [Hugo here.](https://gohugo.io/documentation/)

### Editing the Theme

This website was built using the Hugo geek-docs theme. The theme files exist within the repository so they can be edited freely like the rest of the content. Be aware when updating the theme that you should download and install the 'pre-release bundle build' which is necessary for GitHub pages.

## Wiki Hosting

This website is hosted 'serverless' through Github Pages. After you do updates to the content of the repository the website will automatically do a code check and then regenerate the website once that check clears. If the code check fails then the website will not regenerate until the errors are cleared. The URL wiki.casat.org is applied via the Pages tab in the settings of this repository and within our DNS entries in DigitalOcean.
