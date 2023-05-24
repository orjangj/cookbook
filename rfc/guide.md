# A Guide to Writing RFCs

> Disclaimer: This guide represents my personal preference on writing technical design documents or
> RFCs.

> TL;DR: Use the following [template](https://github.com/orjangj/cookbook/) to start writing your
> RFCs.

## Accronyms

- TDR - Technical Design Review
- RFC - Request for Comment

## Introduction

During the lifetime of an embedded project, developers will be faced with having to make design
decisions for a particular feature or problem that arises during development. At first, a developer
might already know how to solve the problem. However, when the feature or problem affects multiple
stakeholders, an RFC should be used to drive clarity and consensus. An RFC, if used correctly, can
be a great tool for a team of developers to systematically and critically form concrete ideas of how
to solve a problem, and then go for the solution that best fits the goals and requirements.

Usually, we know RFCs as a tool used by [IETF](https://www.ietf.org/) to write technical
specifications. In this scope however, RFCs is used in a broader sense. You can view it as a
lightweight technical design document that is primarily used as a tool for driving decision-making
among multiple collaborators and stakeholders.

## The purpose of RFCs

The process for proposing a set of solutions to a problem is the same whether you’re a junior or
senior developer, or even a company founder. You have the opportunity to present different design
options directly to your colleagues rather than relying on word-of-mouth. In many cases, the use of
RFCs can be a deciding factor in making timely and well thought-through decisions. Writing RFCs
provides context and background to the problem that must be solved, and more importantly - it
encourages critical thinking and analysis of the proposed options. The RFC doesn't have to be just
about software or hardware features. It could be "how to write design solution documents" where the
end-result of the RFC could end up as guidelines and rules for how developers should write software
design documents.

It's important to understand that RFCs is not a silver bullet, and will not always work for every
team or project. In general however, a team that leverages RFCs to drive decision-making and
development can achieve the following benefits:

- Inter-team/org communication.
- On-boarding new developers/engineers. Due to the format and process of doing RFCs, it's easier for
  new participants to follow the thought-process of why a particular solution was chosen.
- Productive contribution. RFC reviews make it easier for a whole range of developers to contribute
  towards a final design.
- After an RFC has been approved, you have already done most of the work of creating design
  documentation.
- An RFC can in some sense be viewed as a "contract" between the RFC owner and the review
  participants (stakeholders). If an RFC was approved, it's harder for participants to complain
  about the solution at a later time.

What about the downsides?

## Downsides of RFCs

Creating RFCs takes time to write, to review and to discuss, and may clash with other processes
enforced by your team or company. It's evident that doing RFCs come at a cost, so you should be
mindful of when to invest in writing RFCs. Sometimes the RFC owner and reviewers are going in
circles because of feedback, and it's hard to get closure. Or an RFC might be tightly coupled to
other parts of software or hardware that is outside the responsibility of the RFC owner. This means
that changes to those other parts, may yield unexpected problems during writing of the RFC or
shortly thereafter. Even worse, it may even make the RFC obsolete, thus wasting precious time. For
agile-paced teams, this is not an unrealistic situation. Although RFCs helps in getting clarity of
the problem at hand and consensus on the design solution, we may actually do things differently when
we start implementing the feature. A reason for doing so might be because we clash with some
constraint that we didn’t find before.

## Guidelines for Writing an RFC

Time is precious, so it's not always clear that RFCs is the right tool for the job. To help mitigate
ambiguities and wasting time, the team (or company) should have some preliminary idea of "When to do
RFCs?" before writing RFCs:

- All the time? Even for smaller problems? When do we require a feature to undergo RFC?
- How much time should be invested in doing RFCs? An hour? A week?
- Who reviews the RFC? Everyone? Managers? Developers with expertise?
- How long to wait for feedback? What is acceptable?
- Do all participants need to approve before marking the RFC as "Accepted"?

### Scope of the RFC

In general, I think RFCs should be short and to the point. It should be about a single feature or
problem that needs to be solved (no more, no less). It should be manageable enough for a single
developer (owner of the document) to write the RFC. Sometimes however, specially in a
multidisciplinary team, one person may not have all the required context. In such cases it could be
beneficial to include one or more contributors during the writing of the RFC. Or people from several
roles could write their own RFCs based on their responsibilities to cover all relevant aspects of
the feature/problem to solve.

### RFC Owner Responsibilities

Avoid subjective words such as “I/We think”, “I/We feel”, “Good/Great/Nice”, “Bad/Terrible”. The RFC
should use objective language to present the problem, and its possible design solutions. Subjective
opinions can wait until after the actual review process has started.

Before the document is presented for review, you should ensure that the goals/requirements of the
RFC and the proposed design options have been considered carefully. You should also have a
preliminary conclusion on what to go for based on the evidence and facts presented. The initial
conclusion might be skewed by reviewer feedback, so making the final decision should be done
together with the reviewer participants (e.g. by votes/approvals).

### Review Participant Responsibilties

See whether assumptions are being made without evidence. If yes, prompt the RFC owner to do more
research and add more evidence. You should critique every statement and give constructive feedback.
Ask questions if something is missing or needs more clarification. Give vote/approval on what option
to choose.

### Picking Reviewers

The RFC should include all stakeholders who are affected by the feature or problem at hand.

As an example (although not an exhaustive one), it could be any of the following:

- Team Manager (or teammates)
- Product Manager
- Test Engineers
- Dependent teams (e.g. due to downstream/upstream dependencies on other SW component interfaces)

Additionally, I would recommend adding the more experienced developers (more senior than you are).
Being challenged by a more experienced developer should be treated as an opportunity to grow, and
they may even ask questions that is important enough to make a big difference in the design.

### Picking Design Options

Pick the most 3-4 reasonable options to consider. Each option with an exhaustive list of pros and
cons. Preferably, each pros and cons should use goals/requirements as a measure to see if the option
is good or bad in that dimension. But listing pros and cons that cannot be measured by goals are
still important if it is relevant to the design decision (e.g. if cost is not part of the goal, but
option 1 is free while option 2 is not free).

## Document Structure

From a Bird's-eye view, an RFC should include some, if not all, of the following key elements:

1. Title
2. A table containing vital information about the RFC (such as owner, list of review participants,
   deadline or target release for the RFC, and status of the RFC)
3. Acronyms/Abbreviations
4. Background
5. Goals/Requirements
6. Out-of-Scope
7. Design options
8. Conclusion
9. Action Points
10. References

### Title

TODO

### Table overview

The RFC should begin with a brief overview - a table - that describes the feature or problem to be
solved.

|                             |                                                                                                             |
| --------------------------- | ----------------------------------------------------------------------------------------------------------- |
| **Problem statement**       | Short description of the problem to solve                                                                   |
| **Owner**                   | The one responsible for the design                                                                          |
| **Contributors**            | List of other contributors (if any)                                                                         |
| **Created**                 | dd.mm.yyyy                                                                                                  |
| **Deadline**                | dd.mm.yyyy                                                                                                  |
| **Target release**          | x.y.z                                                                                                       |
| **Review participants**     | List of stakeholders who would be affected by the design, such as managers, teammates, test engineers, etc. |
| **Review Status**           | Blocked \| Work in progress \| In-review \| Approved \| Obsolete                                            |
| **Relates to**              | Link to other design review documents or milestones/stories/tasks                                           |
| **Depends on / Blocked by** | Link to other design review documents or milestones/stories/tasks                                           |

### Background

TODO

### Goals

TODO

### Out-of-Scope

TODO

### Design options

TODO

### Conclusion

TODO

### Action Points

TODO

### References

TODO

## References

- [Strengthening Products and Teams with Technical Design Reviews](https://medium.com/git-out-the-vote/strengthening-products-and-teams-with-technical-design-reviews-ae6a1bec5216)
- [A Practical Guide to Writing a Software Technical Design Document](https://medium.com/swlh/a-practical-guide-to-writing-a-software-technical-design-document-c6f4d865ccff).
- [RFC Driven Development](https://medium.com/@donbonifacio/rfc-driven-development-dc9695f9e622)
- [RFCs: Lightweight Technical Designs](https://medium.com/caspertechteam/rfcs-lightweight-technical-designs-a508d93ccd34)
