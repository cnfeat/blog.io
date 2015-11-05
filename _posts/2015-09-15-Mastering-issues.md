---
layout: post
title: 玩转 Issue
date: 2015-09-05
categories: blog
tags: [github,麦肯锡]
description: 议题（Issue）是 github 中管理任务的有效方法。


---




## 玩转 Issue

议题（Issue）是 github 中管理任务的有效方法，它能保存任务的进度、跟踪任务的管理和追溯项目的bugs。它有点像可以用来跟团队分享和讨论的邮件。大多数的软件项目都有各自 bugs 追溯系统，GitHub的追溯系统就叫做议题，它在 github 的仓库中发挥着独特的作用。

举个例子，我们来看 Bootstrap 的议题：

因为 GitHub 的运作机制更关注团队的协作、关联和高效沟通，所以 issue 的追溯会设置得比较特别。一个典型 GitHub 的 issue 应具备以下要素：

- **标题和说明**，来解释 issue 的目的；
- **彩色标签**（Color-coded labels），方便日后对 issue 进行分类；
- **里程碑**（milestone），用里程碑来汇总相关的 issue，表示要实现的阶段性目标。例如（一周冲刺 9/5-9/16 或 Shipping 1.0)
- **对接人**（assignee），希望对接人能够在某时间段对 issue 负责；
- **评论**（comment），这样才能接收到别人对仓库的反馈。



## 里程碑、标签和对接人

issue 一旦多了起来，你就可能会一时无可适从了，这是你就需要使用 issue 中的「里程碑」、「标签」和「对接人」的功能了，它们能够及时对 issue 进行标注和分类，让你迅速地找到你想要的信息。

你可以在右边的侧栏中找到「里程碑」、「标签」和「对接人」的小齿轮，单击它们的小齿轮，你就可以对其进行修改或增加。

如果你没有看到可编辑的小齿轮，那证明你还没有可修改该 issue 的权限，你可以向该 issue 的管理员申请管理员权限。

「里程碑」是一系列 issue 的集合，它可以通过项目、功能或时段来设定。程序员们喜欢用「里程碑」在软件管理中做各种各样的事情，例如：

- Beta 测试：在项目 Beta 版上线之前，你需要对各种文件的 bug 进行检测，设定「里程碑」确保你不会错过每一条信息。
- 十月冲刺：当你有很多事情要做的时候，你可以将你要在十月完成的工作都集中在一起，以便集中精力处理。
- 重构：你可以在「里程碑」收集建议，将相关的 issue 进行项目重构。




## Mastering Issues

## 玩转 Issue

Issues are a great way to keep track of tasks, enhancements, and bugs for your projects. They’re kind of like email—except they can be shared and discussed with the rest of your team. Most software projects have a bug tracker of some kind. GitHub’s tracker is called Issues, and has its own section in every repository.

议题（Issue）是 github 中管理任务的有效方法，它能保存任务的进度、跟踪任务的管理和追溯项目的bugs。它有点像可以用来跟团队分享和讨论的邮件。大多数的软件项目都有各自 bugs 追溯系统，GitHub的追溯系统就叫做议题，它在 github 的仓库中发挥着独特的作用。

For example, let’s take a look at Bootstrap’s Issues section:

举个例子，我们来看 Bootstrap 的议题：

GitHub’s issue tracking is special because of our focus on collaboration, references, and excellent text formatting. A typical issue on GitHub looks a bit like this:

因为 GitHub 的运作机制更关注团队的协作、关联和高效沟通，所以 issue 的追溯会设置得比较特别。一个典型 GitHub 的 issue 应该是这样的：

* A title and description describe what the issue is all about.
* Color-coded labels help you categorize and filter your issues (just like labels in email).
* A milestone acts like a container for issues. This is useful for associating issues with specific features or project phases (e.g. Weekly Sprint 9/5-9/16 or Shipping 1.0).
* One assignee is responsible for working on the issue at any given time.
* Comments allow anyone with access to the repository to provide feedback.

- 有标题和说明，来解释 issue 的目的；
- 有合适的彩色标签（Color-coded labels），方便日后对 issue 进行分类；
- 有里程碑（milestone），用里程碑来汇总相关的 issue，表示要实现的阶段性目标。例如（一周冲刺 9/5-9/16 或 Shipping 1.0)
- 有明确的对接人（assignee），希望对接人能够在某时间段对 issue 负责；
- 有评论（comment），这样才能接收到别人对仓库的反馈。

