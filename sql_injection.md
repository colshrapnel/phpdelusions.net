*Disclaimer: My English is far from perfect and this text has been written when it was even worse. If you can't stand such poor grammar and can afford to do a bit of proofreading, [here is the source on GitHub](https://github.com/colshrapnel/phpdelusions.net/blob/master/sql_injection.md), all pull requests to which will be accepted with gratitude.*

In this article I will try to explain the nature of SQL injection; show how to make your queries 100% safe; and dispel numerous delusions, superstitions and bad practices related to the topic of SQL Injection prevention.

###Don't panic.#dontpanic

Honestly, there is not a single reason to panic or to be even worried. All you need is to get rid of some old superstitions and learn a few simple rules to make all your queries safe and sound. Strictly speaking, you don't even need to protect from SQL injections at all! That is, no dedicated measures intended exclusively to protect from SQL injection, have to be taken. All you need is to **format your query** properly. It’s as simple as that. Don't believe me? Please read on.

###What is an SQL injection?#whatis

SQL Injection is an exploit of an improperly formatted SQL query.

The root of SQL injection is the mixing of code and data.     
In fact, an SQL query is *a program.* A fully legitimate program - just like our familiar PHP scripts. And so it happens that we are *creating this program dynamically,* adding data to this program on the fly. Naturally, this data may interfere with the program code and even alter it - and such an alteration would be the very SQL injection itself.

