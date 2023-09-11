I know how to mitigate the Fall of Stack Overflow

### The site simply should be made newbie-friendly

I do realize that such a blunt suggestion is likely to be met with immediate backlash, from all sides.     
But please, hear me out.  

> Please note, English is not my native language and sometimes the choice of words, as well as grammar, are not perfect. Please try to get past such glitches to the actual meaning. This proposal is very dear to me, I was pondering on it for a long time. Please give it a thought. It actually makes sense. 

Stack Overflow declares its goal as creation of ["a library of detailed, high-quality answers to every question about programming"](https://stackoverflow.com/tour). It's the Holy Grail, the Noble goal. But such a library cannot exist without, well - questions. So Stack Overflow also lets people to ask them. But it looks like that people who have questions are not much a concern. They are just a building material for the great library. And Stack Overflow being picky about questions it's willing to answer: only "practical, detailed questions" are welcome. And such an attitude being the source of huge misunderstanding, a Q&A site is commonly seen a place where people are a concern, not asset. A place where they can get help for their problem.

Such ambiguous situation creates a lot of confusion and contradiction:

- People asking questions don't understand why it's forbidden. 
- People willing to answer do not understand why it's forbidden to help a fellow human being to get out of a simple confusion.
- People who share that Noble goal, get bewildered by the number of low quality questions

Or, such a funny fact: on the same tour page, Stack Overflow claims itself being "not a forum". But if you open a [random popular question](https://stackoverflow.com/q/40480/285587), it looks *exactly like a forum thread,* with numerous replies, some of which even [argue with each other](https://stackoverflow.com/a/51522896/285587).

But, out of all that confusion, I'd say that experience for people coming to ask a question is the worst. 

### The problem

*Most questions currently asked on Stack Overflow are off topic*. Right now I am looking at the list of new questions under the [[Java]](https://stackoverflow.com/questions/tagged/java?tab=Newest) tag. Most of them are either already closed, or in the process, having one or few close votes on them. And it's no wonder, given the list of closure reasons:

- Duplicate
- Needs details or clarity
- Needs more focus
- Opinion-based
- Needs debugging details
- Seeking recommendations for books, tools, software libraries, and more
- Not reproducible or was caused by a typo    
(after putting together this list, I can't help but see it as a list of [snarky teacher's quotes](https://i.stack.imgur.com/ZjOYA.jpg))

But the most lamentable part, this list *outlaws most newbie questions out there.* 

If you think of it, asking a good question is a skill on its own. And even for a professional it takes a considerable effort. But the same skill is expected from someone taking first feeble steps in the profession. So instead if help, most newbies get only closures and downvotes. Because Stack Overflow don't want [stupid questions](https://meta.stackoverflow.com/q/314372/285587) to get in the way of the Noble goal. 

So it's no wonder that too many people are starting to see Stack Overflow as an evil place, which even become [a commonplace](https://www.reddit.com/r/ProgrammerHumor/comments/u49a6j/sad_truth/). There is an old article, [The decline of Stack Overflow](https://johnslegers.medium.com/the-decline-of-stack-overflow-7cb69faa575d) (not to be confused with "The Fall..." mentioned below), followed by a [heated discussion on Reddit](https://www.reddit.com/r/programming/comments/3cafkp/is_stack_overflow_overrun_by_trolls/), where people are using rather a strong language towards volunteers and moderators. And - I believe it - unjustly. The whole community cannot be trolls. The problem is not people, but the system. Mods are not evil. They just follow the rules aimed at fulfilling the Noble purpose.

Yet, all their effort is for naught. It's just *technically impossible* to ban all those people who have no idea of the Noble goal pursued by Stack Overflow, but just got a problem at hand and look for assistance (which could be be just a nudge in the the right direction or a fresh pair of eyes). To them, it's just natural to ask a programming question on Stack Overflow... only to realize that their question isn't good enough for this site. 

On the other hand, many people who have a spare time to help a fellow undergraduate, do not understand why it's forbidden either. Or how to actually earn their internet points, if most questions must be closed, not answered. Or, for that matter - how to gain enough reputation points that would enable them to actually close a question or at least leave a comment. That's too twisted a logic, people will never buy it. 

Besides, it's noot the best time for Stack OVerflow to be picky. You are probably aware of this recent article, [The Fall of Stack Overflow](https://observablehq.com/@ayhanfuat/the-fall-of-stack-overflow). It's a fact that traffic is drying up. Which means, Stack Overflow is less demanded as that "library of high-quality answers". But people who prefer assistance from a fellow human being still exist and numerous.

I believe, it's time to embrace them. Yes, of course doing a proper research of course must be encouraged. But it doesn't mean that learners should be just flipped off. Stack Overflow shouldn't be an elitist club. A Q&A site must be open for everyone. 

### Creating better experience for those who ask

Why not to make it this way: just let a silly question to be answered, but then *simply delete it* altogether, without further ado? It will make everyone happy, yet such questions won't get in the way of the Noble goal.

I am strongly convinced that following changes must be implemented:

- No voting down on questions. It serves absolutely no purpose other than harassing the OP and as a flag for the future auto-delete. Оptionally, there could be some canned responses that *explain* how the question could be improved, which is much better than a downvote
- Instead, there must be an option to flag a question as having *no community value*
- Questions flagged such, must be simply *removed* after getting resolved (or after a reasonable timeout)
- Closure reasons listed above should just add such a flag, but don't close the question, letting anyone to answer.

The irony, it will be pretty much the same as the current approach (when low quality questions are downvoted, closed, yet stll answered in the comments, and finally get [automatically deleted](https://stackoverflow.com/help/auto-deleted-questions)), but - without all the grudge! You see, it's really simple: if a question is going to be deleted anyway, it's no use to punish the OP and does no harm to answer. The same outcome but without all the bad vibes!

### Fulfilling the Noble purpose

Now, to our "library of detailed, high-quality answers". Even after making it hard for newbies, Stack Overflow still struggles to achieve its noble goal. When we land from search, it's the same old answer to ad-hoc question, that Google appointed canonical, for the reasons best known to itself. Such is a human nature, that people never write a canonical answer to ad-hoc question. Instead, they always cater for the OP to the minute detail. Not to mention that competitive approach, when a question gets half a dozen different answers in 5 minutes. Which could be good for ad-hoc questions, but turns into a great disservice when it comes to canonical answers. 

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

And for the best results, achievement of these goals is better to be physically separated. Ad-hoc questions could be answered in a forum-like mode, a dialodute that allows both parties to add code or images, to answer each other's questions. Although Stack Overflow takes pride for being not a forum, there is nothing to boast about. Many questions simply *cannot* be answered without a discussion. Why not to make it comfortable for all parties? 

At the same time, canonical answers are best to be made more like Wikipedia articles, with a separate "Discussion" tab, and being specifically written, including a correct title, a generic question and a canonical answer, that features a good practice first and any shortcuts (if any) later, with drawbacks well explained.

Such a separation will solve many problems at once:

- canonical answers won't be distracted by irrelevant details or noise (which was the initial goal when Stack Overflow claimed itself "not a forum")
- ad-hoc questions will be answered in a really non-judging, friendly and convenient environment
- new participants will earn as much reputation as they want

---

I am strongly convinced that the idea is viable and it can revive Stack Overflow. But surely I overlooked many important issues, because it's hard to consider all edge cases from just a single point of view. That's why I am posting here on the Meta site of Stack Overflow and looking for feedback. The only outcome I refuse to accept is leaving everything as is. Simply because it's a road to decline. And in this case someone else will inevitably create another Q&A site, where anyone can ask a programming question without being reproached for having a problem of not that global calibre.
