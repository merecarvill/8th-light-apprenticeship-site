---
layout: post
section-type: post
title:  Pairing Tour Day 8
category: learning
tags:
- learning
---
I paired with Dave today. We've paired a few times before - during the phase of my apprenticeship where I helped with client work.

It was a nice change of pace to work with someone who I've worked with before, and to work in a context that I'm already somewhat familiar with.

I'd never worked within the particular app that we worked on today though. Our work focused on replacing a section of a form view that accepted an e-signature from the user. The client had extracted the e-signature logic from another app into its own module, and we attempted to integrate the module into this form.

We also participated in an iteration planning meeting for about three hours in the morning. The work was challenging, but we made good progress through the day.

Most of the coding work we did during the remaining time in the morning was dedicated to resolving several updates to the app's dependencies in order to get the e-signature module working. By the end of the afternoon, we had everything working with the new module and had begun removing code made obsolete by our changes.

A significant amount of refactoring remained that we wanted to do. Most significantly, the form view was rather large and complex and Dave and I thought the design would be improved by separating it into two distinct classes.

The rough idea was that one would handle rendering the form and performing basic validation on the form's fields, and the other would handle the e-signature logic and the network requests necessary for verifying the e-signature and posting form data.

All in all, it was the most continuous coding I'd done during my pairing tour - at least this week - and I am proud to feel like I contributed consistently to the work throughout the day. 