## Milestones, Labels, and Assignees

## 里程碑、标签和对接人

Once you’ve collected a lot of issues, you may find it hard to find the ones you care about. Milestones, labels, and assignees are great features to filter and categorize issues.

You can change or add a milestone, an assignee, and labels by clicking their corresponding gears in the sidebar on the right.

If you don’t see edit buttons, that’s because you don’t have permission to edit the issue. You can ask the repository owner to add you as a collaborator to get access.

issue 一旦多了起来，你就可能会一时无可适从了，这是你就需要使用 issue 中的「里程碑」、「标签」和「对接人」的功能了，它们能够及时对 issue 进行标注和分类，让你迅速地找到你想要的信息。

你可以在右边的侧栏中找到「里程碑」、「标签」和「对接人」的小齿轮，单击它们的小齿轮，你就可以对其进行修改或增加。

如果你没有看到可编辑的小齿轮，那证明你还没有可修改该 issue 的权限，你可以向该 issue 的管理员申请管理员权限。

## Milestones

## 里程碑

Milestones are groups of issues that correspond to a project, feature, or time period. People use them in many different ways in software development. Some examples of milestones on GitHub include:

- Beta Launch — File bugs that you need to fix before you can launch the beta of your project. It’s a great way to make sure you aren’t missing anything.

- October Sprint — File issues that you’d like to work on in October. A great way to focus your efforts when there’s a lot to do.

- Redesign — File issues related to redesigning your project. A great way to collect ideas on what to work on.

「里程碑」是一系列 issue 的集合，它可以通过项目、功能或时段来设定。程序员们喜欢用「里程碑」在软件管理中做各种各样的事情，例如：

- Beta 测试：在项目 Beta 版上线之前，你需要对各种文件的 bug 进行检测，设定「里程碑」确保你不会错过每一条信息。
- 十月冲刺：当你有很多事情要做的时候，你可以将你要在十月完成的工作都集中在一起，以便集中精力处理。
- 重构：你可以在「里程碑」收集建议，将相关的 issue 进行项目重构。

Labels

Labels are a great way to organize different types of issues. Issues can have as many labels as you want, and you can filter by one or many labels at once.



Assignees

Each issue can have an assignee — one person that’s responsible for moving the issue forward. Assignees are selected the same way milestones are, through the grey bar at the top of the issue.

### 标签

你可以用各种你想要标签来有效地管理 issue，有了标签，你立马就可以从一堆标签中筛选出你想要的 issue。

### 对接人

每个 issue 都应有一个负责推进对接人，对接人的设定方式和里程碑的设定一样：点击对应的齿轮按键选择即可。

Notifications, @mentions, and References

By using @mentions and references inside of Issues, you can notify other GitHub users & teams, and cross-connect issues to each other. These provide a flexible way to get the right people involved to resolve issues effectively, and are easy to learn and use. They work across all text fields on GitHub — they’re a part of our text formatting syntax called GitHub Flavored Markdown.

If you’d like to learn more, have a look at Mastering Markdown.

### 通知，@提醒 和 引用

你可以使用 issue 中的 @提醒 和 引用 来通知 GitHub 用户和团队，或相互链接到其他的 issue。通知，@提醒和引用 在 github 中使用GitHub Flavored Markdown全文本格式的流转，上手容易，且能灵活准确地通知对接人快速高效解决 issue 中的问题。

如果你想更多地了解 GitHub Flavored Markdown，可点击 Mastering Markdown。

Notifications

Notifications are GitHub’s way to keep up to date with your Issues. You can use them to find out about new issues on repositories, or just to know when someone needs your input to move forward on an issue.

There are two ways to receive notifications: via email, and via the web. You can configure how you receive notifications in your settings. If you plan on receiving a lot of notifications, we like to recommend that you receive web + email notifications for Participating and web notifications for Watching.

### 通知

通知是一种让你跟踪 issue 动态的方式，他带有明显 GitHub 特色，你可以由此掌握仓库的新 issue，接收别人对你的输出请求。

接收通知的方式有两种：邮件和网页。你可以自由设置通知的接收方式，如果你乐意接收大量的通知，我们推荐你使用 邮件+网页 的组合，这样，你就用邮件来参与回复通知，用网页来查看通知。












