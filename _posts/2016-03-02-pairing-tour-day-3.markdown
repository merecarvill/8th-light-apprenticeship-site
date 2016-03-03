---
layout: post
section-type: post
title:  Pairing Tour Day 3
category: learning
tags:
- learning
---
I paired with Ben today. It was a scattered day - lots of things seemed to be going wrong in staging and CI was failing pretty much the entire day.

As far as I can tell, it was a storm of small errors. For example, early in the day Ben had pushed a migration to the project database. It worked fine locally, but because the table of migrations in staging apparently capped the names of its entries at 63 characters (as opposed to 255 on local), migrations blew up and nothing worked.

Lacking any prior knowledge about the project, most of this stuff went over my head. I didn't feel terribly useful for much of the day, as we spent a good bit of it stepping through logs from failing builds.

At the end of the day, I was able to write a bit of code. Ben basically handed the reigns to me after grabbing the story. Of anyone I've paired with so far, he was the most willing to just leave me to my own devices and see what I could figure out on my own, which was fun and a bit nerve-wracking.

The task was to load some data from the database and store it as the property of an object rather than hitting the database each time the data was needed. I was able to get pretty far by imitating the code for a similar property on the same object.

We also spent a chunk of time reviewing PRs by other members of the team. That was interesting work, though I didn't have much input that was distinct from the feedback that Ben came up with on his own regarding the code we looked at.

It's been good to see more of what is involved in developing software as a team, and the varying workflows between different clients and projects.
