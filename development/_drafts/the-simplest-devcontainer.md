---
layout: post
title:  "The Simplest Devcontainer"
date:   2022-10-28 17:00:00 -0400
category: development
tag: devcontainer
---

# The simplest devcontainer.json 

Getting started with devcontainers and CodeSpaces is not as daunting as it might seem. This is a short example-driven guide, but I also recommend the [docs on GitHub](https://docs.github.com/en/codespaces/setting-up-your-project-for-codespaces/introduction-to-dev-containers) for a more thorough reference.

## From scratch

By default, you can create a codespace with many useful tools installed. But it doesn't have figlet, an essential tool for any _serious_ developer. Case in point:

```txt
 _____ _       _      _     _        __             
|  ___(_) __ _| | ___| |_  (_)___   / _|_   _ _ __  
| |_  | |/ _` | |/ _ \ __| | / __| | |_| | | | '_ \ 
|  _| | | (_| | |  __/ |_  | \__ \ |  _| |_| | | | |
|_|   |_|\__, |_|\___|\__| |_|___/ |_|  \__,_|_| |_|
         |___/                                         
```

So, lets upgrade our development environment with all the tools we need to ~~have fun~~ get work done.

## .devcontainer

First, checkout a repository you want to create a devcontainer for. 

Add a directory called `.devcontainer` to the root of your repository. 

Inside `.devcontainer`, add `devcontainer.json`.

```txt
.devcontainer
|-- devcontainer.json
```

## devcontainer.json

Populate devcontainer.json with the following: 

```jsonc
{
  // a name for your development environment
  "name": "my first dev container",
  
  // we are going to build the container image rather than fetching from a registry 
  "build": {

    // the dockerfile to build the dev container with, relative to devcontainer.json
    "dockerfile": "Dockerfile"
  }
}
```

## The Dockerfile

Next, create a Dockerfile inside `.devcontainer`.

```txt
.devcontainer
|-- devcontainer.json
|-- Dockerfile
```

This is a regular Dockerfile you can use to set up your development container. In this case, I've chosen to work with the latest Ubuntu image and install `figlet`.

```Dockerfile
FROM ubuntu:latest

RUN apt-get update
RUN apt-get install figlet
```

## Let's run it!

... steps in GH to create

Well... there aren't many tools. Not even git. But there is figlet, and that's all that really matters, right?

```txt
root@codespaces-58b820:/workspaces/equipper# git status
bash: git: command not found
root@codespaces-58b820:/workspaces/equipper# figlet "figlet > git"
  __ _       _      _    __          _ _   
 / _(_) __ _| | ___| |_  \ \    __ _(_) |_ 
| |_| |/ _` | |/ _ \ __|  \ \  / _` | | __|
|  _| | (_| | |  __/ |_   / / | (_| | | |_ 
|_| |_|\__, |_|\___|\__| /_/   \__, |_|\__|
       |___/                   |___/       
```

## We do need real tools, though

Joking aside, we need a devcontainer that starts with a baseline of helpful tools like git. Sorry, figlet. The [default GitHub devcontainer](https://github.com/devcontainers/images/tree/main/src/universal) image has worked well for me in the past, so I'm going to start with it. I'll choose `mcr.microsoft.com/devcontainers/universal:2.0-focal` rather than `mcr.microsoft.com/devcontainers/universal:linux` so that I have more control over updates.

```Dockerfile
FROM mcr.microsoft.com/devcontainers/universal:2.0-focal

RUN apt-get update
RUN apt-get install figlet
```

After pushing the changes and making a new codespace off my branch, I'm greeted with a much more promising console (and still figlet!): 

```txt
👋 Welcome to Codespaces! You are on our default image. 
   - It includes runtimes and tools for Python, Node.js, Docker, and more. See the full list here: https://aka.ms/ghcs-default-image
   - Want to use a custom image instead? Learn more here: https://aka.ms/configure-codespace

🔍 To explore VS Code to its fullest, search using the Command Palette (Cmd/Ctrl + Shift + P or F1).

📝 Edit away, run your app as usual, and we'll automatically make it available for you to access.

@benCoomes ➜ /workspaces/equipper (add-devcontainer) $ dotnet --version
6.0.402
@benCoomes ➜ /workspaces/equipper (add-devcontainer) $ git status
On branch add-devcontainer
Your branch is up to date with 'origin/add-devcontainer'.

nothing to commit, working tree clean
@benCoomes ➜ /workspaces/equipper (add-devcontainer) $ figlet SUCCESS
 ____  _   _  ____ ____ _____ ____ ____  
/ ___|| | | |/ ___/ ___| ____/ ___/ ___| 
\___ \| | | | |  | |   |  _| \___ \___ \ 
 ___) | |_| | |__| |___| |___ ___) |__) |
|____/ \___/ \____\____|_____|____/____/ 
                                         
@benCoomes ➜ /workspaces/equipper (add-devcontainer) $ 
```

There we have it - a development environment that is finally... 
```txt
 _____       _                       _             ____               _      
| ____|_ __ | |_ ___ _ __ _ __  _ __(_)___  ___   / ___|_ __ __ _  __| | ___ 
|  _| | '_ \| __/ _ \ '__| '_ \| '__| / __|/ _ \ | |  _| '__/ _` |/ _` |/ _ \
| |___| | | | ||  __/ |  | |_) | |  | \__ \  __/ | |_| | | | (_| | (_| |  __/
|_____|_| |_|\__\___|_|  | .__/|_|  |_|___/\___|  \____|_|  \__,_|\__,_|\___|
```