But such a thing can only happen if we don't format query parts properly. Let's take a look at a [canonical example](http://xkcd.com/327/),

    $name  = "Bobby';DROP TABLE users; -- ";
    $query = "SELECT * FROM users WHERE name='$name'";

which compiles into the malicious sequence

    SELECT * FROM users WHERE name='Bobby';DROP TABLE users; -- '

Call it an injection? Wrong. It's **an improperly formatted string literal.**  
Which, once properly formatted, won't harm anyone:

    SELECT * FROM users WHERE name='Bobby\';DROP TABLE users; -- '

Let's take another canonical example,

    $id    = "1; DROP TABLE users;"
    $id    = mysqli_real_escape_string($link, $id);
    $query = "SELECT * FROM users where id = $id";

with no less harmful result:

    SELECT * FROM users WHERE id =1;DROP TABLE users; -- '

Call it an injection again? Wrong yet again. It's **an improperly formatted numeric literal.** Be it properly formatted, an honest

    SELECT * FROM users where id = 1

statement would be positively harmless.

But the point is, **we need to format out queries anyway** - no matter if there is any danger or not. Say there was no Bobby Tables around, but an honest girl named `Sarah O'Hara` - who would never get into a class if we don’t format our query, simply because the statement

    INSERT INTO users SET name='Sarah O'Hara'

will cause an ordinary syntax error.

> So we have to format just for sake of it. Not for Bobby but for Sarah. That is the point.

While SQL injection is just a *consequence* of an improperly formatted query.

> Moreover, **all the danger is coming from the very statement in question:** zounds of PHP users still do believe that the notorious `mysqli_real_escape_string()` function's only purpose is "to protect SQL from injections" (by means of escaping some fictional "dangerous characters"). If only they knew the real purpose of this honest function, there would be no injections in the world! If only they were *formatting* their queries properly, instead of "protecting" them - they'd have real protection as a result.

So, to make our queries invulnerable, we need to format them properly and make such formatting **obligatory**. Not as just an occasional treatment at random, as it often happens, but as a strict, inviolable rule. And your queries will be perfectly safe just as a *side effect*.

###What are the formatting rules?#formatting

The truth is, formatting rules are not that easy and cannot be expressed in a single imperative. This is the most interesting part, because, in the mind of the average PHP user, SQL query is something homogeneous, something as solid as a PHP string literal that represents it. They [treat SQL as a solid medium](http://kunststube.net/encoding/), that require whatever all-embracing "SQL escaping" only. While in fact SQL query a *program*, just like a PHP script. A program with its distinct syntax for its distinct parts, and **each part require distinct formatting, inapplicable for the others!**

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
 * should be filtered out to make sure only numerical characters, a decimal delimiter, and a sign
3. Identifiers
 * have to be enclosed in backticks
 * special characters (frankly - the very delimiting backticks) have to be escaped
4. Operators and keywords. 
 * there are no special formatting rules for the keywords and operators beside the fact that they have to be legitimate SQL operators and keywords. So, they have to be *whitelisted*.

As you can see, there are *four* whole *sets* of rules, not just one single statement. So, one cannot stick to a magic chant like "escape your inputs" or "use prepared statements". 

"All right" - you would say - "I am following all these rules already. What's all the fuss is about"? All the fuss is about the *manual* formatting. In fact, you should never apply these rules manually, in the application code, but must have a mechanism that will do this formatting for you instead. Why?

###Why manual formatting is bad?#manual

Because it is manual. And manual means error prone. It depends on the programmer's skill, temper, mood, number of beers last night and so on. As a matter of fact, manual formatting is the very and the only reason for the most injection cases in the world. Why?

1. **Manual formatting can be incomplete.**
Let's take the Bobby Tables' case. It's a perfect example of incomplete formatting: a string we have added to the query was only quoted, but not escaped! While, as we just learned from the above, quoting and escaping should always go together (along with setting the proper encoding for the escaping function). But in a usual PHP application which does SQL string formatting separately (partly in the query and partly somewhere else), it is very likely that some part of formatting may be simply overlooked.

2. **Manual formatting can be applied to the wrong literal**.
Not a big deal as long as we are using *complete formatting* (as it will cause an immediate error which can be fixed in the development phase), but when combined with incomplete formatting it's a real disaster. There are hundreds of answers on the great site of Stack Overflow, suggesting **to escape identifiers** the same way as strings. Which would be completely useless and would cause an SQL injection.

3. **Manual formatting is essentially a non-obligatory measure**.
First of all, there is an obvious lack of attention case, where proper formatting can be simply forgotten. But there is a really weird case - many PHP users often *intentionally* refuse to apply any formatting, because up to this day they still separate the data to "clean" and "unclean", "user input" and "non-user input", etc. Thinking that a "safe" data don't need any formatting. Which is a plain nonsense - remember Sarah O'Hara. From the formatting point of view, it's the *destination* that matters. A developer has to mind the *type of SQL literal*, not the data source. Is it a string going to the query? It has to be formatted as a string then. No matter if it's from user input or just mysteriously appeared out of nowhere amidst the code execution.

4. **Manual formatting can be separated from the actual query execution by a considerable distance.**

The most underestimated and overlooked issue. Yet most essential of them all, as it can spoil all the other rules alone, if not followed.

Almost every PHP user is tempted to do all the "sanitization" in a single place, far away from the actual query execution, and such a wrong approach is a source of innumerable faults alone:

 - first of all, having no query at hand, one cannot tell what kind of SQL literal this a certain piece represents - and thus we have both formatting rules (1) and (2) violated at once.
 - having more than one place for sanitizing (it could be either a centralized facility or in-place formatting), we are calling for a disaster, as one developer would think it was done by another or was already made somewhere else, etc.
 - having more than one place for sanitizing, we're introducing another danger, of double-sanitizing data (say, one developer formatted it at the entry point and another - before query execution), which is not dangerous but make one's site look extremely unprofessional
 - premature formatting will spoil the source variable, making it unusable for anything else.

5. After all, manual formatting will always take extra space in the code, making it entangled and bloated.
All right, now you trust me that manual formatting is bad. What do we have to use instead? 

###Prepared statements.#prepared

Here I need to stop for a while to emphasize the important difference between the implementation of **native** prepared statements supported by the major DBMS and **the general idea of using a placeholder** to represent the actual data in the query. And to emphasize **real benefits** of a prepared statement.

The idea of a native prepared statement is smart and simple: the query and the data are sent to the server separated from each other, and thus there is no chance for them to interfere. Which makes SQL injection outright impossible. But at the same time, native implementation has its limitations, as it supports only two kinds of literals (strings and numbers, namely) which **renders them insufficient and insecure for the real-life usage.**

There are also some wrong statements about native prepared statements:

- they are "faster". **Not in PHP**, which simply won't let you reuse a prepared statement between separate calls. While repeated queries within the same instance occurred too seldom to talk about. 
- they are "safer". This is partially true, but not because they are *native*, but because they are *prepared statements* - as opposed to manual formatting.

And here we came to the main point of the whole article: the general idea of creating an SQL query out of constant part and placeholders, which will be substituted with actual data, which will be **automatically formatted** is indeed a Holy Grail we were looking for.    

The main and most essential benefit of prepared statements is the elimination of all the dangers of manual formatting:

- a prepared statement does the complete formatting - all without programmer's intervention! Just fire and forget.
- a prepared statement does the adequate formatting (as long as we're binding our data using the proper type)
- a prepared statement makes the formatting inviolable!
- a prepared statement does the formatting in the only proper place - right before query execution.

This is why manual formatting is so much despised nowadays and prepared statements are so honored.

There are two additional but non-essential benefits of using prepared statements:

- a prepared statement doesn't spoil the source data which can be used safely somewhere else: shown back in the browser, stored in a cookie, etc.
- a programmer who is lazy enough can make their code dramatically shorter by means of using prepared statements (however, the opposite is true too - a diligent user can write a code for the simple insert of the size of an average novel).

So, "nativeness" of a prepared statement is not that essential, as it proven by PDO, which can just *emulate* a prepared statement, sending the regular query to the server at once, substituting placeholders with the actual data, if `PDO::ATTR_EMULATE_PREPARES` [configuration variable](http://php.net/manual/en/pdo.setattribute.php) is set to `TRUE`. But the data gets **properly formatted** in this case - and therefore this approach is equally safe!

Moreover, **even with the old MySQL extension** we can use prepared statements all right! Here is a small function that can offer rock-solid security with this old good extension:

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

> Here I need to stop again to make a very important statement: we need to distinguish a **constant** query element from a **dynamical** one. Obviously, our primary concern has to be dynamical parts, just because of their very dynamic nature. While constant value cannot be spoiled by design, and whatever formatting issue can be fixed at development phase, dynamical query element is a distinct matter. Due to its variable nature, **we never can tell if it contains a valid value, or not.** This is why it is so important to use placeholders for all the dynamical query parts.

"All right" - says you - "I am using prepared statements all the way already. And so what?" Be honest - you aren't. 
<a name="6">&nbsp;</a>
###One Step Beyond.#more

In fact, our queries sometimes are not as primitive as primary key lookups. Sometimes we have to make them even more dynamical, adding identifiers or whatever complex structures, like arrays. What regular drivers offer to solve this problem? 

**Nothing.**

For the identifier, it would be nothing else... **but old good manual formatting:**

    $field = "`".str_replace("`","``",$field)."`";
    $sql   = "SELECT * FROM t ORDER BY $field";
    $data  = $db->query($sql)->fetchAll();
        
For the arrays, it will be a whole *program* written to create a query on the fly:

    $ids = array(1,2,3);
    $in  = str_repeat('?,', count($arr) - 1) . '?';
    $sql = "SELECT * FROM table WHERE column IN ($in) AND category=?";
    $stm = $db->prepare($sql);
    $ids[] = $category; //adding another member to array
    $stm->execute($ids);
    $data = $stm->fetchAll();

And still, we have a variable interpolated in the query string, which makes me shivers in the back, although I am sure this code is safe. And it should make you shiver too! 

So, in all these cases **we are forced to fall back into the stone age of manual formatting!**

But as long as we have learned that manual formatting is bad and prepared statements are good, there is only one possible solution: **we have to implement placeholders for these types as well!**

Imagine we have a placeholder for the above cases. These two snippets become as simple and *safe* as any other code with prepared statements:

    $sql   = "SELECT * FROM t ORDER BY ?";
    $data  = $db->query($sql, $field)->fetchAll();

    $stm = $db->prepare("SELECT * FROM table WHERE column IN (?) AND category=?");
    $stm->execute([$ids, $category]);
    $data = $stm->fetchAll();

So, here can we make be but one conclusion: regular drivers **have** to be extended to support a wider range of data types to be bound. Having devised an idea of new types, we can think of what these types can be:

- identifier placeholder (single identifier)
- identifier list (comma separated identifiers) 
- integer list (comma separated integers)
- strings list (comma separated strings)
- the special SET type consists of a comma-separated `idientifier=string value` pairs
- you name it

Quite a lot, in addition to existing types.
<a name="7">&nbsp;</a>
###One little trick.#typehinted

So far so good. But here we face another problem. Even with regular types sometimes we need to set data type explicitly, to let driver understand how to format this particular value. A classical example for PDO:

    $stm = $db->prepare('SELECT * FROM table LIMIT ?, ?');
    $stm->execute([1,2]);

won't work in emulation mode, as PDO will format data as strings, whose aren't allowed in this query part. It isn't much a trouble though, as most of time default string formatting is all right:

    $stm = $db->prepare('SELECT * FROM t WHERE id=? AND email=?');
    $stm->execute([$id,$email]);

But with our new complex types, there is no way to use this approach anymore. Because, for example, an identifier can never be bound as a string. So, we have to always set placeholder type explicitly. And it makes a problem: although both PDO and Mysqli offer their own solutions, I wouldn't call any of them viable.

- with PDO's tons of rows with `bindValue` our code is no better than old `mysql_real_escape_string` approach in terms of code size and a number of repetitions. 
- mysqli's bind_param() is a nightmare when you have to bind a variable number of values.
- the worst part: all this manual binding makes *application* code bloated, as we cannot encapsulate these bindings into some internals.

So, we need a better solution again. And here it is:

####to mark placeholder with its type!

Not quite a fresh idea, I have to admit. Well-known `printf()` function has been using this very principle since Unix Epoch: `%s` will be formatted as a string, `%d` as a digit and so on. So, we only have to borrow that brilliant idea for our needs.

To solve all the problems, we need to extend a regular driver with simple *parser* which will parse a query with type-hinted placeholders, extract type information and use it to format a value.

I am not the first to use this approach with DBAL either. There is [Dibi](http://dibiphp.com/),  [DBSimple](http://en.dklab.ru/lib/DbSimple/) or [NikiC's PDO wrapper](https://github.com/nikic/DB) and some other examples. But it seems to me that their authors underestimated the significance of type-tinted placeholder, taking it as some sort of syntax sugar, not as essential and cornerstone feature, which it has to be.

So, I endeavored my own implementation, to emphasize the idea, and called it [SafeMysql](https://github.com/colshrapnel/safemysql), because type-hinted placeholders make it indeed safer than the regular approach. It's also aimed for better usability, to make code cleaner and shorter.

Although there are some improvements still pending for implementation (such as native placeholders support; named placeholders (like in PDO); un-typed default placeholder treated as a string; a couple another data types to support), it's already a complete solution that can be used in any production code.
<a name="8">&nbsp;</a>
###Exception that proves the rule.#whitelist

Unfortunately, a prepared statement is not a silver bullet too, and cannot offer 100% protection alone. There are two cases where it's either not enough or not applicable at all:

1. Remember the last kind of query literals from the section #3 list of formatting rules: SQL keywords. There is no way to format them.
2. A somewhat tricky case, when we are creating our *list of identifiers* dynamically, based on user input, and there could be fields a user isn't allowed to. The very common case is creating an insert query dynamically based on the keys and values of the `$_POST` array. Although we can format both properly, there is still a possibility for some field like 'admin' or 'permissions' that can be set by site admin only. And these fields should be never allowed from user input. 

And to solve these both cases, we have to implement another approach which is called *whitelisting*.
In this case, every dynamic parameter has to be pre-written in your script already, and all the possible values have to be chosen from that set.    
For example, to do dynamic ordering:

    $orders  = array("name","price","qty"); //field names
    $key     = array_search($_GET['sort'],$orders)); // see if we have such a name
    $orderby = $orders[$key]; //if not, first one will be set automatically. smart enough :)
    $query   = "SELECT * FROM `table` ORDER BY $orderby"; //value is safe

For keywords the rules are same, but of course there is no formatting available - thus, only whitelisting is possible and ought to be used:

    $dir = $_GET['dir'] == 'DESC' ? 'DESC' : 'ASC'; 
    $sql = "SELECT * FROM t ORDER BY field $dir"; //value is safe

Note that aforementioned SafeMysql lib supports two functions for whitelisting, one for key=>value arrays and one for single keywords, but now I am thinking of making these functions less advisory but more strict in usage.
<a name="9">&nbsp;</a>
###Dynamically built queries.#dynamic 

As this text pretends to be a complete guide for injection protection, I cannot avoid complex queries creation. A usual case of multiple-criteria search, for example. Or any other case where we have to have *arbitrary query parts* added or taken out from the query. There is no way to use placeholders in this case - so, we need another mechanism. 

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

But sometimes we have a query that is so complex that makes it to painful to write it using query builders. But either way we have to remember the only rule - all dynamical query parts are going into query via placeholders. And for a raw query we can use [a very smart trick](http://stackoverflow.com/a/11231629/285587):

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
- placeholders for every dynamical value

When followed, these rules will guarantee 100% protection.
<a name="app1">&nbsp;</a>
###Appendix 1. Vocabulary.#vocabulary

Almost everyone, who have fancy to talk on the topic, uses the wide range of words, hardly bothering to comprehend their meaning, or - worse of that - having their own idea on the meaning at all. 

Surely, the most notorious term is "escaping". Everyone is taking it as "making data clean", "making data safe", "escaping dangerous characters". While all these terms are essentially vague and ambiguous. At the same time, in the mind of average PHP user, "escaping" is strongly connected to the old `mysql_real_escape_thing`, so they have all these matters intermixed, resulting in the oldest and most callous of PHP superstitions - "`*_escape_string` prevents from injection attack" while we already learned it is far from it.  

Instead, the only proper term should be "formatting". While "escaping" should designate only particular part of string formatting. 

The same goes for every other word from the list of "sanitizing", "filtering", "prepared statements" and the like. Think of the meaning (or, rather, if it has any certain meaning at all) before using a word.

Even "SQL injection" term itself is extremely ambiguous.  It seems most users take injection literally as adding a second query as shown in the Bobby Tables example. And thus trying to protect by means of forbidding multiple query execution (which would be useless rubbish, of course). Somewhat similar delusion is shared by another lot of people, whose belief is that only DML queries can be considered as injection prone (and thus they don't care for injections in SELECT queries at all).

So, reading recommendations from various sources, always keep in mind that you have to understand the terminology and - more than that - understand what the author meant (and if there is *any* particular meaning at all).
<a name="app2">&nbsp;</a>
###Appendix 2. How to protect from of injection of [xxx] type.#otherinjections 

You may be heard of different kinds of injections - "blind", "time-delay","second order" and thousands of others. One has to understand that all these aren't different ways to *perform* an injection, but just different ways to *exploit* it. While there is only one way to perform an injection - to break query integrity. So, if you can keep query integrity, you'll be safe against all thousands of different "kinds" of injections all at once. And to keep the query integrity you have just to format query literals properly.
<a name="app3">&nbsp;</a>
###Appendix 3. False measures and bad practices.#badpractices

1. [**Escaping user Input.**](https://www.owasp.org/index.php/SQL_Injection_Prevention_Cheat_Sheet#Defense_Option_3:_Escaping_All_User_Supplied_Input) This is the King. A grave delusion, still shared by many PHP users (and even OWASP, as you can see). Consists of two parts: "escaping" and "user input":

 - escaping: as we noted above, it does only *part* of the job, for only *one* type of SQL literal. And when used alone or not in the proper place is a sure call for disaster.
 - user input: there should be no such words in the context of injection protection. Every variable is potentially dangerous - no matter of the source! Or, in other words - every variable have to be properly formatted to be put into query - no matter the source again. It's the destination that matters. The moment a developer starts to separate the sheep from the goat, he does his first step to disaster.
 - moreover, even the wording suggests bulk escaping at the entry point, resembling the very `magic quotes` feature - already despised, deprecated and removed from the language.

2. **magic quotes** - the very material incarnation of the above principle. Thank goodness, it's already removed from the language. 

3. **data validation**. One have to understand, that input (in the meaning of user input) data validation has absolutely nothing to do with SQL. Really. No validation rule can help against SQL injection if a free-form text is allowed. Yet we have to format our SQL despite any validations anyway - remember Sarah O'Hara who bears a name which is perfectly valid from the user input point of view. Also, remember that **validation rules may change**.

4. **htmlspecialchars** (and also `filter_var()`, `strip_tags()` and the like).   
Folks. It's **HTML** special characters encoding if you didn't notice yet. It has absolutely nothing to do with **SQL.** It helps nothing in the matter, and should never be used in the context of SQL injection protection. It's absolutely inapplicable for SQL, and cannot protect your query even if used as a string escaping function. Leave it for other parts of your application.    
Also, please understand that SQL formatting should never ever alter the data. Then you put your jewelry in a safe, you want it back **intact**, not some parts amended or replaced "for your own safety!". Same here. A database is intended to store your data, not to "protect" it. And it is essential to store the exact data you want back (means your silly `base64` attempt is wrong as well, by the way). 

5. **Universal** "clean'em all" **sanitization function.**     
Such a function just should have never existed. First, there are too much different contexts/mediums our data could be used in (SQL query, HTML code, JS code, JSON string, etc. etc. etc.) each require different formatting. So you just cannot tell beforehand what kind of formatting/sanitization will be required. Moreover, there are different literals even in a single medium, which, in turn, require it's own formatting too. Which just makes it impossible to create a single all-embracing function to format 'em all at once, without the risk of spoiling the source data yet leaving huge security holes at the same time.

6. **Filtering out malicious characters and sentences.**     
This one is simple - it's an absolutely imaginary measure. It fails the reality check a big one. Nobody ever used it on a more-or-less viable site. Just because it will spoil the user experience. 

7. **Stored procedures.**   
NOT in the web-development. This suggestion is from another realm, where one would have a fancy of limiting database users to a set of stored procedures. It is more like a general security measure, pointed against local users, rather than against an intruder. Anyway, the approach is just inviable in the web development. It can add nothing to what prepared statements already offer, yet in a way inconvenient and toilsome way. 

8. **Separate DB accounts** for running SELECT and DML queries. Again not a protection measure, but a just a [worthless] attempt to soften the consequences of the already performed attack. Worthless because of SELECT-based injection as a disaster alone. And of course useless because we are protected already by formatting our queries properly.
<a name="app4">&nbsp;</a>
###Appendix 4. ORMs and Query Builders.#orms

Although this article is on the raw SQL queries only, in the modern applications you may find not a trace of them. Because all SQL is hidden behind the scenes by an ORM or a Query Builder. If your application is of such kind - you're lucky. However, it doesn't make this article less significant due to the following reasons:

 - First of all, even ORMs and Query Builders have to be written by someone. And prepared statements will make such writing simpler and safer.  
 - Speaking of ORM, as long as you can stick to its methods, you're all right. Unfortunately, in the real life you can't use it all the way - sometimes you have to run raw queries as well. So, always have a good library that supports type-hinted placeholders along with your ORM.    
 - Query Builders are using placeholders already - so, they would only benefit from adding some new placeholder types. 

###TL;DR#tldr

1. There is no such thing like injection but improperly formatted query only
2. To make formatting inviolable we have to use prepared statements
3. Prepared statements have to support much more data types than simple strings and numbers
4. To make formatting usable we need to use type-hinted placeholders
5. In case when formatting is inapplicable, white-listing have to be used. 
