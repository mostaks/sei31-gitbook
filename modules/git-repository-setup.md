---
description: Git lit
---

# Git Repository Setup

* ​[Local Repositories](https://granthanrahan.gitbook.io/wdi27/modules/git-repository-setup#local-repositories)​
* ​[GitHub Remotes](https://granthanrahan.gitbook.io/wdi27/modules/git-repository-setup#github-remotes)​
* ​[Heroku Remotes](https://granthanrahan.gitbook.io/wdi27/modules/git-repository-setup#heroku-remotes)​
* ​[GitHub Pages Remotes](https://granthanrahan.gitbook.io/wdi27/modules/git-repository-setup#github-pages-remotes)​
* ​[Example Rails Project with GitHub and Heroku Remotes](https://granthanrahan.gitbook.io/wdi27/modules/git-repository-setup#example-rails-project-with-github-and-heroku-remotes)​
* ​[Markdown](https://granthanrahan.gitbook.io/wdi27/modules/git-repository-setup#markdown)​
* ​[Project READMEs](https://granthanrahan.gitbook.io/wdi27/modules/git-repository-setup#READMEs)​

## Introduction <a id="introduction"></a>

Git is a distributed version control system designed to help developers manage their code and collaborate with other developers.

It's important to remember that `git !== GitHub`. Git lives on your computer. GitHub is one of several web-based Git repository hosting services, which allows you to store your code in the cloud, share your code with others and collaborate with other developers.

## Local Repositories <a id="local-repositories"></a>

These live on your computer. They can \(but don't need to\) be linked to **remote** repositories on GitHub, Heroku, etc.

### Creating a local repository from scratch <a id="creating-a-local-repository-from-scratch"></a>

Before creating a repository, you should always ensure that the directory you are in is not part of another Git repository by running `git status` - if this returns anything other than `fatal: Not a git repository (or any of the parent directories): .git`, you are inside an existing Git repository!

```bash
$ git status
  #=> fatal: Not a git repository (or any of the parent directories): .git
$ mkdir green-bastard
$ cd green-bastard
$ git init
```

This will initialize a new Git repository in the directory `green-bastard`.

If you have accidentally initialized a new Git repository, you can simply delete the hidden `.git` folder within the directory to change your directory back to an ordinary, untracked directory.

```bash
$ ls -a 
#=> .git
$ rm -r .git
```

### Cloning a remote repository into a local repository <a id="cloning-a-remote-repository-into-a-local-repository"></a>

If you want to create a local copy of a repository on GitHub, follow the steps below.

1. Go to the GitHub repository you want to clone into a local repository.
2. Under your repository name, click **Clone or download**.
3. Copy the URL in the **Clone with HTTPS** section.
4. Open Terminal.
5. Go to the parent directory into which you want to clone the repository.
6. Clone the repository using the command below - this will create a new directory \(with the name of the repository being cloned\) within the directory in which the command was run.

```bash
$ git clone https://github.com/cjbarnaby/green-bastard.git
  => Cloning into 'green-bastard'...
  => [etc]
  => Checking connectivity... done.
```

## GitHub Remotes <a id="github-remotes"></a>

You may want to add remote GitHub repositories for a local Git repository to:

* Backup your code.
* Collaborate with other developers.
* Contribute to open source projects.
* Deploy to a [GitHub Pages](https://pages.github.com/) site.
* Open source your code.

To add a GitHub remote \(which we'll call 'origin'\) for a local repository, follow the steps below:

1. ​[Login to GitHub](http://www.github.com/).
2. Click the **+** icon in the top left-hand corner of the page, and select "New Repository".
3. Complete the **Create a new repository** form, and **do not** check the 'Initialize this repository with a README' checkbox.
4. Copy the **HTTPS** url.
5. Open Terminal.
6. Go to your repo's directory.
7. Run the command below.

```bash
$ git remote add origin https://github.com/cjbarnaby/green-bastard.git
$ git push origin master
```

## Heroku Remotes <a id="heroku-remotes"></a>

Heroku is a platform for hosting and deploying full-stack web applications. It supports a number of different back-end languages and frameworks, including Ruby on Rails, and allows the straightforward deployment of applications using Git - by creating a Heroku app and adding a Heroku remote, you can deploy by simply pushing your code to the Heroku remote.

After you have [signed up for Heroku](https://signup.heroku.com/) and [installed and configured the Heroku CLI](https://toolbelt.heroku.com/), you can create Heroku Apps \(and add Heroku Remotes\) from the command line; while you can create a Heroku app from the [Heroku web interface](http://www.heroku.com/), creating the app using the CLI will automatically add the Heroku remote to your local repo.

The guide below covers the steps for adding a Heroku remote to a Git repo. For an end-to-end guide to creating a Rails app with a Postgresql database to be deployed on Heroku, see [Basic Rails and Heroku Project Setup](https://gist.github.com/cjbarnaby/52640b149f19cf1eb052f2c0fdb2db36).

### Adding a Heroku Remote for a new Heroku app <a id="adding-a-heroku-remote-for-a-new-heroku-app"></a>

Where you have an initialized Git repository, the following Terminal commands will create a Heroku application and add the Heroku remote to your Git repository \(note that your Heroku application name does not need to match your GitHub repository name, or the folder in which your local Git repository is stored - it does, however, need to follow url character rules and be unique to Heroku\).

```bash
$ cd green-bastard
$ heroku create green-bastard
  => Creating ⬢ green-bastard... done
     https://green-bastard.herokuapp.com/ | https://git.heroku.com/green-bastard.git
```

### Adding a Heroku Remote for an existing Heroku app <a id="adding-a-heroku-remote-for-an-existing-heroku-app"></a>

1. Click on the Heroku application on the [Heroku Dashboard](https://dashboard.heroku.com/).
2. Click the 'Settings' tab.
3. Copy the Git URL in the Info section.
4. Open Terminal.
5. Go to your repo's directory.
6. Run the command below.

```bash
$ git remote add heroku https://git.heroku.com/green-bastard.git
```

## GitHub Pages Remotes <a id="github-pages-remotes"></a>

GitHub Pages allows you to host \(front-end\) sites directly from your GitHub repository. There are two types of GitHub Pages sites - Project Sites and Portfolio Sites.

### Project Site <a id="project-site"></a>

Each GitHub repository can have an associated GitHub Pages site. GitHub Pages project sites are automatically generated when you push valid HTML to a branch called `gh-pages` in your GitHub repository.

* The name of the branch must be **exactly** `gh-pages` in order for GitHub to deploy the application to GitHub Pages.
* The URL of the GitHub Pages site will be [https://{USERNAME}.github.io/{REPOSITORY}](https://%7Busername%7D.github.io/%7BREPOSITORY%7D). For example, the GitHub repository [https://www.github.com/cjbarnaby/green-bastard](https://www.github.com/cjbarnaby/green-bastard) will have the GitHub Pages URL [https://cjbarnaby.github.io/green-bastard](https://cjbarnaby.github.io/green-bastard).
* The guides below assume that your GitHub remote for your project is called `origin`.

#### _Creating a GitHub Pages Project Site_ <a id="creating-a-github-pages-project-site"></a>

```bash
$ cd green-bastard
$ git branch
  => * master
$ git branch gh-pages
$ git checkout gh-pages
$ git push origin gh-pages
```

#### _Updating a GitHub Pages Project Site_ <a id="updating-a-github-pages-project-site"></a>

You shouldn't do your work in the gh-pages branch. Whenever you commit changes to your master branch that you want to be reflect on your GitHub Pages site, you need to checkout the gh-pages branch, merge in the master branch, push to your gh-pages branch on GitHub, then checkout your master branch to continue working.

```bash
$ cd green-bastard
$ git branch
  => * master  
       gh-pages
$ git add .
$ git commit -m "Updated everything"
$ git push origin master
$ git checkout gh-pages
  => Switched to branch 'gh-pages'
$ git merge master
  => Updating 8f5b2fd..958d58d
      3871 files changed
$ git push origin gh-pages
$ git checkout master
  => Switched to branch 'master'
```

### Portfolio Site <a id="portfolio-site"></a>

Each GitHub account is given a single 'Portfolio' GitHub Pages site. The process for creating and updating a Portfolio GitHub Pages site is identical to that of an ordinary 'Project' GitHub Pages site, with one important difference - the repository name must **exactly** be `{USERNAME}.github.io`. The URL of the GitHub Pages site will be [https://{USERNAME}.github.io](https://%7Busername%7D.github.io/).

## Example Rails Project with GitHub and Heroku Remotes <a id="example-rails-project-with-github-and-heroku-remotes"></a>

* ​[Basic Rails & Heroku Project Setup](https://gist.github.com/cjbarnaby/52640b149f19cf1eb052f2c0fdb2db36)​

## Markdown <a id="markdown"></a>

Markdown \(`.md`\) is a text-to-HTML conversion tool. It is both:

* A plain-text formatting syntax; and
* A software tool that converts plain-text documents to HTML.

It allows you to add formatting to documents in a way that is both:

* Legible in its plain-text form; and
* Able to be converted to semantically valid HTML.

It's important to get comfortable with Markdown, since that will generally be the format you would use for your GitHub repository's README file.

The Markdown Cheat Sheet below is the best quick-reference resource for Markdown syntax.

#### _Markdown - Further Reading_ <a id="markdown-further-reading"></a>

* ​[Adam Pritchard's Markdown Cheat Sheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)​

## Project READMEs <a id="project-readmes"></a>

Your Git repo's README is incredibly important. This could be the only thing a prospective employer/developer looks at when checking out your GitHub profile. You should include things like a link to the live application, what technology you used, an overview of the app and its features, etc. Here is a [README template](https://gist.github.com/cjbarnaby/e0eab63987806d59712ea1923abce652), if you're struggling to know where to start.

