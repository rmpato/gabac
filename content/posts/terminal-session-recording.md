---
title: "Recording a terminal session"
date: 2019-11-28T15:15:14-03:00
draft: false
---

Whether you are writing a tutorial post on some blog, being able to further reproduce a set of commands, just taking personal notes, or whatever the reason, it is helpful to record your terminal session, with all its commands and outputs.    

For such a task, I've come across the `script` command, which acording to its man page, just makes a typescript of terminal session.

You can start with a minimal expression, such as 

```
script pepe
```

Where `pepe` will be the output file. 

Once you are done recording your terminal work, you can stop recording with `exit`. 

Then you can `cat pepe` and see how it went. 

You might find unwanted text and keystrokes as `Script started, output file is pepe`. I recommend seeing the man pages (`man script`) to learn how to improve the expected result and adapt it to your needs and taste.

In my case, I'm writing complementary material for an online course I'm working on, so it is helpful to record the terminal, instead of copying and pasting as I go, so I can quickly create guides. The form of the command I'm more frequently using is based on: 

```
script -q -k pepe 
```

The `-q` option suppress messages such as the script started and the script finished.  

The `-k` option logs keys sent to the program as well as output, omitting the datetime stamp. 

Super interesting thing: you can record sessions with a particular format and reproduce them with `scriptreplay`. Still thinking of ways this can help me create course material.  