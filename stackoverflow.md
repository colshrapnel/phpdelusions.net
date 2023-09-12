I know how to mitigate the Fall of Stack Overflow

### The site should be made newbie-friendly

I do realize that such a blunt suggestion is likely to be met with immediate backlash, from all sides.    
But please, hear me out.  

> Please note, although I am trying my best to express myself, English is not my native language and sometimes the choice of words, as well as grammar, are not perfect. Please try to get past such glitches to the actual meaning. This proposal is very dear to me, I was pondering on it for a long time. Please give it a thought. It actually makes sense.

Stack Overflow declares its Noble goal as ["to build a library of detailed, high-quality answers to every question about programming"](https://stackoverflow.com/tour). But such a library cannot exist without, well - questions. Thus Stack Overflow also lets people ask, while being, however, rather picky about questions it's willing to answer: only "practical, detailed questions" are welcome. While for a user, a Q&A site is commonly seen as a place to which they turn in distress, no matter the nature or calibre of their problem. A place where they are a matter of some importance, not just a building material for a great library.

Besides, there is one huge miscalculation that impedes the achievement of the Noble goal: Stack Overflow puts too much emphasis on adding new answers, more and more new answers. Only adding new answers will earn you a decent amount of internet points, but never updating an existing one. Ironically, although this approach is rather good for answering ad-hoc questions, is it really best for that Library? Just a funny fact: on the same [tour page](https://stackoverflow.com/tour) Stack Overflow takes pride for calling itself "not a forum". But if you open a [random popular question](https://stackoverflow.com/q/40480/285587), it looks *exactly like a forum thread,* with dozens (if not hundreds) replies, some of which even [argue with each other](https://stackoverflow.com/a/51522896/285587).

But still, the experience for people who come for help is the worst.

### The problem

*Most questions currently asked on Stack Overflow are off topic*. Right now I am looking at the list of new questions under the [[Java]](https://stackoverflow.com/questions/tagged/java?tab=Newest) tag. Most of them are either already closed, or in the process, having one or few close votes on them. And it's no wonder, given the following list of closure reasons (actually, just the first item is enough, as after all those years, 90% of newly asked questions are inevitably duplicates, more or less):

- Duplicate
- Needs details or clarity
- Needs more focus
- Opinion-based
- Needs debugging details
- Seeking recommendations for books, tools, software libraries, and more
- Not reproducible or was caused by a typo    

(After putting them together, I can't help but hear them [intoned by a snarky teacher](https://i.stack.imgur.com/ZjOYA.jpg)).

But, jokes aside, there is a real problem: this list outlaws most *newbie* questions out there. If you think of it, asking a good question is a skill on its own. And even for a professional it takes a considerable effort. Yet, the same skill is expected from someone taking first feeble steps in the profession. But instead of help, most newbies only meet with closures and downvotes. Because Stack Overflow doesn't want [stupid questions](https://meta.stackoverflow.com/q/314372/285587) to get in the way of the Noble goal.

So it's no wonder that too many people are starting to see Stack Overflow as an evil place, which has even become [a meme](https://www.reddit.com/r/ProgrammerHumor/comments/u49a6j/sad_truth/). There is an old article, [The decline of Stack Overflow](https://johnslegers.medium.com/the-decline-of-stack-overflow-7cb69faa575d) (not to be confused with "The Fall..." mentioned below), followed by a [heated discussion on Reddit](https://www.reddit.com/r/programming/comments/3cafkp/is_stack_overflow_overrun_by_trolls/), where people are using rather strong language towards volunteers and moderators. And - I believe it - unjustly. The whole community cannot be trolls. The problem is not people, but the system. Mods are not evil. They just follow the rules aimed at fulfilling the Noble purpose.

Yet, all their effort is for naught. It's just *technically impossible* to ban all those people who have no idea of that Library, but just got a problem at hand and look for assistance (which could be just a nudge in the right direction or a fresh pair of eyes). To them, it's just natural to ask a programming question on Stack Overflow... only to realize that their question isn't good enough for this site.

On the other hand, many people who have spare time to help a fellow undergraduate, do not understand why it's forbidden either. Or how to actually earn their internet points, if most questions must be closed, not answered. Or, for that matter - how to gain enough reputation points that would enable them to actually close a question or at least to leave a comment. That's too twisted a logic, people will never buy it.

And here we are coming to the point of this article. You are probably aware of this recent post, [The Fall of Stack Overflow](https://observablehq.com/@ayhanfuat/the-fall-of-stack-overflow). It's a fact that traffic is drying up. Which means, Stack Overflow is less demanded as that "library of high-quality answers". But people who prefer assistance from fellow human beings still exist and are numerous.

I believe it's time to embrace them. Yes, doing proper research of course must be encouraged. But it doesn't mean that learners should be just flipped off. Stack Overflow shouldn't be an elitist club. A Q&A site must be open for everyone.

### Creating better experience for those who ask

Why not make it this way: just let a silly question be answered, but then *simply delete it* altogether, without much ado? It will make everyone happy, yet such questions won't get in the way of the Noble goal.

I am strongly convinced that following changes must be implemented:

- No voting down on questions. It serves absolutely no purpose other than harassing the OP and as a flag for the future auto-delete. Оptionally, there could be some canned responses that *explain* how the question could be improved, which is much better than a downvote
- Instead, there must be an option to flag a question as having *no community value*
- Questions flagged such, must be simply *removed* after getting resolved (or after a reasonable timeout)
- Closure reasons listed above should just add such a flag, but don't close the question, letting anyone answer.

The irony, it will be pretty much the same as the current approach (when low quality questions are downvoted, closed, yet still answered, guerilla-style - in the comments section under the already closed question - and finally get [automatically deleted](https://stackoverflow.com/help/auto-deleted-questions)). But - without all the grudge! You see, it's really simple: if a question is going to be deleted anyway, it's no use to punish the OP and does no harm to answer. The same outcome but without all the bad vibes!

### Fulfilling the Noble purpose

Even after making it hard for newbies, the Library is still not that great. When we land from search, it's the same old answer to an ad-hoc question, which Google appointed canonical, for reasons best known to itself. And many such answers are hardly intermediate, least of "high" quality. Such is human nature, people never write a canonical answer to an ad-hoc question. Instead, they always cater for the OP to the minute detail. Not to mention those [mad races](https://meta.stackexchange.com/questions/9731/fastest-gun-in-the-west-problem), when a question gets half a dozen different answers in 5 minutes. Which could be good for ad-hoc questions, but turns into a great disservice when it comes to canonical answers.

Quite too often, instead of a "detailed high-quality answer", a user coming from search is faced with multiple similar questions, each crowded with a motley lot of assorted attempts to answer it. Which makes finding a good answer rather a challenge:

- First, each question is asked [dozens of times](https://www.google.com/search?q=submit+form+without+reloading+page+js+site:stackoverflow.com).
- Second, each separate question gets [dozens of answers](https://stackoverflow.com/questions/2866063/submit-form-without-page-reloading).
- Third, nobody cares whether these answers are up to date, feature a good practice or outright make any sense. And no, voting of no help whatsoever. Following the [herd conscience](https://en.wikipedia.org/wiki/Herd_behavior), people tend to upvote already popular answers. For example, for the question above, the modern way of doing things, using Fetch API, is only mentioned way down below, being awarded one single upvote in a year. How many people will make it to this answer? How many people will have enough expertise to understand that it's the recommended method nowadays? Besides, it's just incorrect to rely on the votes from people who are looking for answers. By definition they don't have enough expertise to judge. While those who do, just have no business reading answers.
- Finally, many false positive results, when one lands on a question with a misleading title.

Yes, a relatively small number of most popular questions gradually get something that can be called an acceptable answer, or even a high-quality one. The problem is, it's rather against the rules. Or at least never encouraged by current regulations. Which:

- encourage selfish and competitive attitude
- reward you for adding more new answers but never for improving an existing one
- defend a selfish author who doesn't want their answer to be improved.
- doesn't distinguish between ad-hoc questions and canonical ones. Quite too often, when you land on a question with a canonical title, all you see is some shortcut or a questionable tradeoff. Just a couple examples: [Cannot delete or update a parent row](https://stackoverflow.com/a/17828127/285587) and [mysql error 1364 Field doesn't have a default values](https://stackoverflow.com/questions/15438840/mysql-error-1364-field-doesnt-have-a-default-values) are quite frequent SQL errors that deserve a thorough explanation. But both topmost answers contain anything but such an explanation. Yes, for the *particular ad-hoc question* they possibly offer a plausible tradeoff. But after picking up some wind, a question automatically becomes a *canonical post on the topic*, where people come with different problems indicated by the same error message. And such a narrow-minded answer instantly becomes a disservice.

The recent case made it clear: the internal stance of Stack Overlow is diametrically opposite to what programmers expect from it. Some volunteer, Jan Schultke, who took a habit of "regularly modernizing C++ questions and answers by adding notes or changes based on more recent versions", had been reproached for it. And decided to ask a question on Meta, which boils down to [whether old but popular answers should be updated or left as is](https://meta.stackoverflow.com/q/426288/285587). And active users of Stack Overflow voted for the latter: the question had -10 and a sole answer suggesting to leave the old answer alone had something like +10. But after the link to this question had been [posted on Raddit](https://old.reddit.com/r/programming/comments/168u010/should_an_answer_on_stack_overflow_remain_a/), the question got quite a positive score and a couple supporting answers appeared, while comments on Reddit agreed on "that's what I would hope to see on SO". And it makes sense. Remember that library of high quality answers. Nowhere does the tour page say "a memorial of old practices". Stack Overflow must change its stance on this matter too. 

I am strongly convinced that such a library should provide a current mature approach by default, while optionally listing legacy and bleeding edge solutions. And in this regard, it's better to make the creation of canonical answers a distinct task, separated from answering ad-hoc questions. And it must be based on a collaborative, not competitive effort. And such answers must keep in mind an "average Joe" as its reader, not some particular person who asked.

Every question that survives the "community value" screen, has to be heavily edited:

- The title must be made to reflect the exact problem (That's a pet peeve of mine. When you open a question titled "How to do XXX", most likely you will see the full code that does exactly XXX! While the problem is either "how to debug XXX" or even something completely unrelated)
- All insignificant details must be edited out
- The answer should address the generic question instead of dwelling on irrelevant issues
- The best practice should be provided first, while all shortcuts and tradeoffs mentioned later, with their drawbacks well explained. And of course people should get those internet points for all that effort.  

The participant's mindset must be changed as well. They should stop catering to the OP who asked a question ten years ago, but focus on providing a generic answer for a wider audience.

### Separating the achievement of two goals

As you can see, currently Stack Overflow puts the burden of creating reference questions for that "library of high quality answers" on the people who ask. "Either make your question canonical or go away". But it will never work. That task should be carried on by the community, by people of experience, not by people who ask questions.

And for the best results, achievement of these goals is better to be physically separated. Ad-hoc questions could be answered in a forum-like mode, a dialogue that allows both parties to add code or images, to answer each other's questions. Although Stack Overflow takes pride for being not a forum, there is nothing to boast about. Many questions simply *cannot* be answered without a discussion. Why not make it comfortable for all parties?

At the same time, canonical answers are best to be made more like Wikipedia articles, with a separate "Discussion" tab, and being specifically written, including a correct title, a generic question and a canonical answer, that features a good practice first and any shortcuts (if any) later, with drawbacks well explained.

Such a separation will solve many problems at once:

- canonical answers won't be distracted by irrelevant details or noise (which was the initial goal when Stack Overflow claimed itself "not a forum")
- ad-hoc questions will be answered in a really non-judging, friendly and convenient environment
- new participants will earn as much reputation as they want

---

I am strongly convinced that the idea is viable and it can prevent the decline of Stack Overflow. But surely I overlooked many important issues, because it's hard to consider all edge cases from just a single point of view. That's why I am posting here on the Meta site of Stack Overflow: I am looking for feedback. Please share your thoughts and criticism. The only outcome I refuse to accept in advance is leaving everything as is. Simply because it's a road to decline. And in this case someone else will inevitably create another Q&A site, where anyone can ask a programming question without being reproached for having a problem of not as much importance.
