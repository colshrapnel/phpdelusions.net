I know how to mitigate the Fall of Stack Overflow

### The site should be made newbie-friendly

I realize that such a blunt suggestion is likely to be met with immediate backlash, from all sides.    
But please hear me out.  

> Please note that although I try my best to express myself, English is not my native language and sometimes the choice of words, as well as grammar, are not perfect. Please try to get past such glitches to the actual meaning. This proposal is very dear to me, I was pondering on it for a long time. Please give it a thought. It actually makes sense.

Stack Overflow declares its Noble goal as ["to build a library of detailed, high-quality answers to every question about programming"](https://stackoverflow.com/tour). But such a library cannot exist without, well, questions. So Stack Overflow also lets people ask, but it gets rather picky about what it's willing to answer: only "practical, detailed questions" that "have not been asked before" are welcome. While for a user, a Q&A site is generally seen as a place to turn in times of need, regardless of the nature or magnitude of the problem. A place where they are a matter of some importance, not just a building material for a great library.

Besides, there is a major miscalculation that hinders the achievement of the Noble goal: Stack Overflow puts too much emphasis on adding new answers, more and more new answers. Only adding new answers will earn you a decent amount of Internet points, while updating an existing answer will bring you none. This approach is great for answering ad-hoc questions, but, ironically, for the Library it doesn't seem so. Just a funny fact: on the same [tour page](https://stackoverflow.com/tour) Stack Overflow is proud to call itself "not a forum". But if you open a [random popular question](https://stackoverflow.com/q/40480/285587), it looks *exactly like a forum thread,* with dozens (if not hundreds) of replies, some of them even [arguing with each other](https://stackoverflow.com/a/51522896/285587). And this lengthy discussion doesn't quite fit in with what I imagine to be a "high quality answer" (singular) - what I consider to be the direct result of such a policy.

Still, the experience for people who come for help is the worst.

### The problem

*Most of the questions currently asked on Stack Overflow are off topic*. And that means something has gone very wrong.

Right now I am looking at the list of new questions under the [[Java]](https://stackoverflow.com/questions/tagged/java?tab=Newest) tag. Many of them are either already closed, or in the process of being closed, with one or two votes. And it's no wonder, given the list of closure reasons provided below. Though just the first one is enough, since after all these years, 90% of new questions are inevitably duplicates, more or less. And we don't see all of them closed on the spot just because it takes a considerable effort to find the right duplicate, while Stack Overflow doesn't offer a single Internet point for that, so few people care. Let's have a look at the list, though:

- Duplicate
- Needs details or clarity
- Needs more focus
- Opinion-based
- Needs debugging details
- Seeking recommendations for books, tools, software libraries, and more
- Not reproducible or was caused by a typo    

(After putting them together, I can't help but hear them [intoned by a snarky teacher](https://i.stack.imgur.com/ZjOYA.jpg)).

But, jokes aside, there is a real problem: this list outlaws most *newbie* questions out there. When you think about it, asking a good question is a skill in itself. And even for a professional, it takes considerable effort. Yet, the same skill is expected from someone taking their first feeble steps in the profession. But instead of help, most newbies are met with closures and downvotes. Because Stack Overflow doesn't want [stupid questions](https://meta.stackoverflow.com/q/314372/285587) to get in the way of the Noble goal.

So it's no wonder that too many people are starting to see Stack Overflow as a hostile place, which has even become [a meme](https://www.reddit.com/r/ProgrammerHumor/comments/u49a6j/sad_truth/). There is an old article, [The decline of Stack Overflow](https://johnslegers.medium.com/the-decline-of-stack-overflow-7cb69faa575d) (not to be confused with "The Fall..." mentioned below), followed by a [heated discussion on Reddit](https://www.reddit.com/r/programming/comments/3cafkp/is_stack_overflow_overrun_by_trolls/), where people are using rather strong language towards volunteers and moderators. And - I think - unfairly. The entire community cannot be trolls. The problem is not the people, it is the system. Mods are not evil. They simply follow the rules which are designed to serve the Noble purpose.

Still, all their efforts are for naught. It's just *technically impossible* to ban all the people who have no idea about the Library, but just have a problem and are looking for help (which could be just a nudge in the right direction or a fresh pair of eyes). For them, it's just natural to ask a programming question on Stack Overflow... only to find that their question isn't good enough for that site.

On the other hand, many people who have the time to help a fellow undergraduate, do not understand why it's forbidden either. Or how to actually earn their internet points, if most questions must be closed, not answered. Or, for that matter - how to earn enough reputation points that would enable them to actually close a question, or at least to leave a comment. That's too twisted a logic, people will never buy it.

And that brings us to the point of this article. You are probably aware of this recent post, [The Fall of Stack Overflow](https://observablehq.com/@ayhanfuat/the-fall-of-stack-overflow). It's a fact that traffic is drying up. This means that Stack Overflow is less in demand as a "library of quality answers". But there are still people out there who need help.

I think it's time to embrace them. Yes, of course proper research needs to be encouraged. But it doesn't mean that learners should be just flipped off. Stack Overflow shouldn't be an elitist club. A Q&A site has to be open for everyone.

### Creating better experience for those who ask

Why not do it this way: just let a silly question be answered, but then *simply delete it* altogether, without much ado? It will make everyone happy, yet such questions won't get in the way of the Noble goal!

I strongly believe that the following changes must be implemented:

- No voting down on questions. It serves absolutely no purpose other than to harass the OP and as a flag for the future auto-deletion. Optionally, there could be some canned responses that *explain* how the question could be improved, which is much better than a downvote
- Instead, there needs to be an option to flag a question as having *no community value*
- Questions flagged as such, must simply be *removed* after being resolved (or after a reasonable timeout)
- The closure reasons listed above should just add such a flag, but don't close the question, letting anyone answer it.

The irony is that it will be pretty much the same as the current approach (when low quality questions are downvoted, closed, but still answered, guerilla-style - in the comments section under the already closed question - and finally get [automatically deleted](https://stackoverflow.com/help/auto-deleted-questions)). But - without all the rancor! You see, it's really simple: if a question is going to be deleted anyway, there's no point in punishing the OP and there's no harm in answering it. Same result but without all the bad vibes!

### Fulfilling the Noble purpose

Even after making it hard for newbies, the Library is still not that great. When we land from search, it's the same old answer to an ad-hoc question that Google, for reasons best known to itself, has declared canonical. And many of those answers are hardly intermediate, least of "high" quality. Such is human nature, people never write a canonical answer to an ad-hoc question. Instead, they always cater to the OP to the minute detail. Not to mention those [crazy races](https://meta.stackexchange.com/questions/9731/fastest-gun-in-the-west-problem), when a question gets half a dozen answers in 5 minutes. Which might be good for ad-hoc questions, but is a huge disservice when it comes to canonical answers.

All too often, instead of a "detailed high-quality answer", a user coming from a search is faced with multiple similar questions, each crowded with a motley lot of different attempts to answer it. Which makes finding a good answer more of a challenge:

- First, each question is asked [dozens of times](https://www.google.com/search?q=submit+form+without+reloading+page+js+site:stackoverflow.com).
- Second, each of them gets [dozens of answers](https://stackoverflow.com/questions/2866063/submit-form-without-page-reloading).
- Third, nobody cares whether those answers are up to date, feature a good practice or outright make any sense. And no, voting does not help. Following the [herd conscience](https://en.wikipedia.org/wiki/Herd_behavior), people tend to upvote already popular answers. For example, in the question above, the modern way of doing things, using the Fetch API, is only mentioned way down at the bottom, being awarded one single upvote in a year. How many people will make it to that answer? How many people will have enough expertise to understand that this is the recommended method nowadays?
- Finally, many false positive results, when one lands on a question with a misleading title.

Yes, a relatively small number of the most popular questions gradually get something that can be called an acceptable answer, or even a high-quality one. The problem is that it's rather against the rules. Or at least never encouraged by the current regulations. Which:

- encourage selfish and competitive attitude
- reward you only for adding more new answers but never for improving an existing one
- defend a selfish author who doesn't want their answer to be improved.
- doesn't distinguish between ad-hoc questions and canonical ones. Quite too often, when you land on a question with a canonical title, all you see is some shortcut or a questionable tradeoff. Just a few examples: [Cannot delete or update a parent row](https://stackoverflow.com/a/17828127/285587) and [mysql error 1364 Field doesn't have a default values](https://stackoverflow.com/questions/15438840/mysql-error-1364-field-doesnt-have-a-default-values) are fairly common SQL errors that deserve a thorough explanation. But both topmost answers are anything but. Yes, for the *particular ad-hoc question* they may offer a plausible tradeoff. But after picking up some wind, a question automatically becomes a *canonical post on the topic*, where people come with different problems indicated by the same error message. And such a narrow-minded answer instantly becomes a disservice.

A recent case made it clear: the internal stance of Stack Overflow is diametrically opposed to what programmers expect from it. A volunteer, Jan Schultke, who made a habit of "regularly modernizing C++ questions and answers by adding notes or changes based on more recent versions", had been reproached for it. And decided to ask a question on Meta, [When do modernization edits conflict with the author's intent?](https://meta.stackoverflow.com/q/426288/285587), which, to me, boils down to a more generic "Whether old but popular answers should be updated or left as is?". And active users of Stack Overflow voted for the latter: on the next day the question had -10 and a sole answer suggesting to leave old answers alone had about +20. But after I [posted the link on Reddit](https://old.reddit.com/r/programming/comments/168u010/should_an_answer_on_stack_overflow_remain_a/), the question changed its score to positive and got quite a reasonable answer supporting the modernization of old answers, while the comments on Reddit agreed on "that's what I would hope to see on SO". 

And it makes sense. Remember that library of high quality answers. Nowhere on the tour page does it say "a memorial to old practices". Stack Overflow needs to change its stance on this as well. 

I strongly believe that such a library should provide the current generic approach by default, while optionally listing legacy and bleeding edge solutions. And in this regard, I believe that a canonical answer must be specifically made, by means of editing the initial ad-hoc reply, while targeting an "average Joe" as its reader, not the original poster. And the creation of such answers must be based on a collaborative, not competitive effort.

Any question that survives the "community value" screen, has to be heavily edited:

- The title must be made to reflect the exact problem
- All insignificant details must be edited out
- The answer should address the generic question rather than dwell on irrelevant issues
- The best practice should be provided first, while all shortcuts and tradeoffs mentioned later, with their drawbacks well explained
- The most important part: canonical answers must be regularly maintained and updated, to stay current

And of course, people should be rewarded with those Internet points for all that effort. Or even something more substantial, this "buy me a coffee" stuff is not that hard to implement.

### Separating the achievement of two goals

As you can see, Stack Overflow currently puts the burden of creating reference questions for the Library on the people who ask. "Either make your question canonical or go away". But that will never work. That task should be carried on by the community, by people of experience, not by people who ask questions.

And for the best results, achievement of these goals is better to be physically separated. Ad-hoc questions could be answered in a forum-like mode, a dialog that allows both parties to add code or images, to answer each other's questions. While Stack Overflow prides itself on not being a forum, there is nothing to brag about. Many questions simply *cannot* be answered without a discussion. Why not make it comfortable for all parties?

At the same time, canonical answers are best to be made more like Wikipedia articles, with a separate "Discussion" tab, and are written specifically, including a correct title, a generic question and a canonical answer, that provides the practice first and any shortcuts (if any) later, with drawbacks well explained.

Such a separation will solve many problems at once:

- Canonical answers won't be distracted by irrelevant details or noise (which was the original goal when Stack Overflow called itself "not a forum")
- Ad-hoc questions will be answered in a truly non-judgmental, friendly, and comfortable environment.
- New contributors will earn as much reputation as they want

And Stack Overflow will indeed become that "Library of detailed, high-quality answers to every question about programming" it aspires to be.

---

I strongly believe that the idea is viable and it can prevent the decline of Stack Overflow. But I'm sure I've missed many important issues, because it's hard to consider all edge cases from just one point of view. That's why I am posting here on the Meta site of Stack Overflow: I am looking for feedback. Please share your thoughts and criticism. The only outcome I refuse to accept in advance is to leave everything as is. Simply because it's a road to decline. And in this case someone else will inevitably create another Q&A site, where anyone can ask a programming question without being reproached for having a problem of not as much importance.
