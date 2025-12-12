---
layout: page
title: Projects
permalink: /projects/
---

I love making and breaking things to see how they work. Most of my projects have been on hold while I completed a master's in Computer Science. Since finishing, I've had a little more time to devote to personal projects while my toddler and baby are asleep, here's what I've been working on:

---
### [TomTimer / TicketyPom](https://ticketypom.com) - Loving Swift
Swift is an awesome language to code in. I really enjoyed playing around with Go, and while Swift does seem to be a bit more complicated, I've noticed some similarities that I like. And some quirks that really stumped me. Implicit self really confused me looking at other peoples' code. I couldn't understand how, what looked like local variables, were still in scope in other methods of a class.

The idea for this project came about while I was doing my Master's degree. I really wanted to lay out a todo list for the day and just get it done. Sounds simple, right? I'm happy to sit and write code for hours on end. I love the sense of flow I get from that level of concentration. But, I struggled to put equal enthusiasm into the more academic parts of my degree such as reading complicated, and not always well written, academic papers. Also synthesising and writing material for my thesis was difficult to prioritize. Pomodorro got me through the painful parts. At that time, I really wanted a simple timer and todo list. Now, I've finished my degree, I've had time to put into it. An open source version is [freely available on GitHub](https://github.com/Tomnowell/tomtimer/) and I've just released it as a free app (with in app purchases) on the apple app store (currently pending approval).

This project taught me a lot about what actually goes into an iOS app. I thought it would be simple to combine a timer and todo list (and it was). But to flesh it out as a SwiftUI app with companion watchOS app, integrate with the built-in Reminders app, and navigate all the agreements and build processes that go into Apple's XCode pipeline was, let's say, educational. 

<div style="display: flex; gap: 20px;">
  <a href="https://ticketypom.com">
    <img src="/images/ticketyPomiOS.png" alt="Tickety Pom iOS screenshot" width="300" />
  </a>
  <a href="https:ticketypom.com">
    <img src="/images/ticketyPomWatchOS.png" alt="Tickety Pom watchOS screenshot" width="300" />
  </a>
</div>

[![TicketyPom.com](/images/TicketyPomiOSL.png)](https://ticketypom.com)

### [MacBox](https://github.com/Tomnowell/macbox) - Learning to Swift
While I have recently really enjoyed coding in Go and Python, I like iOS and macOS and thought it would be good to learn some Swift. This project currently not shipped but I've been Alpha testing it for a while. If you'd like to help me Beta test drop me a message.
[![MacBox on Github](/images/macBox.jpeg)](https://github.com/Tomnowell/macbox)

---

### [GoDis](https://github.com/Tomnowell/GoDis)
[GoDis](https://github.com/Tomnowell/GoDis) is an exercise in Go implementing a very basic Redis style cache. This project really allowed me to appreciate Go's simplicity while exploring go routines and ways to approach unit testing in Go.

---

### [Reggie](https://github.com/Tomnowell/Reggie)
[Reggie](https://github.com/Tomnowell/Reggie) was my first project in Go and implements a very basic recursive regex matcher. The initial implementation was heavily influenced by [Rob Pike's regex matcher](https://www.cs.princeton.edu/courses/archive/spr09/cos333/beautiful.html) in C. Although this example adds a few extras which were a lot of fun to make.

---

### [TicketyDo](https://github.com/Tomnowell/TicketyDo) - An exercise in CI/CD - ToDo List with built-in Pomodoro
This is an ongoing project for a product that almost certainly doesn't need to exist, but I like consolidating organisational apps and this is going to be the app I wanted when I was studying and trying to raise a family and work a full-time job. It's also really fun to explore GitHub Actions with Render for CD. I've done Azure DevOps stuff before, but it's more interesting to spread it out over different providers.
[![ticketydo on github]](https://github.com/Tomnowell/TicketyDo]
[ticketydo](https://ticketydo.com)

---

### Il Pane - A Sourdough Hydration Calculator in C# for Windows

I wrote this simple bread dough hydration calculator in C# to replace a handy little iOS app that I used previously. The iOS app was not updated and eventually would no longer run on the current version of iOS. The application utilises a SQLite database to store recipes.

In the future I plan to use .NET to turn this into a multi platform mobile application. When time allows...

[![Il Pane on Github](/images/ilPane.jpg)](https://github.com/Tomnowell/IlPane)

---

### T-Bay - A Django based Ebay Clone

Written to complete the *Commerce* challenge on Harvards online MOOC CS50 Web, this is my attempt at an ebay clone. While I feel quite comfortable using Python, this was my first attempt at using the Django framework. Good fun!

[![T Bay on Github](/images/Tbay.jpg)](https://github.com/Tomnowell/cs50w-commerce)