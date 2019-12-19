# Technical Design Process

Copyright SurveyMonkey Inc., 2019
This work is licensed under the Creative Commons Attribution 4.0 License (CC-BY-4.0). A copy of the license may be found at https://creativecommons.org/licenses/by/4.0/legalcode or in the LICENSE file accompanying this document.



# The problem

As discussed in [Appendix A](#heading=h.88sxb7lelwsv), there are two broad categories of technical implementation: organic, evolved change and designed change. To scale as a company, we must move from a predominantly organic approach to a designed approach.

Traditional design processes usually review only after completion of the full, detailed technical design document. This typically renders the review process moot since so much work has already been sunk into the project and, often, an actual implementation was built alongside the document itself. 

The primary purpose of documented, reviewed designs is to improve the implementation and sustainability of our complex systems. They also serve critical secondary purposes:

* Educating teams

* Developing trust between engineers

* Facilitating healthy communication

* Demarcating responsibilities

* Preserving nuance and context in historical documents for future discussions

 

While our current processes help with some of this, they fall short by postponing review and leaving many of the steps unstructured.

# Goals

* **The process will be clear.** None of this matters if there’s a maze to navigate to get from point A to point B.

* **The process will be relatively lightweight.** Design is intensive by nature but it doesn’t have to be onerous. Frontloading a deep, technical design document puts a mountain between conception and execution.

* **Reviews will happen at critical points and account for the work already done.** It’s unfair to ask someone to specify everything about a project from API structure to scaling model to monitoring strategy to transition plan--only to be told in a review that the root approach is faulty. For a process to work, reviews must be scheduled at major decision points.

* **Responsibilities will be clear.** Without clear responsibility for decision-making, reviewers and reviewees may be operating at cross-purposes. It’s not helpful if the reviewee believes they must achieve consensus while a reviewer believes it’s their responsibility to play devil’s advocate.

* **The process will leave a historical record.** Future generations of engineers who wonder why a decision was made, whether we ever attempted to solve a problem, or whether a specific technology was ever evaluated should be able to review the archives and learn.

* **The process will allow for personal judgment. **Different engineers work differently. Some design best by prototyping in parallel while others need to write things out before they touch the code. Some need to hear every viewpoint while others have to touch things themselves to evaluate them. This process respects those differences and allows for them within bounds.

* **The process will encourage ownership.** Consensus struggles to deal with both organizational scale and the disagreements that inevitably arise between well-intentioned engineers. Clearly delegated decision-making ensures that everyone involved understands who’s ultimately responsible for making a call and owning its outcome.

* **The process will lead to better results.** Processes are by us and for us. They should be followed in good faith and changed if they don’t produce the results we want.

# Roles

For this process to work, it must be clear who is responsible for what. This list of roles is the *who*. The *what* will be touched on throughout the process documentation.

<table>
  <tr>
    <td>Role</td>
    <td>Notes</td>
  </tr>
  <tr>
    <td>Designer</td>
    <td>This is the person embarking upon the design process. If there are multiple people collaborating on a design, a single person should still be named as the designer to ensure there’s no conflicting messages. This person is entirely responsible for the document’s content.</td>
  </tr>
  <tr>
    <td>Owners</td>
    <td>This is the team responsible for the finished product should the design be accepted and implemented. It should be rare for the designer to not be a member of this team.

… if a design spans multiple teams:
The owner will be determined by the nearest common manager.

… if the owner is unclear:
It will be determined by the director or VP who would be held responsible should the design be implemented poorly.

… if the owner is still unclear:
It’s probably you. It will be determined by the designer and their management chain in cooperation with the new owners.</td>
  </tr>
  <tr>
    <td>Architect(s)</td>
    <td>The Architect responsible for the area under discussion. If the design spans multiple areas, each affected Architect plays a part.</td>
  </tr>
  <tr>
    <td>Reviewers</td>
    <td>The panel assembled to offer feedback on a particular part of the design. Note that this includes but is not exclusive to the Owners and Architects. Reviewer selection will be discussed later.</td>
  </tr>
  <tr>
    <td>Moderator</td>
    <td>The person responsible for keeping the review discussion on track. Ideally a project manager, but this role may be filled by the Designer’s immediate manager.</td>
  </tr>
</table>


# The process

## I. Define the problem

**_Deliverable: _***a clear, unambiguous statement of the problem that needs to be solved. This will be the first section of the Solution Proposal.*

The most essential step in this entire process is the first: define the problem that needs to be solved. A perfect design for a nonexistent problem or a problem that doesn’t need to be solved is simply wasted effort. Unfortunately, this is also the most difficult step.

### Characteristics of a problem statement

#### Written for your audience

Your audience is technical. They’re your peers, your manager, the architects, and your future self. The problem statement is crafted for them. Unless it’s a core part of your argument, there’s no need to justify decisions that have already been made or to put your case in terms of business value.

#### Clear, concise, and complete

You should be able to hand the problem statement to another engineer and get in return a set of proposals that match your intentions. Yet the problem statement is also concise; it doesn’t editorialize or hyperbolize and it doesn’t bother with history your audience doesn’t need.

#### Describes *why* it needs to be solved

This is typically the best place to start as the *why* must inform the *what*. Finding the right *why* will take time but the answer to that question should help your audience gauge the problem’s importance and urgency for themselves. An effective *why* will get them concerned, worried, or excited. It will demand action.

If you’re struggling to find the right balance for expressing a problem statement, consider working with your manager or a neighboring tech lead to discuss why they should care about the problem. The [5 Whys Technique](https://en.wikipedia.org/wiki/5_Whys) is also a useful model.

Finally, there will be times when the *why *is arbitrary: if PCI compliance demands that antivirus software run on all hosts under its domain, so be it. You needn’t spend time justifying the rule itself beyond citing the compliance requirement and noting that we run a PCI environment.

#### Describes *what* needs to be solved

The hardest part of expressing the *what* of a problem is knowing *which* problem to solve. This is a critical judgement call.

One effective approach is to list a handful of candidates and then break down the assumptions behind each of them. Running systems are messy, especially when distributed, so the best problem to solve will typically have few optimistic assumptions about the world. The optimizations in [Appendix B](#heading=h.v4mmh7sjtcry) may also be helpful reality checks.

For example, consider a site outage caused by loss of the link between our US and EU datacenters after a submarine volcano erupts and severs a primary trans-Atlantic fiber link. Which problem needs to be solved?

* *The US-EU link was lost.*

* *There is no US-EU link redundancy.*

* *Submarine fiber bundles have a lower melting point than the earth’s crust.*

* *Loss of an inter-datacenter link causes a site outage. *← it’s this one

If you find yourself struggling, it may be helpful to return to the *why*. A poor answer there can make it nearly impossible to find the right problem to solve.

#### Does not imply an answer

Virtually no technical problem is open to only a single solution. A good problem statement is objective, no matter how strongly you feel about your proposed solution.

If you’re unable to separate the problem from your proposed solution, consider outsourcing this part of the process. A dispassionate expression of the problem will help expose any blind spots and ensure that you’re making a good, persuasive decision.

## II. Solution Proposal document

**_Deliverable: _***a completed proposal based on the **[Solution Proposal Templat*e](https://docs.google.com/document/d/1P8upq4Y0kstBXwKl0lBmL_g49ciouQkbJMu-VNVIXrs/edit?usp=sharing)*.*

Complete a Solution Proposal from the [Solution Proposal Template](https://docs.google.com/document/d/1P8upq4Y0kstBXwKl0lBmL_g49ciouQkbJMu-VNVIXrs/edit?usp=sharing), using your finished Problem Statement for the first section. The template is self-explanatory, but in broad strokes its sections are:

* **Problem Statement. **You’ve already finished this.

* **Proposed Solution. **A high-level, 1-3 page proposal for how you believe the problem should be solved. This is not a fleshed-out design document but should include enough information that another engineer could design it effectively.

* **Alternatives.** Two or more alternatives to your proposal. These should be viable alternatives described in enough depth that you feel confident a reviewer unfamiliar with this space would understand the implementation. Furthermore, each alternative should be accompanied by an explanation of how it’s superior to the proposed solution and why you decided against it.

Once the Solution Proposal is complete, have at least one other person read it over before moving to Review. This reader is looking for clarity as well as basic flaws that might derail Review before it begins.

## III. Review the proposal

**_Deliverable: _***a decision on how to move forward and an updated Solution Proposal marked as either Accepted or Rejected accordingly.*

The purpose of review is to make the best decision possible.

There are three possible results of a review session:

* **Accepted.** The Designer exits the review feeling confident in their proposal. Once any necessary changes are made, it is marked as Accepted and the Designer may move on to the next step when ready.

* **Rejected.** The Designer exits the review feeling their proposed solution should not be pursued as-is. The document is marked as Rejected with an explanation and the Designer chooses how to proceed depending on the circumstances. Reasons for rejecting a proposal include but are not limited to:

    * The current proposal is incomplete.

    * The current proposal has a fatal flaw.

    * An alternative is superior to the current proposal.

    * The problem statement is inaccurate.

    * The problem statement is accurate but simply does not need to be solved.

* **Vetoed.**  While the Designer remains in favor of the proposal, a party with veto power believes strongly that moving forward would cause significant damage to the organization. The proposal is marked as Vetoed with a clear explanation of why. The Designer may seek to propose a new solution (in which case the existing proposal is moved to an Alternative section) or to let the problem lie.

### For the Designer

#### 1. Take a moment

The whole purpose of this process is to come to the best answer for the org and the company at large. So take a moment and review your proposal.

* Is it complete?

* Do you fully understand what you’re suggesting?

* What are the gaps reviewers are likely to bring up? Are you comfortable with them or do they warrant revision of your decision?

* Does this problem need to be solved?

* Does the solution presented really solve the problem?

* If your proposal were vetoed, how would you feel? Are you sure it’s the best solution and not a pet solution?

* What’s your position in this problem? Are you its owner, an affected consumer, an affected neighbor?

* Who are you asking to do what? If you’re asking someone else to do ongoing work, what’s the justification? How would you react if you were them? If you’re asking people to change how they do something, what do they lose? What do they gain?

Revise as often and as long as needed before moving forward.

#### 2. Select your Reviewers

This should be 3-6 people. Feel free to solicit feedback from others, but discussions scale poorly with the number of participants. Your committee must include:

* Your immediate manager.

* At least one representative of the Owners, even if you’re a member of that team.

* At least one other technical voice who’s neither on your team nor the Owners.

* *Depending on the problem/solution: *a representative from Security Policy

* *Depending on the scale of the problem*: the relevant Architect(s).

Use judgment when deciding whether to include the Architects. They maintain veto power even if not invited, so ask if you’re in doubt.

Likewise, Security Policy will need to be involved if your proposal affects the current security posture. While Security does not have veto power, they are responsible for both articulating policy and holding teams accountable for their implementation of that policy.

#### 3. Select your Moderator

The Moderator is responsible for keeping the review discussion on track. This is not a technical reviewer role.

Ideally, a project manager performs this duty. If no PM is available, look for a third-party engineering manager. As a last resort, your immediate manager may wear this hat. Doing so puts them in a tough position since they must balance offering their own perspective with performing neutral moderation duties.

#### 4. Send out your Solution Proposal

Invite your entire panel to read and comment on your Solution Proposal document.

This is a time for clarifying questions. Update your document as necessary to ensure that everyone is on the same page before entering the discussion. If you realize the document is fatally flawed or incomplete, cancel or postpone the review.

If reviewers begin to dive into review commentary as opposed to asking for clarification, simply resolve the comments with the reply: "Please save for review discussion."

#### 5. Schedule your review session

Schedule a one-hour review session, ensuring that you’ve left sufficient time for reviewers to read your Solution Proposal. In most cases, 24-48 hours is enough.

#### 6a. Session: present the proposal

The goal of your presentation is to ensure that everyone fully understands what’s being proposed and the decision-making process that went into it. Before moving to discussion, validate that the entire room is on the same page.

#### 6b. Session: listen and answer questions

Open it up for discussion. *Listen and answer questions*.

Remember that you own the decision at the end of this review. Your job during the discussion is not to persuade, but to make damned sure you make the best possible decision. Listen to the feedback, answer questions, ask for clarification when needed, and consider what’s being suggested. When people offer better ideas, incorporate them.

This discussion is about the problem and how to solve it. While it can be critically hard sometimes, it’s imperative to listen without without taking disagreement personally.

#### 6c. Session: decide

As the review session draws to a close, it’s time to make a decision. Document all decisions in the Solution Proposal itself.

As the Designer, you announce your intention first:

* **Accept it.** You intend to move forward with the Solution Proposal with minimal (or very well-understood) revision. Note that this may be the case for solutions that ought to be delayed indefinitely.

* **Revise it.** You feel the Solution Proposal needs more work and intend to revise it. This may be as small as swapping the proposed solution out for an alternative or as large as completely reframing the problem.

* **Abandon it**.** **You feel the Solution Proposal needs more work but do not intend to revise it. Perhaps you learned that there’s another ongoing effort to solve this problem, you were persuaded that the problem isn’t what you thought, or you realized that you’re not the right person to tackle this particular problem.

The Veto-enabled parties are then given an opportunity to exercise their Vetoes.

#### 7. Close the loop

The outcome of the session should be noted in the Solution Proposal doc.

* **Accepted** proposals should be marked as such at the top. 

* **Revise** proposals should be changed up as necessary and then rescheduled for review.

    * If you end up revising your problem statement, use your judgment on whether to revise what you have or create a new document. If you create a new doc, add a note and a link both old and new.

    * If you end up changing you proposal, move the original underneath an "Alternative" heading and add a clear explanation of why you decided against it.

    * If the revisions are already largely agreed upon, it may be sufficient to just send the document back out for a re-read. Use your judgment.

* **Abandoned** proposals should be marked with a clear explanation for future generations. If another Designer elects to take up the problem, they should begin with a new document and add links to both. Future reviews should take the previous attempt into consideration.

* **Vetoed** proposals should be marked accordingly. Each party that issued a veto is responsible for adding a full accounting of why. Vetoes *may not be secret*. 

### For the Reviewers

#### 1. Answer the call

Reviewing designs is an invaluable contribution to the health of our organization and a great chance to learn, but it’s also a significant time commitment. If you feel unable to make that commitment, please decline the invitation as soon as possible to allow the Designer to make alternate plans.

If you’re the Designer’s immediate manager, your participation is required. Please work with the Designer to find a time that works for you. In extreme circumstances, discuss with your manager to ensure that *someone* from the Designer’s management chain is involved.

If you’re an Architect, you will simply not be able to attend every design you’re invited to. Focus on the highest leverage, highest stakes, and most controversial reviews. Bear in mind that you have veto privileges that may be exercised without attending a review; but never should be except in the most dire of circumstances.

#### 2. Understand the Solution Proposal

Read and understand the Solution Proposal ahead of time.

The goal of this reading is *only* comprehension. Confine your comments to clarifying questions about things that are unclear or missing, but ensure that you ask when you don’t understand something. The review session cannot proceed until everyone involved is on the same page.

Note strong reactions, concerns, and disagreements in a separate personal document for discussion during the review session. Useful questions to consider are:

* Is the problem statement accurate?

* Is the problem statement at the right depth?

* Does this problem need to be solved?

* Are the alternatives complete? Is there a high-quality alternative not considered?

* Do you agree with the relative evaluations of the alternatives and proposed solution?

* Are any there deep concerns with the proposed solution left unaccounted for?

#### 3a. Session: listen to the presentation

Again, the purpose of the presentation is to ensure the Designer and review panel are on common ground when the discussion begins. Listen and ask clarifying questions as necessary.

#### 3b. Session: discuss

Once the group has reached a common understanding, the Designer will open it up for discussion. This is your chance to debate, disagree, analyze, and raise concerns.

Remember that this is not a consensus process; your job is to ensure that all necessary information is accounted for during the decision-making process. By all means, advocate for a particular solution if you believe in it, but the *why* is far more important to the end-goal than the *what*.

Be mindful that it’s tough to be the Designer in a contentious process and to listen to critique of what you painstakingly drafted. It’s their responsibility to listen to and internalize this feedback and to remember that it’s not personal, but you can help them better hear you by delivering your points with empathy. Keep the conversation centered on the problem and solution as presented, not on the person or a presumptive process behind the document.

For example, "You didn’t deal with encryption" puts the listener on the defensive and implicitly demands that they either disagree or publicly admit failure. The same thing can be expressed in a way that allows the listener to agree to a mistake without challenging their ego: “This solution doesn’t touch on encryption.”

Finally, avoid getting caught up in detailed implementation plans. Only consider implementation where necessary to influence the overall solution proposal. The Technical Design document will break down implementation in depth.

#### 3c. Session: veto or not

At the close of the session, the Designer will announce their intent and offer a chance for vetoes. If you have veto power, now is the time to choose. Vetoes are discussed in more depth below.

#### 4. Close the loop

Depending on the result of the session, the Designer may ask for further written feedback or even another session. Work to see this particular problem through to the end. If you reach a point where you can no longer participate, let the Designer and their immediate manager know they need to find a suitable replacement.

### For the Moderator

The moderator’s job is straightforward: keep the review session on track.

* **Ensure understanding before discussion.** The entire review panel will be anxious to dive into the fun part: debate, discussion, and analysis. Your job is to gently but firmly keep the conversation focused on achieving general understanding first. Until everyone’s on the same page, conversation is not productive.

* **Help to divert away from implementation rabbit holes. **If you notice the conversation stalling on details that seem more appropriate for planning a detailed implementation than for deciding how to solve the problem at hand, ask the participants to do a self-check. Some details may need to be understood before a decision can be made. Most do not.

* **Call people on ineffective communication.** Help pause and recalibrate if someone becomes hostile, seems unwilling to contribute, talks over others, misrepresents others, or is otherwise not part of a healthy conversation.

* **Do not submit your perspective.** Because you’re playing a neutral role and there’s nobody to mediate you, you should withhold your own perspective. If you are the Designer’s manager you’re in an unfortunate position of also having veto power; you must be very careful to keep these roles separate.

* **Keep an eye on the clock.** Ensure that the Designer and veto parties always have a chance to make their intent known at the end of the session.

Thanks in advance to every moderator who helps us come to a better answer than we would have otherwise.

### Vetoes

Vetoes are a check against delegation of decisions to Designers. They are intended to protect against decisions that would do harm to the org or company and usually indicate that something was missed during the design process, whether an irreconcilable difference of judgment, aDesigner’s unwillingness to listen to feedback, or a perceived abuse of the process.

Vetoes should be rare and must always be accompanied by an explanation.

#### Who can veto

Three parties have veto authority:

* **The Designer’s management chain. **As part of the Designer’s management chain, you are ultimately responsible for what they work on and how they do so. Your veto is most appropriate when:

    * The Designer is being unreasonable during the review process.

    * There simply isn’t bandwidth to do what the Designer proposes.

    * You believe the proposal incurs unreasonable risk or cost.

* **The Owner’s representative.** As the reviewing member of the Owners, your team will ultimately be responsible for the outcome of this proposal. Note that only the delegate who participates in the review has veto privileges. If you do choose to veto, be aware that the problem under discussion was apparently perceived as important enough that the Designer felt compelled to submit a proposal. Your veto is most appropriate when:

    * The proposal would require work that your team simply cannot perform.

    * The proposal would have an unacceptable impact on the areas under your responsibility.

* **The Architect(s).** As the relevant Architect, you are ultimately responsible for the technical health of this part of the company. Your veto is never appropriate simply for disagreement; it is only appropriate when:

    * The proposal would do significant, lasting harm to the technical environment or organization.

#### How to veto

Ideally, veto privileges are never used. Seek to persuade first and foremost. Vetoes are a last resort to block an implementation that simply *cannot* happen.

Never lead with veto or the threat of veto. Instead, make your case and read the room. Most Designers will notice if the entire review panel is aligned against what they’re proposing and will choose to either revise or abandon the proposal. On the other hand, if you’re isolated in your position, it warrants reflecting on why and whether a veto is appropriate.

In the case where it’s unavoidable:

* **Don’t let it be a surprise.** Articulate your reasons as clearly as you can.

* **At the close of the session**, when asked, announce your veto and repeat your reasons.

* **Never humiliate the Designer**. No matter how contentious the situation may get, keep the reasons to a precise accounting of why you feel *this solution* to *this problem* cannot be pursued.

* **After the session**, add a comment to the top of the Solution Proposal noting your veto. Clearly explain your reasons. This is a public document; vetoes are never secret.

## IV. Technical Design Document

**_Deliverable: _***a completed, reviewed Technical Design Document.*

The Technical Design Document (TDD) is what’s typically meant when technical design is discussed. It’s a detailed, heavy-weight document that accounts for everything from operationalization to API specifications to scalability characteristics. This process is agnostic about the TDD format. Requirements vary wildly based on the nature of the project, so dictating a single template is unlikely to succeed.

Instead, internal examples will gradually be added to this process document. Until then, here is Joel Spolsky’s excellent functional spec series:

* [Painless Functional Specifications - Part 1](https://www.joelonsoftware.com/2000/10/02/painless-functional-specifications-part-1-why-bother/)

* [Painless Functional Specifications - Part 2](https://www.joelonsoftware.com/2000/10/03/painless-functional-specifications-part-2-whats-a-spec/)

* [Painless Functional Specifications - Part 3](https://www.joelonsoftware.com/2000/10/04/painless-functional-specifications-part-3-but-how/)

* [Painless Functional Specifications - Part 4](https://www.joelonsoftware.com/2000/10/15/painless-functional-specifications-part-4-tips/)

These are by no means the only approach. The purpose of a TDD is not to confine your process, but to give your review panel a way to meaningfully contribute to the design process and help flesh out the decisions that you might not otherwise account for.

### TDD Cycle

Drafting the TDD is an iterative process consisting of the following steps:

#### Plan + Design + Prototype

Add a set of questions to the TDD that you intend to answer in this cycle. It depends on your style, but typically at the beginning these will be fairly high level and vague and become more detailed and precise over time.

Share the document with your review panel but don’t block on their response.

Answer the questions you posed, prototyping as necessary. Different engineers and projects require different styles. Experiment until you find what works for you.

Continue cycling until you reach a break-point appropriate for review. Review should only be conducted over a substantive body of work but should also not be delayed for months. Use your judgment.

#### Review

Let your panel know where you’re at in the TDD and schedule a review. This review follows roughly the same process as [review of the Proposal](#heading=h.vt175wltekjb). Once again, the intent is to help the Designer make the very best possible decisions.

Focus on missing details, structure, and broad issues with serious runtime implications. This review process is not a venue for bikeshedding; it exists to make sure the implementation works, accounts for operationalization and runtime questions, doesn’t paint future generations into a corner, and so on.

[There’s always an exit hatch](#heading=h.bgmlf1z8064l), but the use of Solution Proposals ought to cull out most non-starters. While vetoes should be exceedingly rare at this stage, the process is the same.

#### Repeat

Continue developing and reviewing the TDD until you’re satisfied with the document and feel that you can move to formal implementation. This decision follows the same rules as the Proposal review decision.

Some projects will require only a single TDD review while others may require many. Very large projects may need to be chunked so that parts may move into implementation while others remain in design.

### When to revisit the Solution Proposal

If you realize the proposal you chose is not doable, [return to that step](#heading=h.o1csimhqfnc). Move the current solution to an alternative and mark it as rejected with a link to the TDD and an explanation of the blockers you ran into.

Do not throw good money after bad; the Solution Proposal is a lightweight document that is intended to catch rather coarse problems. There will inevitably be accepted Proposals that must be abandoned because subtler issues were discovered in the TDD step. **This is not a bad thing--they were still caught before moving to implementation.**

## V. Implement

**_Deliverable: _***an implemented solution.*

Once the TDD is complete and vetoers have declined to veto, it’s time to move on to formal implementation.

At this point, you likely have a prototype that’s been built alongside the design. Ideally, delete this and draft afresh with the TDD as a reference. At the very least, put it aside and pull in only one what you need from the prototype. It’s very difficult to free yourself from the first draft and all its encoded thought processes until it’s out of sight.

The TDD should be updated as the implementation changes the design. Notify your reviewers when appropriate; whether a particular update matters to them will depend on the project and its review history. For example, word choice in an API URL is usually immaterial but if the review focused heavily on that for UX reasons a change warrants another check-in.

Go forth and do.

# FAQ

## How do I come up with alternatives?

Alternatives play an important role keeping the solution proposal honest, complete, and fair. These strategies might help if you’re struggling to find at least two viable alternatives:

* **Design to your preferred solution’s shortcomings.** Every option has its tradeoffs. How would you approach the problem statement if your preferred solution’s shortcomings were explicitly unacceptable?

* **Consider the problem from a different organization’s perspective.** How would you solve this problem as a cloud engineer? A network engineer? DBA? Web developer? Platform engineer? Security engineer?

* **Research prior art.** Many larger organizations regularly publish papers and blog posts describing how they tackle difficult problems. Search for posts by Facebook, LinkedIn, Netflix, Twitter, Yahoo, Slack, and so on. While many of their approaches are inappropriate for a company our size, their documentation often inspires fresh ideas. Likewise, browsing Apache’s catalog for open source projects that are related to the problem can help nudge you in a new direction.

* **Delegate to another engineer.** Find another engineer who’s willing to help and present them with the problem statement. Don’t give them your solution or critique their approach; just listen to how they think about the problem and what they suggest.

* **Take a break. **Copy the problem statement into a separate doc and put the whole thing aside for a couple days. When you return, re-read the problem statement alone and list five to ten options no matter how stupid or implausible. At least one of these is likely worth developing.

## How do I choose between terrible options?

If you’re faced with a set of equally crappy options, these strategies might help:

* **Search for more alternatives.** Use [the above strategies](#heading=h.tuchcqi7moop) until you’re confident that you’ve got the best set of alternatives available to you.

* **Discuss with someone you trust.** Fresh eyes might see something you don’t.

* **Choose the option with the least amount of new work.** Ultimately, if you’re forced to choose a crappy option you might as well pick the one that requires less work. Consider both implementation and ongoing maintenance in this equation.

## What if I’m bad at writing?

Writing prose is its own skill that, like every other skill, requires time, practice, and feedback to develop. Despite the popular myth, good writing doesn’t spring fully formed from the fingertips of a drunken genius; it emerges through revision of [shitty first drafts](https://wrd.as.uky.edu/sites/default/files/1-Shitty%20First%20Drafts.pdf).

There are countless approaches for working from thought to final draft. If you’re at a loss, here are two that new writers find particularly helpful:

* **Topic sentences and stream of consciousness.**

    1. Write down a few topic sentences as questions (e.g., "Why does this problem need to be solved?").

    2. Answer each with a braindump. Simply unload everything you think in response to that question onto the virtual page with only a passing regard for relevance. Ignore quality and clarity in favor of completeness.

    3. Put it aside for at least a couple hours.

    4. Re-read what you have for accuracy, organization, and relevance. Cut what’s unnecessary. Ignore sentence quality; this step is about ensuring that your document conveys what’s needed.

    5. Repeat until you feel comfortable with the content if not the form.

    6. Delete the topic sentences.

    7. Put it aside again.

    8. Read it aloud. Rephrase anything you stumble over until it sound smooth to the ear. This step is about quality; at the very least what you’ve written should flow easily. Reading aloud is a way of preventing your brain from smoothing over what’s on the page without you noticing.

* **Bullet point outline.**

    9. Write down the questions this document must answer as bullet points (e.g., "Why does this problem need to be solved?").

    10. Begin filling out those bullet points as if you’re taking notes. Be as short and clipped as you like. There’s no need to follow a particular order; jump around however feels right.

    11. As your outline grows, begin to restructure some of these bullet points as complete sentences. Often, this step will beg further questions that should be answered as deeper bullet points.

    12. Continue developing this outline until you feel it’s accurate and complete.

    13. Begin merging separate bullet points into sentences. Then as paragraphs. Finally, delete the bullets altogether.

    14. Put it aside.

    15. Read it aloud. Rephrase anything you stumble over until it sound smooth to the ear. This step is about quality; at the very least what you’ve written should flow easily. Reading aloud is a way of preventing your brain from smoothing over what’s on the page without you noticing.

## What if I disagree with a decision?

A decision has been made and you feel strongly that it’s the wrong one. What you do with this depends on the situation.

First and foremost, read through the accepted proposal. One purpose of this process is to publicly document the reasoning behind decisions. Even if you disagree with the outcome, understanding the decision-making progress may help to show how or whether your concerns were accounted for.

Second, what are the consequences if the accepted proposal moves forward and you turn out to be right? If you believe the accepted proposal will cause *significant, long-term damage* to the organization, you should raise your concerns (more on that in a moment). This is a deliberately high bar for interjection; this process is designed around federated decisions and review rather than consensus or gatekeeping.

If your concerns don’t meet that high bar, let them lie. Relitigating designs after they’ve already been reviewed and accepted is cognitively heavy and often leaves people feeling distrusted, micro-managed, and defensive. Even if you’re right, the benefits of promoting responsibility outweigh the costs of allowing agreed upon work to be interrupted by anyone who disagrees.

However, if you conclude that the consequences *do* meet the bar of significant, long-term damage to the company, they need to be raised. **Reach out to the designer’s immediate manager and the architect(s) involved in the review.** Together, they’re responsible for validating your concerns. If they disagree with your conclusion, they will communicate why *and* take responsibility for the consequences if they’re wrong: it’s their necks on the line, after all. If they agree with your conclusion, the manager is responsible for communicating the problem to the designer and gently guiding them back to the design process.

The designer is not the first point of contact for three reasons. First, invoking management and the architects underscores the bar such concerns must meet to justify revisiting a design. Second, it ensures that validation of the concerns is relatively neutral. It’d be very difficult for most people to hear and evaluate such critique without defensiveness. Finally, it puts the onus on the manager to gracefully communicate a negative result to the designer.

## Should my project go through this process?

Generally speaking, significant technical projects should go through this process. The extremes are self-evident: building a new API routing layer needs a design process while updating a dependency for an existing service probably doesn't. The middle is often a judgment call.

If you're uncertain whether your project warrants working through this process, consider the following questions:

* **What are the stakes?** The higher the stakes, the more important it is to both make a good decision and to be able to show your reasoning towards that decision. If making a bad choice means annoying the person next to you and having to spend a couple hours redoing it, formally going through this process is probably not a good use of time. On the other hand, if your choice could lead to outages for the company or significant work for other teams, you should opt for the designed approach.

* **Are there other teams with a vested interest?** The more teams involved, the higher the stakes.

* **Is this issue contentious?** Is your solution? A documented and reviewed solution ensures that concerns are addressed ahead of time and allows you to answer future questions with a link rather than having to defend your decision anew.

* **Is this issue too complex to fully capture in a Jira comment and PR description?**

If you're still not sure, draft the problem statement, sketch the outline of the solution proposal, and see what your manager or a prospective reviewer think. Even if you end up skipping the remaining steps, framing the issue this way may still lead you to a stronger answer.

# Appendix A: Organic vs. designed change

There are two broad categories of technical implementation: organic, evolved change and designed change.

## Organic change

Organic change is a reactive process that adapts a system to its immediate stimulus and generally considers the rest of the system immutable. Each adaptation is incorporated into that immutable system for the next change.

For example, if the network is occasionally loses packets, an organic response might be to simply add retries to all network calls and call it good when the behavior looks better. As frequent retries overload downstream services, timeouts are raised and retry backoff is added. This results in elevated response times and a poor UX, so individual service calls are aggressively cached to improve performance. Average performance is now excellent and lost packets are not a problem, but even modestly increased latency is now amplified by the array of retries and frequent, aggressive cache checks. To protect against latency, the network is upgraded to cutting edge hardware.

Note that organic change may be planned out and "designed" in the sense that they’re written down, debated, and reviewed before being implemented. Engineers are smart; most won’t blindly change a system based on a hunch. The difference is in the approach.

## Designed change

Designed change, on the other hand, is both reactive and proactive. It seeks to view the immediate stimulus, future plans, and unexplored problems as variables in a largely mutable system. Changes are made to the system as a whole rather than treated as individual acts.

To continue the above example, a designed response would treat dropped packets as one variable within the system. This presents further questions: 

* What other network issues can go wrong? 

* Considering the product being delivered and the money and time available, what level of network flakiness is acceptable? 

* What happens if they coincide? 

* Is it better for our product to rapidly fail or to delay answering until it can? 

* Are there components with different needs? 

* If so, how can the system delegate that decision to those components?

It’s possible (albeit unlikely) that a designed change ends up expressing the same adaptation as an organic change. The difference is in the approach.

## Transitioning and process

There’s nothing inherently wrong with organic growth. Nearly every technical organization begins that way because they don’t know what they’re building or how it will behave, so they must rapidly react to context and shore up whatever needs work in the moment. Most design work would be futile since "the system" is so volatile. Ideally, as the product vision becomes clearer and use-patterns fall into a groove, a designed model becomes more viable.

This transition isn’t automatic. It requires intent, discipline, and usually some false steps. An org that remains in organic mode for too long finds it more and more difficult to get out: systems continue to stack adaptations, growing more complicated and rigid. The whole environment grows distorted by the unintentional effects of this evolution, which demands still more specific adaptations.

Traditional design processes can accidentally reinforce this pattern when paired with a culture of urgency. Design work is difficult and usually needs to be done through experimentation and prototyping. When time is at a premium, design documents become reflections of implementation rather than the other way around. Reviews become impractical and sensitive because so much work has already been sunk into the project.

Effectively, the design and review process is little more than a formality that hides and legitimizes the ongoing evolution behind the scenes.

# Appendix B: Optimization patterns

This appendix briefly covers preferred optimizations for technical designs. These warrant their own document at some point.

It’s important to note that these are *preferred optimizations*. Nothing is forbidden here and every one of the discussed concepts has perfectly valid use-cases. However, each of these were specifically selected because they’re necessary corrections to patterns common at in some development organizations. Choosing a different optimization should be carefully weighed and explicitly justified.

## Simple > Flexible > Easy

### Definitions

**Simple** solutions have as few moving parts as possible. They’re transparent, well-defined, and easy to reason and are generally made up of small units of implementation. For example, stairs.

**Flexible **solutions attempt to support multiple related possibilities to allow unforeseen interactions. Flexibility and simplicity often go hand-in-hand since simplicity lends itself to precise application. For example, an escalator allows users to move faster than stairs or expend zero effort for the same result. When broken, it reverts to stairs.

**Easy** solutions seek to flatten the learning and adoption curve, usually by concealing details within a black box. Because details must be hidden, they’re generally made up of large units of implementation. For example, an elevator is very easy to use but is much more prone to failure than stairs. When it does fail, you’re likely to get stuck in a suspended steel coffin.

### Rationale

Easy solutions tend to be brittle, high maintenance, and "easy" only for one specific path. In a landscape that changes as rapidly as a SaaS environment, optimizing for ease betrays overconfidence in the stability of the problem space.

The truth is, the world of a distributed, polyglot SaaS environment is already deeply complex. Choosing the simple implementation whenever possible makes maintenance or change far easier and allows for low-cost iteration. Most importantly, it keeps things as straightforward to reason about as possible.

Flexibility is an extension of simplicity. It should be eschewed where it’s not needed since an over-flexibility system result in a formless implementation that defers complexity to its consumers, but a properfly flexible system can be quickly iterated upon or repurposed with minimal work.

Easy should be reserved for cases where the chosen path is very stable or where the benefit is significantly higher than the cost of implementation and maintenance. This is typically the UX.

Note that this applies equally to general technical solutions, building systems, and actually writing code. In the latter case, easy code tends to optimize for adding new features while simple code tends to optimize for reading and comprehension.

## Debuggable > Performant

### Definitions

**Debuggable** solutions optimize for readability and comprehensibility while exposing the right information at runtime to understand behavior and solve problems.

**Performant** solutions optimize for throughput or latency *above an acceptable threshold*. Understanding the performance needs of a solution is critical to judging where this threshold is. There are cases where esoterica or highly compressed code is necessary to meet that threshold and simply cannot be sacrificed. Note that this threshold may change over time.

### Rationale

Both stability and performance are critical to the success of a SaaS, but performance tends to be micro-optimized for. At the scale of most SOAs, macro-optimizations like scaling, processing models, and caching strategies matter and micro-optimizations don’t.

This optimization pushes back against decisions to micro-optimize at the expense of readability or visibility.

## Composable > Extensible

### Definitions

**Composable** solutions have clear interfaces that allow them to interact rationally with other solutions in novel ways. Composability enables new behavior without changing existing implementations. Lego pieces are the ultimate composability.

**Extensible** solutions are built to allow covering of new use-cases by adding or changing the existing implementation. Extensibility usually requires prebuilt internal interfaces, or at least enough mutability to allow changes without destruction of the system. If lego pieces are composable, action figures with mountable pieces are extensible.

### Rationale

To continue torturing the metaphor, a lego person can wear any lego hat because heads are a standard size and the hats all mount using the exact same stud. An action figure can wear any hat that was designed for it specifically, but Wonder Woman’s hairband can’t be moved to Bruce Wayne’s head without a razorblade and tube of superglue.

Composable and extensible solutions are both useful, but composability allows for new integrations to be built against a standard with no changes to the existing systems. Likewise, existing components can be swapped out without affecting the rest of the system.

This is invaluable in a space as unstable as SaaS, which is why composability is the very DNA that makes up a Platform.

## Enable > Gatekeep

### Definitions

**Enabling** solutions guide by making the Right Way as foolproof, straightforward, and easy as possible. They attempt to answer the question of how to do something. For example, allow users to subscribe to a mailing list that emails them a confirmation link before sending them the list contents.

**Gatekeeping** solutions guide by either blocking the Wrong Ways or by funneling all use-cases through a chokepoint. They focus more on the question of how to prevent something. For example, require users to open a ticket to be added to a mailing list to ensure that nobody gets signed up for something they don’t want.

### Rationale

Solutions that enable tend to be built with stronger automation, gain faster adoption, and scale better than solutions that gatekeep. Most importantly, they guide users toward the desired path rather than putting up a blocker either deflects them from the wrong path or, worse, acts as a barrier to them doing the right thing.

In cases where putting up barriers is necessary for the sake of safety, security, or compliance, 

Enabling *and then* Gatekeeping tends to be more successful and less abrasive.


