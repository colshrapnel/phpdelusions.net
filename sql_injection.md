*Disclaimer: My English is far from perfect and this text has been written when it was even worse. If you can't stand such poor grammar and can afford to do a bit of proofreading, [here is the source on Github](https://github.com/colshrapnel/phpdelusions.net/blob/master/sql_injection.md), all pull requests to which will be accepted with gratitude.*

In this article I will explain the nature of SQL injection; show how to make your queries 100% safe; and dispel numerous delusions, superstitions, and bad practices related to the topic of SQL Injection prevention.

###Don't panic.#dontpanic

Honestly, there is not a single reason to panic or to even be worried. All you need to do is get rid of some old superstitions and learn a few simple rules to make your queries safe and sound. Strictly speaking, you don't even need to protect from SQL injections at all! That is: no dedicated measures intended exclusively to protect from SQL injection have to be taken. All you need is to **format your query** properly. It’s as simple as that. Don't believe me? Please read on.

###What is an SQL injection?#whatis

SQL Injection is an exploit of an improperly formatted SQL query.

The root of SQL injection is the mixing of code and data.     
In fact, an SQL query is *a program.* A fully legitimate program - just like our familiar PHP scripts. And so it happens that we are *creating this program dynamically,* adding data to this program on the fly. Naturally, this data may interfere with the program code and even alter it - and such an alteration would be the very SQL injection itself.

