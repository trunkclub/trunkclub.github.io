---
layout: post
title: Programming on the Command Line
author: Joel Ash
comments: true
categories:
- cli
- programming
---

## The command line is your friend

Recently it dawned on me that one of the most useful things I’ve learned since college was actually one of the older technologies when it comes to computers: the unix command line and how to “program” using it.

<!-- more -->

## How is CLI programming

What I’ve always heard referred to as “Taco Bell ® Programming” is the art of combining several commands to acheive something much more powerful. This is nothing crazy, we’ve probably all tailed a filed and piped (|) it to grep before, that makes you a taco bell ® programmer. For those of you who have never done that, let me explain. tailing a file, simply means that your display is constantly being updated with the contents of the file. This is very useful when you want to see the contents of a log file. But many times when you’re looking for something in a log file, you don’t want or need to see every line of that file, you’ve logged much more than you need to troubleshoot this issue. This is where grep comes in, it allows you to only show the lines of a body of text that contain a phrase you’re looking for. Here’s an example where I’m following my production log file and looking for the lines that contain “Publishing Message”:

``` sh
$ tail -f production.log | grep ''Publishing message''
```


That’s just the begining of how the command line can be useful. Let’s say that this message returns us a lot of lines that looks like:

``` ruby
Publishing message: exchange: member_example_event, opts: {:headers=>{"message_id"=>"message2"}}, message: "{\"id\":123456,\"first_name\":\"Example\",\"last_name\":\"Member\",\"email\":\"example.member@example.com\"}"
```

And from these lines we want a list of all the member’s email addresses. How do we do this? Yes, I’m sure most of you are thinking, “Why not write a ruby script?” (where ruby could be any language). Some of you might even be thinking “I can do that in VIM”. (That one would be me). But why not just use the command line? A tool that is already at your disposal? Probably because, you’re not familiar with how to. Let’s do this piece by piece:

First we need to remove the parts of the message we don’t need. To do this, I would use the cut command, which acts much like split in ruby. I’m going to cut on the ‘,’ (-d,) and then take only the parts of the cut line that I want by passing -f6.

``` sh
$ tail -f production.log | grep ''Publishing message'' | cut -d, -f6
    #\"email\":\"example.member@example.com\"}"
```

Now, I think I would cut that again on ‘”’ to get to my email address, but this leaves a trailing ‘', so I’ll use sed, which is like sub in ruby to remove it:

``` sh
$ tail -f production.log | grep ''Publishing message'' | cut -d, -f6 | cut -d''"'' -f4 | sed ''s/\\//''
    #example.member@example.com
```

And we’re done! Now we can tail the logs and see on our screen the email addresses for members who have had a message published about them in our system. How would you have accomplished this? Similarly or completely different?

This example may be a bit contrived, however being able to string together a series of unix commands can be very useful, especially if you’re sshed into a server and might not have the ability to create a file, or maybe that server does not have ruby installed.

Why is it called Taco Bell ® Programming?

I cannot take this credit for this and I cannot remember where I first heard this term, but searching the internet shows that it seems to come from [Ted Dziuba](http://teddziuba.com/), but his blog post on it is no longer available. What it comes down to is that most of the food Taco Bell’s ® is made of by combining many of the same ingredients differently. When we’re writing this helper scripts on the command line we’re just combining all of these programs that already exist produce a program that fits our need, each time.

Learning how to use these commands

Personally I find my self just reading man (manual) pages on these apps or googling some examples for them. Another useful linux tool is apropos which can be used to search all available commands and their descriptions.

Some useful links

- [grep](http://unixhelp.ed.ac.uk/CGI/man-cgi?grep)
- [cut](http://unixhelp.ed.ac.uk/CGI/man-cgi?cut)
- [sed](http://unixhelp.ed.ac.uk/CGI/man-cgi?sed)
