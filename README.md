# CASAT Wiki
wiki.casat.org/casat.github.io

The CASAT wiki is a 'wiki-style' page that was created to house our IT documentation and any other relevant processes. The website itself is dynamically generated from the content within this GitHub repository using the Hugo framework and hosted 'server-less' through GitHub Pages. If you need to edit or otherwise modify the content of this wiki, it can be edited by any GitHub account that is part of our CASAT organization. *Any changes should be applied to the 'source' branch*, **do not edit the 'main' branch directly** as this is dynamically created content that is generated after detecting changes to the 'source' branch.

## Editing the Wiki

Since all the relevant information is stored in GitHub there are many options for editing the content. You can edit the files [directly from GitHub](https://docs.github.com/en/repositories/working-with-files/managing-files/editing-files) which is ok for small edits but you may want a better way when doing more substantial updates. The preferred way to edit the files would be by using a third party text editor such as [Atom](https://atom.io/), or [Visual Studio](https://visualstudio.microsoft.com/) which allow you to checkout a copy of the repository, edit it locally, and then push the updates to the repository. *Make sure you are checking our the correct 'source' branch when using these tools to do your edits.* Any edits to the content in GitHub will trigger an action that checks the code for errors, if there are errors they must be corrected before the website will rebuild. You can check the status of this by clicking on the 'commits' tag right under the 'Code' button on this page.

The actual content of the website is in the 'content' folder. The structure of the wiki will follow the structure of the folders within the 'content' folder, and then the files within there make up the individual pages. When adding a new page to the wiki, the file should end with the Markdown extension (.md) and the content within the file needs to begin with a header. You can also start by copying the 'example.md' file from the /static folder. If you are actively working on a page and need to push it to github, but do not want the page to show on the website, set the 'draft:' tag to 'true' and it will not be included when the website generates.

### Editing Hugo Files

[Hugo](https://gohugo.io/) is a framework that uses Markdown files to generate html files, these frameworks are referred to as 'static site generators'. The important things to remember when updating the content is to match the existing file structure as much as possible as Hugo builds the webpages/folders from the folder structure, and to pay attention to the 'Front Matter' on the pages. Front Matter is what Hugo calls the 'Header' type section of each Markdown document, the area delineated with the ---. And again only make changes to the 'source' branch in GitHub, the 'main' branch will be automatically generated. Otherwise you can find more information about [Markdown syntax here](https://www.markdownguide.org/basic-syntax/) and more information about [Hugo here.](https://gohugo.io/documentation/)

### Editing the Theme

This website was built using the Hugo [geek-docs](https://github.com/thegeeklab/hugo-geekdoc) theme. The theme files exist within the repository so they can be edited freely like the rest of the content. Be aware when updating the theme that you should download and install the 'pre-release bundle build' which is necessary for GitHub pages. Updates to the CSS should be done by making edits to the custom.css file within the 'static' folder.

## Wiki Hosting

This website is hosted 'server-less' through GitHub Pages. After you do updates to the content of the repository the website will automatically do a code check and then regenerate the website once that check clears. If the code check fails then the website will not regenerate until the errors are cleared. The URL wiki.casat.org is applied via the Pages tab in the settings of this repository and within our DNS entries in DigitalOcean, it also must be defined in the CNAME file in the 'static' folder.
