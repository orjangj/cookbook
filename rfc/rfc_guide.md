A Guide to Writing Request for Comment's (RFCs)
===============================================

This article has been inspired by mutiple great sources on writing Request for Comments (RFCs):
- [A Practical Guide to Writing a Software Technical Design Document](https://medium.com/swlh/a-practical-guide-to-writing-a-software-technical-design-document-c6f4d865ccff).
- [RFC Driven Development](https://medium.com/@donbonifacio/rfc-driven-development-dc9695f9e622)

> Disclaimer: This guide represent my personal preference on writing technical design documents.

> TL;DR: Use the following [template](https://github.com/orjangj/cookbook/) to start writing your RFCs.

The purpose of RFCs
-------------------

Usually, we know RFCs as a tool used by [IETF](https://www.ietf.org/) to write technical specifications. In this document however, RFCs is used in a broader sense. You can view it as a lightweight lightweight technical design document, primarely used as a tool for driving ... decision making... reaching concencus between stakeholders.

During the lifetime of a software project, developers will be faced with having to make design decisions for a particular feature or problem that arises during software development. ....At first, a developer's design solution may surface ....  However, when feature or problem at hand affects multiple stakeholders, an RFC should be used to drive clarity and concensus. An RFC, if used correctly, can be a great tool for a team of developers to systematically and critically form concrete ideas of how to solve a problem.

The process for proposing a change in writing is the same whether you’re a junior employee or a company founder, and you’ll get to present it directly to your colleagues rather than relying on word-of-mouth.

In my experience as an embedded software developer, the use of RFCs has been a deciding factor on numerous occasions in making timely and well thought-through decisions. This is due to the ... critical thinking and ... analysis of proposed options.

After an RFC has been approved, you have already done most of the work of creating design documentation.

The RFC doesn't have to be just about software features. It could be "how to write design solution documents" where the end-result of the RFC could end up as guidelines and rules for how developers should write software design documents. Or it could be about something that falls into the category of "what tools to use for development".

It's important to understand that RFCs is not a silver bullet, and will not always work with all teams.

It provides a common ground for collaboration between "owner" and stakeholders... and is a tool for communicating intent and ....

Downsides of RFCs
-----------------

Creating RFCs takes time and may clash with other processes enforced by your team or company. There's also the questions of "When to do an RFC? All the time? Even for smaller problems?", "How much time should be invested in doing an RFC? An hour? A week?", or "Who reviews the RFC? All the team? Managers? How long to wait for feedback, and do all need to approve the design solution for the RFC to be accepted?". Time is precious, so it's not always clear that RFCs is the right tool for the job. Sometimes the RFC owner and reviewers are going in circles because of feedback, and it's hard to get closure.

Sometimes, an RFC might be tightly coupled to other parts of software that is outside the responsibility of the RFC owner. This means that changes to those other parts, may yield unexpected problems during writing of the RFC or shortly thereafter. Even worse, it may even make the RFC obsolete, thus wasting precious time. For agile-paced teams, this is not an unrealistic situation.

Another issue is that although the RFC helps in getting clarity of the problem at hand and concensus on the design solution, we may actually do things differently when we start implementing the feature. A reason for doing so might be because we clash with some constraint that we didn’t find before.

All of these questions and issues naturally depends on the scope of the task to accomplish.

Guidelines for Writing an RFC
-----------------------------

With having listed both pros and cons of using RFCs, I think it's a about time we dig into some guidelines and recommendations to help mitigate RFCs ending up as a bootleneck for productive software development.

**Scope of the RFC**

In general, I think RFCs should be short and to the point. It should be about a single feature or problem that needs to be solved (no more, no less). It should be manageable enough for a single developer (owner of the document) to write the RFC.Sometimes however, specially in a multidisciplinary team, one person may not have all the required context. In such cases it could be beneficial to include one or more contributors during the writing of the RFC. Or people from several roles could write their own RFCs based on their responsibilities to cover all relevant aspects of the feature/problem to solve.

**RFC Owner Responsibilities**

Avoid subjective words such as “I/We think”, “I/We feel”, “Good/Great/Nice”, “Bad/Terrible”. The RFC should use objective language to present the problem and it's possible design solutions. Subjective opinions can wait until after the actual review process has started.

Before the document is presented for review, you should ensure that the goals/requirements of the RFC and the proposed design options have been considered carefully. You should also have a preliminary conclusion on what to go for based on the evidence and facts presented. The initial conclusion might be skewed by reviewer feedback, so making the final decision should be done together with the reviewer participants (e.g. by votes/approvals).

**Review Participant Responsibilties**

See whether assumptions are being made without evidence. If yes, prompt the RFC owner to do more research and add more evidence. You should critique every statement and give constructive feedback. Ask questions if something is missing or needs more clarification. Give vote/approval on what option to choose.

**Picking Reviewers**

The RFC should include all stakeholders who are affected by the feature or problem at hand.

As an example (although not an exhaustive one), it could be any of the following:
- Team Manager (or teammates)
- Product Manager
- Test Engineers
- Dependent teams (e.g. due to downstream/upstream dependencies on other SW component interfaces)

Additionally, I would recommend adding the more experienced developers (more senior than you are). Being challenged by a more experienced developer should be treated as an opportunity to grow, and they may even ask questions that is important enough to make a big difference in the design.

**Picking Design Options**

Pick the most 3-4 reasonable options to consider. Each option with an exhaustive list of pros and cons. Preferably, each pros and cons should use goals/requirements as a measure to see if the option is good or bad in that dimension. But listing pros and cons that cannot be measured by goals are still important if it is relevant to the design decision (e.g. if cost is not part of the goal, but option 1 is free while option 2 is not free).

Document Structure
------------------

From a Bird's-eye view, an RFC should include some, if not all, of the following key elements:

1. Title
2. A table containing vital information about the RFC (such as owner, list of review participants, deadline or target release for the RFC, and status of the RFC)
3. Accronyms/Abbreviations
3. Background
4. Goals/Requirements
5. Out-of-Scope
6. Design options
7. Conclusion
8. Action Points
9. References

**Title**

**Table overview**

The RFC should begin with a brief overveiw - a table - that describes the feature or problem to be solved.

| | |
|-|-|
| **Problem statement** | Short description of the problem to solve |
| **Owner** | The one responsible for the design |
| **Contributors** | List of other contributors (if any)|
| **Created** | dd.mm.yyyy |
| **Deadline** | dd.mm.yyyy |
| **Target release** | x.y.z |
| **Review participants** | List of stakeholders who would be affected by the design, such as managers, teammates, test engineers, etc. |
| **Review Status** | Blocked \| Work in progress \| In-review \| Approved \| Obsolete |
| **Relates to** | Link to other design review documents or youtrack milestones/stories/tasks |
| **Depends on / Blocked by** | Link to other design review documents or youtrack milestones/stories/tasks |

**Background**
**Goals**
**Out-of-Scope**
**Design options**
**Conclusion**
**Action Points**
**References**
