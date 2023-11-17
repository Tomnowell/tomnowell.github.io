# Log Operations

## Learn the operation process details

## SOC Level 2 Learning Pathway

### Introduction

Here we'll take a look at the Tryhackme room [Log Operations](https://tryhackme.com/room/logoperations)

I thought this room would be a breeze. The last one was a bit of a challenge, but this one has no deployable VMs and no files to download. However, this lack of practical aspect means that this room is one of the heaviest in information density and it's too easy to skip over bits without letting them sink in properly. So I will do my best to actually comment on the room and thus, hopefully, consolidate my understanding and hopefully give some added value to anyone reading this.

We're introduced to the needle and haystack metaphor again. It's probably quite a good one however cliche it may sound, but let's extend it a little, just for fun. It's good to have some idea whether the needle you're looking for may be ferrous, in which case a magnet might be helpful. Also, you might want to wear gloves and something with long sleeves if you're going to be sifting through hay all day - stuff gets itchy. Also, you're not going to want to knock the stack over and leave it in a messy pile when you're done. So leaving our metaphor and returning to our literals take care to follow policy on log collection and consolidation and sanitization. Not really sure if that is applicable to this room - I just enjoyed playing with the metaphor - other things happen in haystacks apart from needles getting lost - so use protection.

---

### Task 2: Log Configuration

Here we are asked to think about (or discuss with a team) what the purpose of the logs are.  The for purposes given are: Security, Operational, Legal and Debug. A real world implementation would likely be a balance of these but it separates the considerations well for a discussion. For example if the company are seeking ISO 27001 certification then they might well focus on the Legal (and regulatory) aspects of log collection. Whereas a company that are trying to streamline business processes or make efficiency savings might want to consider the log purposes as debugging tools for their processes. It is likely that no purpose is outright ignored. For example a company focussed on achieving PCI DSS compliance to process credit card information will likely also have to focus on Security purpose and will want to ensure any system's reliability thus implementing the debug purpose.


*Q1: Which of the given log purposes would be suitable to measure the cost of using a service?*

> Cost of services is usually an xxxxxxxxxx concern.

<details>

  <summary>Spoiler warning: Answer</summary>
  
    operational

</details>

*Q2: Which of the given log purposes would be suitable for investigating application logs for enhancement and stability?*

> The key term here is stability - enhancement could also fall under the answer for Q1 but here we want to remove problems.

<details>

  <summary>Spoiler warning: Answer</summary>
  
    debug

</details>

---

### Task 3: Where to Start After Deciding the Logging Purpose

Here we are presented with some guiding questions that we might use in a meeting to decide our logging strategy. The first question is the *what* this is highly influenced by the purpose as discussed previously.

Most questions relate to the *how*. How much will (needs to be) logged? - This likely depends on the purpose also. There may be regulatory stipulations for example if credit card details or personal information are involved. How will logs be created and stored? - This might also be regulated depending on the geographic area of collection (eg. GDPR in Europe). How are logs protected and analysed? Again there may be regulation or answers may be influenced by company information security policy. Finally we consider resources both financial and workforce related. Log maintenance and continuous usage should be considered here.

*Which question's answer can be "as much as mentioned in the PCI DSS requirements."?*

> PCI DSS relates to companies who process credit card information and is quite strict on how much data (and of course what data) is logged.

<details>

  <summary>Spoiler warning: Answer</summary>
  
    How much do you need to log?

</details>

---

### Task 4: Configuration Dilemma: Planning and Implementation

Of course it's resources. Did you think the last two questions for the meeting were going to be easy? Essentially, everything comes down to resources.  There are certain non-negotiable requirements and aspirational requirements and the most successful teams will maintain the base requirements while adding the most useful aspirational requirements possible using the resources that are (or could be made) available to them.

Base requirements lay out the data that must be logged to meet operational, security and legal obligations. Aspirations may include logging data to achieve better business or operational insights thus improving processes yet are not absolutely required to meet security and operational goals.

* Q1: Which requirements are non-negotiable?*

> We've said it in so many ways but we have to get the wording right for the answer to be accepted.

<details>

  <summary>Spoiler warning: Answer</summary>
  
    operational and security requirements

</details>

---

### Task 5: Principles and Difficulties

Some of these principles may seem obvious but the obvious can easily be overlooked so it's always worth thinking about. I won't repeat what the room says - it's good information. I will list the principles though. Log principles include: Collection, format, archival and accessibility, monitoring and alerts, security and change.

Next a discussion of challenges. These should mostly be faced at the planning stage. Challenges include: Volume & noise, performance and collection, process and archive, security, analysis. Other challenges are lumped together under miscellaneous and mostly include the lack of something: planning, resources, technical skills.

Notice, archive and security appear both in principles and challenges. In fact the challenges consider different aspects of the principles so that isn't really surprising. The challenge of data volume applies to both collection and archival and accessibility.

While I don't want to copy the info in the room the final point seems relevant to all aspects of security not just logging so it's worth emphasising: *Adhere to principles and address challenges proactively.*

*Q1: Your team is working on policies to decide which logs will be stored and which portion will be available for analysis. Which of the given logging principles would be implemented and improved?*

> The key words here are stored and available. We're looking at principles. So what's another word for store (especially for a long time)

<details>

  <summary>Spoiler warning: Answer</summary>
  
    Archiving and Accessibility

</details>

*Q2: Your team implemented a brand new API logging product. One of the team members has been tasked with collecting the logs generated by that new product. The team member reported continuous errors when transferring the logs to the review platform. In this case, which of the given difficulties occurs?*

> Here we assume the team member is technically proficient and is following procedure. There's a hint in the word error. Which challenge covers things that might be error-prone?

<details>

  <summary>Spoiler warning: Answer</summary>
  
    process and archive

</details>

---

### Common Mistakes and Best Practices

In almost all situations within a professional environment it is good to be aware of and knowledgeable about best practice. While it is important to learn from one's own mistakes a true master learns from the mistakes of others and doesn't make them themself in the first place *ancient Japanese proverb*.

*Q1: As a consultant, you are doing a comprehensive risk assessment and noticed that one of the development teams implemented a custom script to generate logs for an old system which omits loggings at some phases. What would you call this? (Mistake or Practice?)*

> It's not directly obvious which this is. If the omissions were planned then it may not be a problem to omit some phases from the logs. However the use of the word 'omit' rather than 'exclude' makes it sound like this is not the intended outcome. You could also just count the number of letters the answer requires.

<details>

  <summary>Spoiler warning: Answer</summary>
  
    mistake

</details>

---

### Conclusion

I thought this room might be a bit light on substance due to no VM or separate activity. After completing it I found it's changed how I think about logging, why it's done, how it's done and how it might be improved to address different goals.
