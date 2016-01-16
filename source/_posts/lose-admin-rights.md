---
title: How to Drop Administrator Elevation Rights on Windows
date: 2016-01-10 15:20:19
tags:
- Windows
- Tips
---

This might not be a popular topic but it is something that has been raised a few times by colleagues at Unity. Most developers who work with Windows know that you can elevate yourself from being a non-admin user to being an administrator with elevated rights, but what about dropping back down again so that you can run things as a user with normal rights? Not quite as simple.

So in a perfect world this post would end right here with a simple command line to run, but this isn't a perfect world and there isn't a *simple* way to do this. There are however a number of options depending on how often you want to be doing this task.
<!-- more -->


One of my favorite ways to do this is to use the task scheduler (it's the simplest option I know without having to write your own C++ application), with this you can run things as an elevated or normal user without working too hard to do it. If you want to run a specific task again and again always as a normal user then you can create your self a task by creating a task either programatically or manually with the task scheduler.

As I'm a developer I'll show you the more interesting way of doing this by creating it with the command line.


{% codeblock lang:bat line_number:false create_task.bat https://msdn.microsoft.com/en-us/library/windows/desktop/bb736357(v=vs.85).aspx schtasks %}
schtasks /Create /SC ONCE /TR "echo 'Your command here'" /IT /ST 00:00 /SD 01/01/2000 /TN "My Task"
{% endcodeblock %}

So what does this do? Lets discuss the more interesting of the flags.

#### /TR

This simply defines what will run each time this task is ran. You can run anything that you would from the command line here including scripts.

#### /TN
The name of the task which we will use to run the task later on.

#### /ST & /SD
These flags define when the task is ran I picked a random date as we only really want to launch this task manually from the command line at a later point.

Now that we have created our task we need to be able to run it, you can do that with this simple command.

{% codeblock lang:bat line_number:false create_task.bat https://msdn.microsoft.com/en-us/library/windows/desktop/bb736357(v=vs.85).aspx schtasks %}
schtasks /Run /TN "My Task"
{% endcodeblock %}

And that's it your task will now run as the currently logged in user without elevation. Neat huh? Okay I know you can only run a static command with this. What about if you want to change your command each time you run it?

There is another way to do this, it's a bit of a hack but it does work.

The basic principle is to get another application to open the command that you want to open, and one of the best applications to target is explorer.exe because that is already designed to allow you to do that task.

I could now spend 15 minutes of your time explaining to you how to do this, however this idea is not new and neither my own, so for everyones sake I will let Microsoft themselves explain the ins and outs as they have a [great blog post](http://blogs.msdn.com/b/winsdk/archive/2013/06/18/launching-a-process-as-a-normal-user-from-an-elevated-user.aspx) on this very problem. However if your lazy and don't want to move onto their fantastic blog well then I can tell you that you simply need to get a handle of the explorer process and to use the WinAPI function *CreateProcessWithTokenW*. This will allow you to run any applications, it's not as simple but a lot more powerful than doing it via the Task Scheduler.