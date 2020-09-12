---
title: Publishing DEV articles from GitHub
tags: showdev, github, actionshackathon, opensource
---

How do you write your article for [DEV](https://dev.to/)? Do you use the markdown editor provided by DEV on the browser? Or do you use your favorite editor on your local machine? If you do use a regular editor on your PC, how do you submit the article to DEV?

I like to use [my favorite editor](https://neovim.io/) to write articles. When I am ready to publish an article, I have to copy and paste the texts into the online editor, then click the "Publish" button. Sometimes I want to see how DEV is showing the article while I am in the middle of the writing. Therefore, my writing experience usually involves more than one copy-pasting. As you can imagine, this is troublesome, tedious, and not a good experience at all.

I also like to use [Git](https://git-scm.com/) to manage the versions of my articles. Then I can also push the articles into [GitHub](https://github.com). Since GitHub renders Markdown files, readers also can read the articles on GitHub if they choose to. Of course, images should work as expected on GitHub if we use relative links and commit the image files too. Now I also have a backup system for my articles.

# Workflow with GitHub Actions

![Flow of submitting article to DEV from GitHub](./images/dev_submission_flow.svg)

I want everything described above to be fully automated. The diagram above shows the workflow I want to realize with GitHub Actions. When I want to start writing a new article, I will branch out from the main branch and start writing. When I want to see how this article looks on DEV, I can push the new branch to GitHub and make a (draft) pull request. At this instance, my GitHub Action should submit the article to DEV as a draft. I can merge the branch back into the main branch when I am ready to publish the article.
