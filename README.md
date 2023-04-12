# Website

This website is built using [Docusaurus 2](https://docusaurus.io/), a modern static website generator.

### Installation

```
$ yarn
```

### Local Development

```
$ yarn start
```

This command starts a local development server and opens up a browser window. Most changes are reflected live without having to restart the server.

### Build

```
$ yarn build
```

This command generates static content into the `build` directory and can be served using any static contents hosting service.

### Deployment

Using SSH:

```
$ USE_SSH=true yarn deploy
```

Not using SSH:

```
$ GIT_USER=<Your GitHub username> yarn deploy
```

If you are using GitHub pages for hosting, this command is a convenient way to build the website and push to the `gh-pages` branch.

---
# AkTeck Docs Guidelines

Let's discover **Tech's  documentation**.

## Getting Started

Get started by **ensuring you have the following**.
- Valid email Id
- Account In Github
- Contrubution Access required


### What you'll need
- Mind-Set to write your own documentation
- Write only Important documents.
- Any Technical skill

---

# Git hub policy

# Overview 
This document provides instruction to be followed by Tech team.

- Creating branches
- Creating PR’s 
- Merge flow


## Creating Branches
- New branch to be cloned from the Main/Master branch for Creating a documentation 

### 1 Naming the branches
- Feature branches for creation a dock should name in the following format 
  - **branch name**: feature/create-dock-{yourname}
  - **example:** feature/create-dock-akshay

- Feature branches for updation a dock should name in the following format 
  - **branch name**: feature/update-dock-{yourname}
  - **example:** feature/update-dock-akshay

- Feature branches for deletion a dock should name in the following format 
  - **branch name**: feature/delete-dock-{yourname}
  - **example:** feature/delete-dock-akshay

### 2. Creating Pull Requests
- Once you complete your task on perticular document then you have to push it on your on branch.
- You will have to request for a peer review.
- Before raising a PR please Take a look on documents.
- After the successful review create a PR for merge branch.
- PR should contain the information on the change made to the code in the comment.

## 3. Merge flow & Release

- Merge branch to be created by the lead.
- You need to merge your branch into the Main/Master branch


