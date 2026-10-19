---
layout: page
title: Projects
permalink: /projects/
---

I love making and breaking things to see how they work. Most of my projects have been on hold while I completed a master's in Computer Science. Since finishing, I've had a little more time to devote to personal projects while my toddler and baby are asleep, here's what I've been working on:

---

### [PAIR]() - This one's a little bit sensitive

PAIR stands for Pentest AI Reporter. For what I hope are obvious reasons, I shall not make the code or the design public. But I'm happy to discuss my thoughts on AI in the cybersecurity landscape. This project is a harness designed for a local AI with guardrails mostly removed to conduct a very specific penetration test of a very specific network. The difficulty hasn't been in getting the AI to hack. My ethical hacking concience and desire to use this technology for good have made me build this with guardrails in the harness, rather than in the AI model. The model mostly directs the operation with every command and proceedure being checked against a global and project specific allowlist. Test and target environments are sandboxed and no rogue AI behaviour is accepted.

---

### [HackerTime](https://github.com/HackerTimeQuest) - U Can't Hack This

This project has been at the back of my mind for a while now. I love sites like Try Hack Me and Hack The Box. I think they provide excellent resources to learn and practice all aspects of cybersecurity. One issue I've had with them both, consistently, is the speed (or lack thereof) of the VMs / cyberrange environments. HackerTime and the labs at HackerTimeQuest are my attempt to build a PAYG cyberrange that offers flexible provisioning for the cyberrange assets. If the lab is a simple ssh pwnbox or similar, a container might be all that is required. If you want to get hands on with a full Elastic Stack implementation, you're going to need some CPU and memory behind it unless you want it to feel like quicksand. That's the inspiration behind this project. My actual experiences on Try Hack Me, trying desperately to use the Elasticsearch UI and drowning in VM juice.

---
### [πPhone](https://github.com/Tomnowell/Mercury) - Reinventing the Wheel
I've had this (probably ridiculous) idea bouncing round my head for a while now. Wouldn't it be nice to have a solid, corded phone that's always in the same place and rings when friends and family call it? This (far from finished) project implements a simple VoIP phone system with a registry server to track phone numbers and IDs. 

The idea is that users will be able to have their very own semi-private phone network running over the internet. Dangerous - yes, I'm sure it is! Don't rely on it for sensitive communications. Bonkers - almost certainly - if you have a smartphone in your pocket and want to talk to your friends, just fire-up Skype...oh no... RIP Skype. But, possibly an awesome use for the Rasberry Pi you've had kicking around forever.

Okay, there's no hardware support yet, but one of my early goals is to implement a simple dialling system that will work with a real tone-dialing phone of yore... The middle ages is calling, they wan't their Yellow-Pages back, please.

If only a fruity trillion-dollar company would take this project on, it would be the most delicious telephony system..served with ice-cream...

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

---

### [MacBox](https://github.com/Tomnowell/macbox) - Learning to Swift

While I have recently really enjoyed coding in Go and Python, I like iOS and macOS and thought it would be good to learn some Swift. This project currently not shipped but I've been Alpha testing it for a while. If you'd like to help me Beta test drop me a message.
[![MacBox on Github](/images/macBox.jpeg)](https://github.com/Tomnowell/macbox)

---

### [GoDis](https://github.com/Tomnowell/GoDis)

[GoDis](https://github.com/Tomnowell/GoDis) is an exercise in Go implementing a very basic Redis style cache. This project really allowed me to appreciate Go's simplicity while exploring go routines and ways to approach unit testing in Go.

I really should have called it reDisGo 🕺 ...branding is not my speciality.

---

### [Reggie](https://github.com/Tomnowell/Reggie)

[Reggie](https://github.com/Tomnowell/Reggie) was my first project in Go and implements a very basic recursive regex matcher. The initial implementation was heavily influenced by [Rob Pike's regex matcher](https://www.cs.princeton.edu/courses/archive/spr09/cos333/beautiful.html) in C. Although this example adds a few extras which were a lot of fun to make.

---

### [TicketyDo](https://github.com/Tomnowell/TicketyDo) - An exercise in CI/CD - ToDo List with built-in Pomodoro

This is an ongoing project for a product that almost certainly doesn't need to exist, but I like consolidating organisational apps and this is going to be the app I wanted when I was studying and trying to raise a family and work a full-time job. It's also really fun to explore GitHub Actions with Render for CD. I've done Azure DevOps stuff before, but it's more interesting to spread it out over different providers.
[![ticketydo on github]](https://github.com/Tomnowell/TicketyDo]
[ticketydo](https://ticketydo.com)