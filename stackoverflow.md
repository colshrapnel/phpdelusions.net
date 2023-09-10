I know how to mitigate the Fall of Stack Overflow

### It simply should be made newbie-friendly

I do realize that such a blunt suggestion is likely to be met with a backlash.   
Also, I think it's not the best time, given all the perturbations of the [moderation strike](https://meta.stackexchange.com/questions/389811/moderation-strike-stack-overflow-inc-cannot-consistently-ignore-mistreat-an) to which I greatly sympathize.    
But please, hear me out.  

> Please note, English is not my native language and sometimes the choice of words, as well as grammar, are not perfect. Please try to get past such glitches to the actual meaning. This proposal is very dear to me, I was pondering on it for a long time. Please give it a thought. It actually makes sense. 

### The eternal controversy

Stack Overflow was designed to sit between two stools: to answer "ad-hoc" questions and ["to build a library of detailed, high-quality answers to every question about programming"](https://stackoverflow.com/tour). But when pursued simultaneously, these two goals are rather mutually exclusive, and, as a result, frantically step on each other's toes, greatly *worsening* the achievement of both. I call it the eternal controversy of Stack Overflow.

Current regulations are so confused that it make users to clash in every direction:

- People asking questions don't understand why it's forbidden. And ask anyway. 
- People willing to help do not understand why it's forbidden to help a fellow human being to get 
out of a simple confusion. And do it anyway, guerrilla-way, even if it takes a long discussion in the comments under already closed question
- People who keep in mind that library of high quality answers, get bewildered by the number of low quality questions (and answers as well, because such a question hardly gets a good answer)
- While restricting new questions, Stack Overflow still encourages that [mad race](https://meta.stackexchange.com/questions/9731/fastest-gun-in-the-west-problem) of adding more and more new answers. Simply because people are awarded internet poins only for adding new answers but never for improving existing ones
- Finally, all that mess *is* "the library of answers" we have. As a result, people coming from search have to shuffle through innumerable attempts to answer their question, as opposed to settling with a single canonical answer

Or, rather a funny fact: Stack Overflow claims itself being [not a forum](https://stackoverflow.com/tour), but if you take a look at any [popular question](https://stackoverflow.com/q/40480/285587), it looks *exactly like a forum thread,* where answers or comments often [argue with each other](https://stackoverflow.com/a/51522896/285587).

But, out of all that confusion, I'd say that experience for people coming to ask a question is the worst. 

### The problem

*Most questions currently asked on Stack Overflow are off topic*. It's hard to believe, but it's a fact. Right now I am looking at the list of new questions under the [[Java]](https://stackoverflow.com/questions/tagged/java?tab=Newest) tag. Most of them are either already closed, or in the process, having one or few close votes on them. 

And it's no wonder, given the list of closure reasons (excluding those intended for deliberate harm):

- Duplicate
- Needs details or clarity
- Needs more focus
- Opinion-based
- Needs debugging details
- Seeking recommendations for books, tools, software libraries, and more
- Not reproducible or was caused by a typo

It simply outlaws *most newbie questions out there!* And if new questions still make it to Stack Overflow, it's rather due to negligence. That's not normal. There is a huge mismatch between what people take Stack Overflow for, and what Stack Overflow actually is.

Yet, people still come asking questions anyway, only to face with closures and downvotes. 

And all this gruesome experience is to prevent [stupid questions](https://meta.stackoverflow.com/q/314372/285587) from getting in the way of the Noble goal, creation of that *library of high-quality answers*. And this situation is really getting out of hand, because too many people are starting to see Stack Overflow as an evil place.

There is an old article, [The decline of Stack Overflow](https://johnslegers.medium.com/the-decline-of-stack-overflow-7cb69faa575d) (not to be confused with "The Fall..." mentioned below), with a long list of similar articles at the bottom, followed by a [heated discussion on Reddit](https://www.reddit.com/r/programming/comments/3cafkp/is_stack_overflow_overrun_by_trolls/). This article, although being factually correct, uses rather a strong language towards volunteers and moderators. And - as I feel it - unjustly. The whole community cannot be trolls. The problem is not the people, but the system: it's the very contradiction mentioned above that antagonizes people. And mods are not evil. They just follow the rules aimed at fulfilling the Noble purpose.

Yet, all their effort is for naught. It's just *technically impossible* to ban all those people who have no idea of the Noble purpose but just have a problem at hadn ad consider a programming Q&A site the rith place ho get help. Or those who have spare time to help a fellow undergraduate. It's an ever-losing battle, no mater how strict rules you impose. 

Besides, it's just inhumane. Asking a good question is a skill on its own. And even for a professional it takes a considerable effort. But the same skill is expected from someone taking first feeble steps in the profession. Yes, doing a proper research of course must be encouraged. But it doesn't mean that learners should be just flipped off. Stack Overflow shouldn't be made into an elitist club. A Q&A site must be open for everyone. 

Besides, it's simply *counter-productive*. Why even ban people from asking a silly question? Especially in the current situation. You are probably aware of this article, [The Fall of Stack Overflow](https://observablehq.com/@ayhanfuat/the-fall-of-stack-overflow). It's a fact that traffic is drying up. Which means, Stack Overflow is less demanded as that "library of high-quality answers". But people who prefer assistance from a fellow human being still exist and numerous. It's time to embrace them. 

### Creating better experience for those who ask

Why not to make it this way: actually *answer* a newbie question but then *just delete it*? It will make everyone happy, yet such questions won't get in the way of the noble goal.

I am strongly convinced that following changes must be implemented:

- No voting down on questions. It serves absolutely no purpose other than harassing the OP and as a flag for the future auto-delete (optionally, there could be some canned responses that *explain* how the question could be improved, which is much better than a downvote)
- Instead, there must be an option to flag a question as having *no community value*
- Questions flagged such must be simply *removed* after getting resolved (or after a reasonable timeout)
- Closure reasons listed above should just add such a flag, but don't close the question, letting anyone to answer.

The irony, it will be pretty much the same as the current approach, when [low quality questions get automatically deleted](https://stackoverflow.com/help/auto-deleted-questions), but - without all the grudge! You see, it's really simple: if a question is going to be deleted anyway, it's no use to punish the OP and does no harm to answer. The same outcome but without all the bad vibes!

### Fulfilling the Noble purpose

Now, to our "library of detailed, high-quality answers". Even after making it hard for newbies, Stack Overflow still doesn't achieve its noble goal. Because it uses same ad-hoc questions for creating canonical answers, employing the same competitive approach. But what is good for ad-hoc questions, turns into a great disservice when it comes to canonical answers. 

Quite too often, instead of a "detailed high-quality answer", a user coming from search is faced with multiple similar questions, each crowded with a motley lot of assorted attempts to answer it. 
Which makes finding a good answer rather a challenge:

- First, each question being asked [dozens of times](https://www.google.com/search?q=submit+form+without+reloading+page+js+site:stackoverflow.com).
- Second, each separate question gets [dozens of answers](https://stackoverflow.com/questions/2866063/submit-form-without-page-reloading).
- Third, nobody cares whether these answers are up to date, feature a good practice or outright make any sense. For example, for the question above, the modern way of doing things, using Fetch API, is only mentioned way down below, being awarded one single upvote in a year. How many people will make it to this answer? How many people will have enough expertise to understand that it's the recommended method nowadays?
- Finally, many false positive results, when one lands on a question with a misleading title (no examples because I am actively editing such questions to make them match with answers).

Yes, a relatively small number of most popular questions gradually get something that can be called an acceptable answer, or even a high-quality one. The problem is, it's rather against the rules. Or at least never encouraged by current regulations. Which:

- encourage selfish and competitive attitude 
- encourage adding new answers as opposed to improving an existing one
- defend a selfish author who doesn't want their answer to be improved
- doesn't distinguish between ad-hoc questions and canonical ones. Quite too often, when you land on a question with a canonical title, all you see is some shortcut or a questionable tradeoff. Just couple examples: [Cannot delete or update a parent row](https://stackoverflow.com/a/17828127/285587) and [mysql error 1364 Field doesn't have a default values](https://stackoverflow.com/questions/15438840/mysql-error-1364-field-doesnt-have-a-default-values) are quite frequent SQL errors that deserve a thorough explanation. But both topmost answers contain anything but such explanation. Yes, for the *particular ad-hoc question* they possibly offer a plausible tradeoff. But after picking up some wind, a question automatically becomes a *canonical post on the topic* (while *nowhere being so!*), where people come with different problems indicated by the same error message. And such a narrow-minded answer instantly becomes a disservice.

As you can see, using the same approach for answering ad-hoc questions and building a library of  canonical answers creates rather a mess.

I am strongly convinced that creation of such a library should be a distinct task, separated from answering ad-hoc questions. And it must be based on a collaborative, not competitive effort. 

A "customer" of such answer must be a person coming from search, not the original poster.

Every question that survived the "community value" screen, has to be heavily edited. Its title must be made to reflect the exact problem. All petty and insignificant details must be edited out. While answer should address the generic question instead of dwelling on irrelevant issues. The best practice should be provided first, while all shortcuts and tradeoffs mentioned later, with their drawbacks well explained. And of course people should get those internet points for all that effort.  

The participant's mindset must be changed as well. They should stop catering to the OP who asked a question ten years ago. They should focus on providing a generic answer for a wider audience. 

### Separating the achievement of two goals

As you can see, currently Stack Overflow puts the burden of creating reference questions for that "library of high quality answers" on the people who ask. "Either make your question canonical or go away". But it will never work. That task should be carried on by the community, by people of experience, not by people who ask questions.

And for the best results, achievement of these goals is better to be physically separated. Ad-hoc questions could be answered in a forum-like dialogue, allowing both parties to add code, images, to answer each other's questions. Although Stack Overflow takes pride for being not a forum, there is nothing to boast about. Many questions simply *cannot* be answered without a discussion. Why not to make it comfortable for all parties? 

At the same time, canonical answers are best to be made more like Wikipedia articles, with a separate "Discussion" tab, and being specifically written, including a correct title, a generic question and a canonical answer, that features a good practice first and any shortcuts (if any) later, with drawbacks well explained.

Such a separation will solve many problems at once:

- canonical answers won't be distracted by irrelevant details or noise (which was the initial goal when Stack Overflow claimed itself "not a forum")
- ad-hoc questions will be answered in a really non-judging, friendly and convenient environment
- new participants will earn as much reputation as they want

---

Of course, this is not a complete plan, but rather an idea, many details has to be considered and well thought. But I am strongly convinced that current stance has to be changed. Otherwise someone else will inevitably create another Q&A site, where anyone can ask a programming question without being reproached for having a problem of not that global calibre.
