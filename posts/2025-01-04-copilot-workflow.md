---
title: My GitHub Copilot workflow
description: How I'm using GitHub Copilot for development at work
date: 2025-01-04
tags:
  - data
  - engineering
  - copilot
  - gen-ai
  - github
extraWideMedia: false
opengraph:
  image: /assets/images/article.png
---

I think gen AI hype is annoying, but still find it useful. This is how I'm using GitHub Copilot at work.

Despite being a little exhausted of the generative artificial intelligence hype cycle we're in, it's not to the extent
that I don't use it.

## Day-to-day activities

Daily tasks in my data engineering role typically involve using an inherited, well-established codebase that contains a
lot of reusable, generic functions for which inputs are tweaked per data source.

Because of that, I don't think it's particularly well-suited for GitHub Copilot and so I don't tend to use it here. This
has a positive side effect that I'm more familiar with the quirks of that inherited codebase, which in turn makes it
easier to review my colleagues' pull requests.

We mainly stick with PyCharm as our IDE because it's what other team members (data scientists, data engineers,
quantitative analysts) also use, and that makes it easier to troubleshoot issues and help each other out. I do have
[Full Line code completion](https://www.jetbrains.com/help/idea/full-line-code-completion.html) switched on here. It
gives fairly helpful hints and is not intrusive so does well in this workflow.

## More experimental tasks

On occasions that I find myself doing something a bit different---perhaps in another codebase, using a package I'm not
familiar with, or simply experimenting---Copilot makes more sense.

When I first got access to it, Copilot integration with PyCharm wasn't seamless (but I don't recall the details, sorry!)
so I used VS Code with it instead. This separation actually suits me, I believe, and I've kept it like that: instead of
always having Copilot there and it possibly becoming a crutch, it's a deliberate action to switch over to VS Code and
make use of it.

So that workflow is like me offloading a task to Copilot, it helps me come up with some ideas which I then review,
adapt, and take forward what I need.

To help with that workflow, I have the same repos cloned twice to different locations on my file system. For PyCharm
they sit in "PyCharmProjects" and for VS Code it's "copilot_projects".

I then separately work in those IDEs and make distinct commits, either on the same branch or sometimes even create
another branch off of my feature when doing something more experimental with Copilot. If it ends up going down a dead
end or just being a bit rubbish, I can simply delete that sub-branch!

### Git config to identify Copilot-assisted commits

Thanks to
[this StackOverflow answer](https://stackoverflow.com/questions/14754762/can-gitconfig-options-be-set-conditionally/59184292#59184292),
I have a `~/.gitconfig.copilot` with contents:

```
[user]
    email = bob.loblaw+copilot@abc.com
```

Then my main `~/.gitconfig` looks something like:

```
[user]
    email = bob.loblaw@abc.com
    name = Bob Loblaw

[includeIf "gitdir:~/copilot_projects/"]
    path = .gitconfig.copilot
```

Setting things up like this, I can easily go back and see commits Copilot has helped with!