Such a thing can only happen if we don't format query parts properly. Let's take a look at a [canonical example](http://xkcd.com/327/),

    $name  = "Bobby';DROP TABLE users; -- ";
    $query = "SELECT * FROM users WHERE name='$name'";

which compiles into the malicious sequence

    SELECT * FROM users WHERE name='Bobby';DROP TABLE users; -- '

Call it an injection? Wrong. It is an **improperly formatted string literal.**  
Which, once properly formatted, won't  cause any harm:

    SELECT * FROM users WHERE name='Bobby\';DROP TABLE users; -- '

Let's take another canonical example,

    $id    = "1; DROP TABLE users;"
    $id    = mysqli_real_escape_string($link, $id);
    $query = "SELECT * FROM users where id = $id";

with no less harmful result:

    SELECT * FROM users WHERE id ='1;DROP TABLE users; -- '

Call it an injection again? Wrong yet again. It is an **improperly formatted numeric literal.** WHen it is properly formatted, the honest statement:

    SELECT * FROM users where id = 1

would be positively harmless.

But the point is: **we need to format our queries properly** - no matter if there is any danger or not. Say there a girl named *Sarah O'Hara*. Her record would never be added if we do not format our query properly, simply because the statement

    INSERT INTO users SET name='Sarah O'Hara'

will cause an ordinary syntax error.

> So we have to properly format the name entry. Not for Bobby but for Sarah. That is the point. If we have to do it for one name, we have to do it for all of them.

What this tells us is that what is called an SQL injection is actually just a *consequence* of an improperly formatted query.

> Moreover, **all the danger is coming from the very statement in question:** many PHP users still believe the only purpose of the notorious `mysqli_real_escape_string()` function is "to protect SQL from injections" (by means of escaping some "dangerous characters"). If only they knew the real purpose of this function, there would be no injections in the world! If only they were *formatting* their queries properly, instead of "protecting" them, they would have real protection as a result.

So, to make our queries invulnerable, we need to format them properly and make such formatting **obligatory**. Not as just an occasional treatment at random, as it often happens, but as a strict, inviolable rule. And your queries will be perfectly safe just as a *side effect*.

###What are the formatting rules?#formatting

The truth is, formatting rules are not that easy, and they certainly cannot be expressed in a single imperative. This is the most interesting part, because, in the mind of the average PHP user, an SQL query is something homogeneous, something as solid as a PHP string literal that represents it. They [treat SQL as a solid medium](http://kunststube.net/encoding/) that requires little more than the all-encompasing "SQL escaping" process. While in reality, an SQL query is a complete *program*, just like a PHP script. A program with its distinct syntax for its various parts, and **each part requiring distinct formatting, inapplicable for the others!**

For Mysql it would be:

1. Strings
 * have to be added via a native prepared statement     
or
 * have to be enclosed in quotes
 * special characters (frankly - the very delimiting quotes) have to be escaped
 * a proper client encoding has to be set   
or
 * could be [hex-encoded](https://dev.mysql.com/doc/refman/5.0/en/hexadecimal-literals.html)
2. Numbers
 * have to be added via a native prepared statement     
or
 * should be passed through a potentially complex set of filters to make sure it contains only actual numerical characters
3. Identifiers
 * have to be enclosed in backticks
 * special characters (frankly - the very delimiting backticks) have to be escaped
4. Operators and keywords. 
 * there are no special formatting rules for the keywords and operators beside the fact that they have to be legitimate SQL operators and keywords. So, they have to be *whitelisted*.

As you can see, there are *four* whole *sets* of rules, not just one single statement. So, one cannot stick to a magic incantation like "escape your inputs" or "use prepared statements". 

"All right," you could say, "I am following all these rules already. What's all the fuss is about"?

All the fuss is about the *manual* formatting. In fact, you should never apply these rules manually, in the application code, but must have an automated mechanism that will do this formatting for you instead. Why?

###Why manual formatting is bad?#manual

Because it is manual. And manual means error prone. It depends on the programmer's skill, temperament, mood, number of beers last night, and so on. As a matter of fact, manual formatting is **THE** reason for the majority of injection cases in the world. Why?

1. **Manual formatting can be incomplete.**
Let's take the Bobby Tables' case. It is a perfect example of incomplete formatting: a string we have added to the query was only quoted, but not escaped! While, as we just learned from the above, quoting and escaping should always go together (along with setting the proper encoding for the escaping function). But in a usual PHP application which does SQL string formatting separately (partly in the query and partly somewhere else), it is very likely that some part of formatting may be simply overlooked.

2. **Manual formatting can be applied to the wrong literal**.
Not a big deal as long as we are using *complete formatting* (as it will cause an immediate error which can be fixed in the development phase), but when combined with incomplete formatting it's a real disaster. There are hundreds of answers on the great site of Stack Overflow, suggesting **to escape identifiers** the same way as strings. Which would be completely useless and could easily cause an SQL injection of its own.

3. **Manual formatting is essentially a non-obligatory measure**.
First, there is an obvious lack of attention, where proper formatting can be simply forgotten or skipped.

But there is a really weird case: many PHP programmers often *intentionally* refuse to apply any formatting, because, even to this day, they still separate the data to "clean" and "unclean", "user input" and "non-user input", etc. Thinking that "safe" data does not need any formatting. This is a plain nonsense. Remember *Sarah O'Hara*. From the formatting point of view, it's the *destination* that matters. A developer has to mind the *type of SQL literal*, not the data source! Is it a string going to the query? It has to be formatted as a string then. No matter if it's from user input or just mysteriously appeared out of nowhere amidst the code execution.

4. **Manual formatting can be separated from the actual query execution by a considerable distance.**
THis might be the most underestimated and overlooked issue. Yet most essential of them all, as it can spoil all the other rules if you do not keep it in mind.

Almost every PHP user is tempted to do all the "sanitization" in a single place, far away from the actual query execution, and such an approach is the source of innumerable potential fault:

 - first of all, having no query at hand, one cannot tell what kind of SQL literal this certain piece represents. Thus we have both formatting rules (1) and (2) violated.
 - having more than one place for sanitizing (it could be either a centralized facility or in-place formatting), we are calling for a disaster, as one developer would think it was done by another or was already performed somewhere else, etc.
 - having more than one place for sanitizing, we're introducing another danger, of double-sanitizing data. For instance, one developer formatted it at the entry point and another waited until just before query execution. This might not be dangerous, but could make the site look extremely unprofessional
 - premature formatting will spoil the source variable, making it unusable for anything else.

5. After all, manual formatting will always take extra space in the code, making it entangled and bloated.

All right, now you trust me that manual formatting is bad. What do we have to use instead? 

###Prepared statements.#prepared

Here I need to stop for a moment to emphasize the important difference between the implementation of **native** prepared statements supported by the major DBMS, and **the general idea of using a placeholder** to represent the actual data in the query. And then to emphasize **real benefits** of a prepared statement.

The idea of a **native** prepared statement is smart and simple: the query and the data are sent to the server separated from each other, and thus there is a more limited chance for them to interfere with each other. This makes SQL injection pretty much impossible. But at the same time, the **native** implementation has its limitations. It supports only two kinds of literals (strings and numbers) which **renders them insufficient and insecure for the real-life usage.**

There are also some wrong statements about native prepared statements:

- they are "faster". **Not in PHP**, which simply won't let you reuse a prepared statement between separate calls. While repeated queries within the same instance occurred too seldom to talk about.
- they are "safer". This is partially true, but not because they are **native**, but because they are *prepared statements*, as opposed to manual formatting.

Here we come to the main point of the whole article: the idea of creating an SQL query out of built-in functionality and placeholders, which will be substituted with actual data, which will then be **automatically formatted**. This is indeed a Holy Grail we were looking for.    

The main and most essential benefit of prepared statements is the elimination of all the dangers of manual formatting:

- a prepared statement does the complete formatting, all without programmer's intervention! Just fire and forget.
- a prepared statement does adequate formatting, as long as we're binding our data using the proper type.
- a prepared statement makes the formatting inviolable!
- a prepared statement does the formatting in the only proper place, which is right before query execution.

This is why manual formatting is so much despised, and prepared statements are so honored.

There are two additional but non-essential benefits of using properly formatted prepared statements:

- a prepared statement doesn't spoil the source data which can be used safely somewhere else: shown back in the browser, stored in a cookie, etc.
- a programmer who is lazy enough can make their code dramatically shorter by means of using prepared statements (however, the opposite is also true, that a diligent user can write code for the simple insert of the size of an average novel.

So, "nativeness" of a prepared statement is not that essential, as is proven by PDO, which can just *emulate* a prepared statement, sending the regular query to the server at once, substituting placeholders with the actual data, if `PDO::ATTR_EMULATE_PREPARES` [configuration variable](http://php.net/manual/en/pdo.setattribute.php) is set to `TRUE`. But the data gets **properly formatted** in this case, and therefore this approach is equally safe!

Moreover, **even with the old MySQL extension** we can use prepared statements all right! Here is a small function that can offer rock-solid security with this good old extension:

    function paraQuery()
    {
        $args  = func_get_args();
        $query = array_shift($args);
        $query = str_replace("%s","'%s'",$query); 
    
        foreach ($args as $key => $val)
        {
            $args[$key] = mysql_real_escape_string($val);
        }
    
        $query  = vsprintf($query, $args);
        $result = mysql_query($query);
        if (!$result)
        {
            throw new Exception(mysql_error()." [$query]");
        }
        return $result;
    }
    
    $query  = "SELECT * FROM table where a=%s AND b LIKE %s LIMIT %d";
    $result = paraQuery($query, $a, "%$b%", $limit);

Look - everything is parameterized and safe, at least to the degree PDO can offer.

So the all-embracing rule for the protection:

####Every dynamical element should go into query via **placeholder**

> Here I need to stop again to make a very important statement: we need to distinguish a **constant** query element from a **dynamic** one. Obviously, our primary concern has to be dynamic parts, just because of their very nature. While constant value cannot be spoiled by design, and whatever formatting issue can be fixed at development phase, dynamic query elements are another matter. Due to its nature, **we never can tell if it contains a valid value, or not**. This is why it is so important to use placeholders for all the dynamic query fields.

"All right," you might say, "I am using prepared statements all the time already. So what?" Be honest, you aren't. 

<a name="6">&nbsp;</a>
###One Step Beyond.#more

In fact, our queries sometimes are not as primitive as primary key lookups. Sometimes we have to make them even more dynamic, adding identifiers or some fairly complex structures, like arrays. What do regular drivers offer to solve this problem? 

**Nothing.**

For the identifier, it would be nothing else... **but good old manual formatting:**

    $field = "`".str_replace("`","``",$field)."`";
    $sql   = "SELECT * FROM t ORDER BY $field";
    $data  = $db->query($sql)->fetchAll();
        
For arrays, it will be a whole *program* written to create a query on the fly:

    $ids = array(1,2,3);
    $in  = str_repeat('?,', count($arr) - 1) . '?';
    $sql = "SELECT * FROM table WHERE column IN ($in) AND category=?";
    $stm = $db->prepare($sql);
    $ids[] = $category; //adding another member to array
    $stm->execute($ids);
    $data = $stm->fetchAll();

And still, we have a variable interpolated in the query string, which sends shivers up my back, even though I am sure this code is safe. Frankly, this example should make you shiver, too! 

So, in all these cases **we are forced to fall back into the stone age of manual formatting**!

But as long as we have learned that manual formatting is bad and prepared statements are good, there is only one possible solution: **we have to implement placeholders for these types as well!**

Imagine we have a placeholder for the above case. These two snippets become as simple and *safe* as any other code with prepared statements:

    $sql   = "SELECT * FROM t ORDER BY ?";
    $data  = $db->query($sql, $field)->fetchAll();

    $stm = $db->prepare("SELECT * FROM table WHERE column IN (?) AND category=?");
    $stm->execute([$ids, $category]);
    $data = $stm->fetchAll();

So, here can we make but one conclusion: regular SQL drivers **have** to be extended to support a wider range of data types to be bound. Having decided to do this, we can think of what these types can be:

- identifier placeholder (single identifier)
- identifier list (comma separated identifiers) 
- integer list (comma separated integers)
- strings list (comma separated strings)
- the special SET type consists of a comma-separated `idientifier=string value` pairs
- you name it

Quite a lot, in addition to existing types.

<a name="7">&nbsp;</a>
###One little trick.#typehinted

So far so good. But here we face another problem. Even with regular types sometimes we need to set the data type explicitly, to let the driver explicitly understand how to format this particular value. A classical example for PDO:

    $stm = $db->prepare('SELECT * FROM table LIMIT ?, ?');
    $stm->execute([1,2]);

This won't work in emulation mode, as PDO will format the data values as strings, which are not allowed in this part of the query. It isn't much a trouble though, as most of time default string formatting is all right:

    $stm = $db->prepare('SELECT * FROM t WHERE id=? AND email=?');
    $stm->execute([$id,$email]);

But with our newly designed complex data types, there is no way to use this approach any more. Because, for example, an identifier can never be bound as a simple string. So, we must always set the placeholder type explicitly. This causes another problem: both PDO and Mysqli offer their own solutions, I won't call either of them viable.

- with PDO's tons of rows with `bindValue` our code is no better than the old `mysql_real_escape_string` approach in terms of code size and the number of repetitions. 
- mysqli's bind_param() is a nightmare when you have to bind a variable number of values.
- the worst part: all this manual binding makes *application* code bloated, as we cannot encapsulate these bindings into some internal mechanism.

So, we need a better solution again. And here it is:

####to mark placeholder with its type!

Not quite a fresh idea, I have to admit. Well-known `printf()` function has been using this very principle since the start of the Unix Epoch: `%s` will be formatted as a string, `%d` as a digit and so on. So, we only have to borrow that brilliant idea for our needs.

To solve the problem here, we need to extend a regular driver with simple *parser* which will parse a query with type-hinted placeholders, extract type information and use it to format a value.

I am certainly not the first to use this approach with DBAL either. There is [Dibi](http://dibiphp.com/), [DBSimple](http://en.dklab.ru/lib/DbSimple/), or [NikiC's PDO wrapper](https://github.com/nikic/DB), as well as others. But it seems that their authors underestimated the significance of type-hinted placeholders, taking it as some sort of syntactical sugar, not as the essential, cornerstone feature, it has to be.

So, I endeavored to create my own implementation, to emphasize the idea. I call it [SafeMysql](https://github.com/colshrapnel/safemysql). This is because type-hinted placeholders make it indeed safer than the regular approach. It's also aimed for better usability, to make code cleaner and shorter.

There are some improvements still pending for implementation (such as native placeholder support; named placeholders (like in PDO); un-typed default placeholder treated as a string; a couple of other data types to support, it is already a complete solution that can be used in any production code.

<a name="8">&nbsp;</a>
###Exception that proves the rule.#whitelist

Unfortunately, a prepared statement is not a silver bullet, and cannot offer 100% protection alone. There are two cases where it is either not enough or not applicable at all:

1. Remember the last kind of query literals from the section #3 list of formatting rules: SQL keywords. There is no way to format them.
2. A somewhat tricky case, when we are creating our *list of identifiers* dynamically, based on user input, and there could be fields a user is not allowed to use. The very common case is creating an insert query dynamically based on the keys and values of the `$_POST` array. Although we can format both properly, there is still a real possibility that some fields (like 'admin' or 'permissions') that can be set by the site admin only. These fields should be never allowed from user input.

To solve these cases, we have to implement another approach which is called *whitelisting*.

In this case, every dynamic parameter has to be pre-written in your script already, and all the possible values have to be chosen from that set.

For example, to do dynamic ordering:

    $orders  = array("name","price","qty"); //field names
    $key     = array_search($_GET['sort'],$orders)); // see if we have such a name
    $orderby = $orders[$key]; //if not, first one will be set automatically. smart enough :)
    $query   = "SELECT * FROM `table` ORDER BY $orderby"; //value is safe

For keywords the rules are the same, but of course there is no formatting available. Thus only whitelisting is possible and ought to be used:

    $dir = $_GET['dir'] == 'DESC' ? 'DESC' : 'ASC'; 
    $sql = "SELECT * FROM t ORDER BY field $dir"; //value is safe

Note that the aforementioned SafeMysql library supports two functions for whitelisting, one for key=>value arrays and one for single keywords, but now I am thinking of making these functions less advisory, and more strictly required in usage.

<a name="9">&nbsp;</a>
###Dynamically built queries.#dynamic 

As this text intends to be a complete guide for injection protection, I cannot avoid complex query creation. A usual case of multiple-criteria search, for example. Or any other case where we have to have *arbitrary query parts* added or taken out from the query. There is no way to use placeholders in this case. So, we need another mechanism. 

First, a **query builder** is a king here. The exact case query builders were invented for

    $query = $users = DB::table('users')->select('*');
    if ($fname = input::get('first_name'))
    {
        $query->where('first_name = ?', $fname);
    }
    if ($lname = input::get('last_name'))
    {
        $query->where('last_name = ?', $lname);
    }
    // and so on
    $results = $query->get();

But sometimes we have a query that is so complex that it simply makes it to painful to write using query builders. Either way we have to remember the only rule: all dynamic query parts are going into the query via placeholders. For a raw query we can use [a very smart trick](http://stackoverflow.com/a/11231629/285587):

    SELECT * FROM people 
    WHERE (first_name = :first_name or :first_name is null)
    AND (last_name = :last_name or :last_name is null)
    AND (age = :age or :age is null)
    AND (sex = :sex or :sex is null)

Written this way, we only have to bind our variables, either contains a value or a NULL, to these placeholders. For the NULL values, the smart engine would just throw away their conditions, leaving only ones with values!

Either way, one has to bear in mind that the resulting query should be always built from only two sources - either constant part or a placeholder.

<a name="10">&nbsp;</a>
###Conclusion.#conclusion 

In short, we can formulate two simple rules:

Even dynamically created, an SQL query has to consist of 2 possible kinds of data only:

- constant parts hardcoded in the script
- placeholders for every dynamic value

When followed, these rules will guarantee 100% protection.

<a name="app1">&nbsp;</a>
###Appendix 1. Vocabulary.#vocabulary

Almost everyone who wants to talk on the topic, uses the wide range of words, hardly bothering to define their meaning, or,worse, have their own idea on the meaning. 

Surely, the worst term is "escaping." Everyone is taking it as "making data clean," "making data safe," or "protecting dangerous characters." All these are essentially vague and certainly ambiguous. At the same time, in the mind of your average PHP user, "escaping" is strongly connected to specific function, like the old `mysql_real_escape_thing`. So all of these concepts become intermixed, resulting in the oldest and most callous of PHP superstitions: "`*_escape_string` prevents an injection attac,k" which we have already learned it is far from reality.

Instead, the only proper term should be "formatting". While "escaping" should designate only a particular style of string formatting. 

The same goes for every other word from the list of "sanitizing", "filtering", "prepared statements" and the like. Think of the meaning (or, rather, if it has any certain meaning at all) before using a word.

Even the term "SQL injection," itself, is extremely ambiguous.  It seems most users take injection literally as adding a second query as shown in the Bobby Tables example. And thus trying to protect by means of forbidding multiple query execution (which would be useless rubbish, of course). A somewhat similar mistaken impression is shared by another group of people, whose belief is that only DML queries can be considered as injection prone (and thus they don't care for injections in SELECT queries at all).

So, reading recommendations from various sources, always keep in mind that you have to understand the terminology and, more than that, understand what the author meant (and if there is *any* particular meaning at all).

<a name="app2">&nbsp;</a>
###Appendix 2. How to protect from of injection of [xxx] type.#otherinjections 

You may have heard of different kinds of SQL injections with names like "blind," "time-delay," and "second order." We need to understand that all of these are not different ways to *perform* an SQL injection. They are just different ways to *exploit* it. When all is said and done, there is only *one way* to perform an SQL injection: to break the query's integrity.

If you can maintain the query integrity, you will be safe against all of the various "kinds" of SQL injections, all at once. To keep the query integrity you *MUST* format query literals properly.

<a name="app3">&nbsp;</a>
###Appendix 3. False measures and bad practices.#badpractices

1. [**Escaping user Input.**](https://www.owasp.org/index.php/SQL_Injection_Prevention_Cheat_Sheet#Defense_Option_3:_Escaping_All_User_Supplied_Input)

This is the equivalent of Hans Christian Anderson's story, *The Emperor's New Clothes*. It is a grave delusion, still shared by many, including PHP, developers. That includes OWASP, as you can see. This, as we have already discussed, consists of two parts: "escaping" and "user input":

 - escaping: as we noted above, it does only *part* of the job, mostly addressing only *part* of the world of SQL injection issues. And when used alone or not in the proper place is a sure call for disaster.
 - user input: there should be no such words in the context of SQL injection protection. Every variable is potentially dangerous, irrespectiver of the source! Or, in other words: every variable must be properly formatted to be put into query! 
 
 It is the *destination* that matters, not the *source*. The moment a developer looses sight of that fact, then the program begins to lose its quality. Even worse, though, the very *wording* suggests some type of bulk escaping at the entry point, resembling the very `magic quotes` feature, something which is already despised to the point it was deprecated and removed from the language.

2. **magic quotes** 

As was just stated, this is the very material incarnation of the above principle. Thank goodness it has already removed from the language. 

3. **data validation**. 

One has to understand that user input (as in data from a form) data validation has absolutely nothing to do with SQL or *fixing* SQL injection. Really.

Understand this one simple point: no validation rule can help against SQL injection if free-form text is allowed.

Yet we have to format our SQL query, despite any validation steps. Remember Sarah O'Hara who bears a name which is perfectly valid from the user input point of view. But there is another, equally important, point: **validation rules may change**. If you scatter your validation across the code, changes become a nightmare to update.

4. **htmlspecialchars** (and also `filter_var()`, `strip_tags()` and the like).   

Sometimes the name of a function is so obvious that we understand what it does immediately. But with time, and over use, we tend to lose sight of that. So, folks, let me remind you of the obvious: this does **HTML** special character encoding! Nothing more. It has absolutely *nothing* to do with **SQL**. It helps nothing in this matter, and should never be used in the context of SQL injection protection. It is absolutely inapplicable for SQL, and cannot protect your query even if used as a string escaping function. Leave it for other parts of your application.    

Also, please understand that SQL formatting should never ever alter the data. If it does, then you risk the very integrity of the data you are storing! You put data into a database so other programs can *use* it. If it is not intact, then it not usable. It is like you put your jewelry in a safe, you want it back **intact**, not some parts amended or replaced "for your own safety!". The same concept applies here. A database is a *storage* vehicle, not a *protection* one. It is essential to store the exact data you want back. Yes, this means the silly `base64` attempt is wrong as well, by the way. 

5. **Universal** "clean'em all" **sanitization function.**

Such a function should never exist. First, there are too many different contexts/mediums our data could be used in (SQL query, HTML code, JS code, JSON string, etc). Each requires completely different, and potentially unique, formatting. So you just cannot tell beforehand what kind of formatting/sanitization will be required. Moreover, there are different literals even in a single medium, which, in turn, require separate formatting. This makes it impossible to create a single all-embracing function to format everything at once, without the risk of spoiling the source data, and potentially leaving a huge security hole at the same time.

6. **Filtering out malicious characters and sentences.**     

This one is simple sounding, yet an absolutely imaginary measure. It fails the reality check in a big way. Companies like Yahoo! and Google employ thousands of analysts and editors attempting to do this. What makes us think we can write a "quick function" do pull it off? 

7. **Stored procedures.**

NOT in web-development. This suggestion is from another realm, where one would have a limited group of qyakufued database users who are relying on a well-defined and tested set of stored procedures. These are meant to build something akin to a general security measure pointed at local users, rather than a comprehensive solution working against an intruder. This approach is just invalid in the web development. It can add nothing to what prepared statements already offer, yet they are in the way, inconvenient, and toilsome. Furthermore, they are added by a database administrator, not the PHP developer, so their content and quality is unknown. 

8. **Separate DB accounts** for running SELECT and DML queries. 

Again, this is not a protection measure for web-development. It is a way for the database administrators to limit legitimate users from doing things they otherwise should not do. You cannot change your user information at work and increase your pay. In a perfect world, nobody would even think of doing that, but since we do not live in a perfect world, those kinds of safeguards are needed.

In the world of web-development, doing this is little more than a [worthless] attempt to soften the consequences of the attack that was already made. On one hand, it is worthless because any SELECT-based SQL injection attack as a disaster all on its own. On the other hand, it is useless because we should already be completely protected by fully formatting our queries properly.

<a name="app4">&nbsp;</a>
###Appendix 4. ORMs and Query Builders.#orms

Although this article is on the raw SQL queries only, in the modern applications you may find not a trace of them. Because all SQL is hidden behind the scenes by an ORM or a Query Builder. If your application is of this kind, you may be lucky. However, it doesn't make this article less significant due to the following reasons:

 - Even ORMs and Query Builders have to be written by someone. And prepared statements will make such writing simpler and safer.
 - Speaking of ORM, as long as you can stick to its methods, you're all right. Unfortunately, in the real life you can't use it in every instance. Frequently you have to run raw queries as well. So, always have a good library that supports type-hinted placeholders along with your ORM.
 - Query Builders are using placeholders already, so they would only benefit from adding some new placeholder types.
 - You are trusting someone elses code to do what you should be taking care of. That might not be a problem, but unless you do a comprehensive validation, you simply do not know. When you find out it failed is *after* your system has been hacked, which is, obviously, too late.

###TL;DR#tldr

1. There is no such thing like injection but improperly formatted query only
2. To make formatting inviolable we have to use prepared statements
3. Prepared statements have to support many more data types than simple strings and numbers
4. To make formatting usable we need to use type-hinted placeholders
5. In case when formatting is inapplicable, white-listing have to be used. 
