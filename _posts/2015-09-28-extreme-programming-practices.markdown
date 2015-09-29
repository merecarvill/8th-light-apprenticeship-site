---
layout: post
title:  Extreme Programming practices
date:   2015-09-28
categories: agile extreme-programming
---
I receieved a deeper introduction to [Agile](https:**//en.wikipedia.org/wiki/Agile_software_development) software development over the weekend, thanks to Kent Beck's *[Extreme Programming Explained](http://www.amazon.com/Extreme-Programming-Explained-Embrace-Edition/dp/0321278658)*. In particular, the description of XP's component practices has helped me to establish a more concrete understanding of what it means to create software in an agile way. I thought I'd review the practices listed in the book to consolidate my learning and to hopefully provide a useful resource to others.

My understanding of agile methodologies up to this point has felt fairly nebulous. This is not surprising, since I have hardly any experience of my own that I can use to ground my understanding. This is why I was particularly pleased to read Beck's section on XP practices.

The values and principles he enumerates are interesting and certainly important - I'm inclined to believe him when he says:

> "Practices by themselves are barren. Unless given purpose by values, they become rote."

While the values of XP lend meaning to its practices, however, the values themselves fall rather flat without menion of practices that demonstrate what a given value actually entails. For example, most methodologies would likely claim "communication" is a top-priority, but XP diverges from many other methodologies in what advises you do in order to align yourself with such values *in practice*.

In my view, then, the "extreme" aspect of XP lies not the relatively uncontroversial and widely-shared values, but in the practices that Beck advocates in support of those values.

# The practices

Beck distinguishes between "primary" and "corollary" practices - the practices that generally yeild tangible benefits even when implemented in isolation, versus the practices that may not be of benefit unless implemented with the support of other XP practices.

### Primary Practices

- **Sit Together:** Work together in an open, shared space rather than relying on cubicles, offices, or remote work. This hinges on the idea that "physical proximity enhances communication."
- **Whole Team:** Have a team that incorporates all the skills necessary to bring the project from start to completion - i.e. a cross-functional team. Have each member fully dedicated to the team, rather than splitting their efforts between multiple teams.
- **Informative Workspace:** Important information should be immediately available and easily understood in the workspace. Charts and notecards arranged on the walls is one suggested way of achieving this.
- **Energized Work:** Work only as much and as often as you are productive and is indefinitely sustainable. This is based on the idea that more time spent working != greater productivity - and can, in fact, harm productivity beyond a certain point.
- **Pair Programming:** Write all production code in pairs - two programmers, one computer, one problem, crafting a solution together.
- **Stories:** "Plan using units of customer-visible functionality." You know when you've finished a discrete unit of work when the product performs a new function as requested by the customer.
- **Weekly Cycle:** Conduct work according to a process that is reiterated each week - allowing frequent opportunities to act on feedback and make decisions based on up-to-date information.
- **Quarterly Cycle:** "Plan work a quarter at a time." Basically, have longer intervals set aside for feedback that is larger-scale and longer-term, and make associated decisions then.
- **Slack:** Structure work so that some amount is invested in non-essential (but beneficial) activities. In an emergency, that work can be temporarily marshalled towards a remedy.
- **Ten-Minute Build:** Maintain the ability to automatically build and test the entire project within ten minutes.
- **Continuous Integration:** Integrate and test changes frequently - every few hours.
- **Test-First Programming:** Write a failing test before adding to production code. Add only so much code as it takes to get the test to pass.
- **Incremental Design:** Alter the design of the project whenever necessary in order to fit the needs of the system *as it is now*. Defer investing in any design decision until the "last responsible moment."

### Corollary Practices

- **Real Customer Involvement:** "Make people whose lives and business are affected by the system part of the team." Have them guide the development of the system. This is in the same vein as the practice of creating a "whole team."
- **Incremental Deployment:** When replacing an existing system with a new one, deploy the new system incrementally, as new functionality is added. Maintain both systems in parallel until the new system can take over.
- **Team Continuity:** "Keep effective teams together." Don't needlessly break them apart after the completion of a project.
- **Shrinking Teams:** As a team grows more productive, keep the team's workload constant. Focus on improving until a member is no longer needed on the team and can be removed.
- **Root Cause Analysis:** When a defect is discovered, eliminate the root cause in addition to the defect.
- **Shared Code:** Allow team member to improve any part of the system's code.
- **Code and Tests:** Consider the code and associated tests as the only enduring elements of the project. Use the code and tests as the sole source for generating other documents.
- **Single Code Base:** Maintain only one version of the code - all altered versions must be reconciled with the authoritative version within a few hours of their existence.
- **Daily Deployment:** Put new code into production on a daily basis.
- **Negotiated Scope Contract:** Write contracts that dictate terms for a continuous negotiation of project scope, rather than a contracts that determines it up-front. Negotiate a series of smaller contracts that address sequential portions of a project, as opposed to a single, comprehensive contract.
- **Pay-per-use:** Charge every time the system is used, as opposed to profit models that are not directly coupled to (and indicative of) usage.

# Thoughts

The above is my best attempt at distilling the practices employed in XP. In a subsequent post, I plan to dig a little deeper into an analysis and critique of these practices.