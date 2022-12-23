---
layout: page
title: red cross screening list
description: Jan 2022
importance: 2
img: assets/img/rc.png
category: school/work
---

<h3>Background</h3>
The Red Cross is a large, distributed organization with thousands of people - and therefore also has many people applying to join the organization. Like many large companies, the organization has a screening process to ensure that individual departments/managers only need to interview candidates which are fully committed and qualified. 

Every Monday, Wednesday, and Friday, an assignment sheet is created and sent to the screening team (around 15 screeners). However, the process of creating this assignment sheet is not trivial - for a number of reasons, the list of applicants can't simply be divided evenly among the screeners. 

<h3>Overview</h3>

The process of filtering and assigning the applicants to screeners used to be a lengthy, tedious process for the following reasons:

1. The full list contains everyone who has applied for a role but has not been referred or rejected. 
  - There are applicants have already been assigned to a screener - they need to be assigned to the same one.
2. A small amount of applicants have unique situations.
  - For the different unique situations, there exist different subsets of screeners qualified to handle these scenarios. These applicants and screeners should be matched accordingly. 
3. Many screeners have preferences on workload.
  - The screeners are volunteers, so any limitations on total/new applicants they are assigned each day should be honored.

There were additional nuances to the process, although they were more straightforward to deal with (ex. if value == x, do this). Designing an algorithm to achieve the goals above was the main challenge.

<h3>Algorithm Summary</h3>

1. First, we will simply copy over existing assignments. If a screener was matched with a prospective volunteer who applies for another role, they will also be in charge of screening them for the new role - it makes sense to have the prospective volunteer work with the same screener, even if it is for multiple roles.

2. Next, we will assign the prospective volunteers that have unique situations. Each unique situation has a different "queue" of screeners that assigns to volunteers with that specific situation. The queue is updated after each pass through and will remove screeners if they have reached their preferred workload (number of volunteers they were assigned).

3. Finally, we do the same thing for prospective volunteers that can be assigned to any screener, using one big queue of all the screeners. Again, the queue is updated after each pass through to ensure screener preferences are respected.

<h3>Links</h3>
This is code which has been slightly reconfigured as an easily deployable web app (using Heroku and Mercury) so that it is easy for users without a programming background to use the tool.

[Source Code](https://github.com/jasmine2000/screener-list-creator)
