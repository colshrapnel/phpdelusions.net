During a decade of active participation on Stack Overflow I was able to determine a set of reasons that lead to the most frequent questions on Q&A sites. It turned out that by following a rather limited set of basic principles one can avoid a multitude of generic problems. 

Sadly, but these principles, although being universal, are almost as universally *ignored* or even *violated* in virtually every online tutorial. And I would say it's one of the biggest problems of online education. 

For some reason there is a huge gap between a code written "for the education" and a professionally written code. And surprisingly, the difference is not in the professional code being more complex but on the contrary - the educational code being more elaborate, at the same time being error-prone and insecure. 

But the irony is, following the professional practices doesn't make your code more complex! All you need to do is to learn a few basic principles and it will make your code tidy and secure!

I only have to add that the following set of rules is the generic right way of doing things. Of course there could be exceptions. But the point is, your first approach should be generic, not exceptional.

Although all language-specific details are given for PHP and MySQL, the principles are universal, and can be applied to any language.

###Search engines

Learn to use Google. Seriously. There are loads of existing information on the web. All you need is to pick it up. Here are some useful hints:

- work with the results. Do not give up after the first one.
- refine the search. Add certain terms, change phrasing or punctuation
- limit your search to the certain site, e.g. adding `site:stackoverflow.com` to your search phrase will limit the search to this site, or `site:reddit.com inurl:/r/php` to even a single subreddit!


###Security

is the biggest shame of PHP tutorials. It is not a rare exception but rather a *rule* that a PHP tutorial picked at random from Google, or an Udemy course, would straight up teach you how to make your code critically vulnerable to every possible attack out there. And all this despite the fact that basic security rules are really simple and a no-brainer to follow! The good news, lately  *some* tutorials managed to make it right at least with basic security.

Also, a big problem with security recommendations is that they are either uncertain (like "don't trust user input", "all data must be sanitized" etc) or make the wrong emphasis in the data source, not destination. But it's only *destination* that matters. Refer to the following list for the concrete recommendations:

- **SQL injection** is *completely mitigated* by following the two simple rules:
 - every *string* or a *number* that needs to be added to the query must be added through a *placeholder* and then a query executed using a prepared statement. See examples for [Mysqli](https://phpdelusions.net/mysqli_examples) or [PDO](https://phpdelusions.net/pdo_examples)
 - any other query part (such as a table/field name or a keyword) has to be filtered through a list of values *explicitly written in your code*
- **passwords** must be *hashed* using a *dedicated function*, [password_hash()](https://www.php.net/password_hash)
- all **HTML output** has to be encoded using [htmlspecialchars()](https://www.php.net/htmlspecialchars) (unless it's a deliberately HTML formatted text). All javascript data must be encoded with `json_encode()`. All data in the URL must be encoded using `urlencode()`
- a **system error message** should be never shown to a site user, being an invaluable feedback for a potential attacker
- a **cookie** can be forged and therefore should store either insignificant data or a cryptographically secure random identifier. For the secure storage use sessions.
- The **Location:** header doesn't stop the code execution and therefore must be followed by an obligatory `die;` call (if it's not the last line of the code)
- **CSRF** attack is mitigated by having a hidden field with a unique token that must be also stored in the session and these two must be compared on the form submit.
- when **uploading files**, a filename extension must be checked against a white list of allowed values. All mime type-based checks are utterly unreliable security-wise. Ideally, a file must be renamed and given a server-generated name and extension. Even better - uploaded to a distinct server with all scripting turned off. 

*Disclaimer:* of course this is not an exhaustive list of vulnerabilities but just a list of the most frequent issues in the learners' code. For the more in-depth information refer to the resources like OWASP.

All I have to add is that all those rules only work when followed **unconditionally**. One steps into abyss when starting to choose whether their data is "safe" or not, whether it must be protected or not. *Never judge the data by the source* but only by the *destination:* sending it to SQL? Then it must be prepared. Sending it into HTML? Then it must be encoded. And so on.

###Error reporting

is really a sore spot for the PHP community, endorsed - sadly - by none other than the official PHP manual and thousands of answers on Stack Overflow repeating the same bad practices again and again. Which makes even a smallest piece of code unnecessarily elaborate, unsuitable for the production and even potentially dangerous, leaking the sensitive information outside.

> **NB**. There must be a clear distinction between user interaction errors and system errors.
- user errors are not actually errors but just notifications, such as "Passwords did not match" that can and should be shown to a site user
- but real system errors, such as "Too many connections" MySQL error should be never shown to a site user. We are talking here of the system errors.

A lot of confusion is coming from the fact that every site has *two kinds of customers*: a programmer and a user, who require totally different treatment in regard of error messages:

- a programmer needs to see *every possible error* in the *full detail*
- whereas a site user must see *none*, being given just a *generic excuse page* instead

To make both satisfied at the same time, follow the principles below:

1. **Errors must be only thrown**, but never printed out unconditionally. PHP is damn good at displaying a thrown error - you don't need no special code to see the error message on the screen - not a single line! Just let errors to be thrown or throw them yourself, and then let PHP to handle the rest
2. Try to avoid checking for the errors manually
 - most PHP modules can be configured to throw errors automatically (Most notable are [PDO](https://phpdelusions.net/pdo#errors) and [Mysqli](https://phpdelusions.net/mysqli/error_reporting))
 - in case you need to check for the error manually, never echo it out. Instead, just *throw* it, using either `trigger_error()` or `throw new Exception()`
2. There must be always a distinction between **development and production modes**. 
 - however, such a distinction should never affect the code itself. Always write your code right away, without any conditions related to the server role
 - instead, it must be configured *globally*, affecting the whole site by means of setting *a single configuration option*
 - in the development mode PHP is usually configured to display errors right away
3. There is a **production mode**, folks. And it's drastically different from the setup you are accustomed for, being a sole user for your site while learning. In the production mode, 
 - not a single system error message must ever be shown to a site user
 - errors must be *logged* into a file inaccessible from outside, or handled otherwise but only available to a site admin/developer
 - for a site user, a generic excuse page must be shown
4. **Errors shouldn't be swept under the rug**. This includes not only the use of the infamous `@` operator but some other techniques that aren't percepted as being evil by the general audience, but really are. Such as
 - silencing errors with `error_reporting(0)`. This function has nothing to do with *displaying* errors and hence should never be used for the purpose. Which is fulfilled by the completely different option, surprisingly called *display_errors*. Whereas `error_reporting` should be set at `E_ALL` whenever possible
 - problem checking without an articulated outcome. A very typical example is 

            if ($result = mysqli_query($link, $query)) {
                // do stuff
            }
            // or
            if (is_readable($filename)) {
                include $filename;
            }
although such verifications look innocent or even the right thing to do, in practice it's none other than sweeping the dirt under the rug. Without such a condition, PHP would be vocal about its problems and hence instrumental in fixing the issue. While such a code just makes it fail silently and leave you oblivious of not only the the actual error but even the place in the code where it happened
 - substituting an invaluably precise and certain error message with some vague assertion, such as

            if (!is_readable($filename)) {
                echo "A file cannot be read!";
            }
`*jackie_chan_wtf.jpg*`! There are virtually *hundreds* of different reasons that may prevent a file from being readable - a permission issue, or some restriction such as open_basedir, or file doesn't exist, or it exists but in the different directory - etc,. etc., etc. Not to mention that a system error message always points at the exact line in the code where the problem occurs. While such home-brewed error messages will just pop out of nowhere. 
 - some minor issues such as [excessive usage of `isset`](https://phpdelusions.net/articles/null_coalescing_abuse) or its variants: `empty` and `??`. It is often used again only to mute the possible error message. Of course there are situations when a variable can be genuinely not set, but such cases are much scarcer than the usage of such operators
- *disclaimer:* of course there are situations when errors are non-critical and can be intentionally bypassed. But it should be a weighted decision, not a habit. 

A practical implementation of all the rules mentioned above can be found in the detailed article on the [PHP error reporting](https://phpdelusions.net/articles/error_reporting).

###Debugging

The concept of debugging is almost universally unknown to PHP learners, probably because it's as universally ignored by tutorial authors, who have quite peculiar ideas on the debugging, if any. 

At the same time, the concept of debugging is quite simple and natural. All you need to do is to *make your code talk to you*.


I am speaking here not about some specific tooling but about the generic principle, the actual *idea* that an issue can be hunted down and fixed by some other method than just staring at the code or asking someone else to take a look. The idea that a problem code must be *run* instead of stared at, and made *talking* about its problems. 

Basically, in order to find the error, you need two things:

- make your PHP talk to you by means of turning on all the possible PHP error reporting facilities mentioned in the previous chapter
- make your code talk to you by means of running it and *printing out intermediate results*

The learner's approach at the code is similar to assembling a Lego figure: if it failed, all you can do is to start over or ask someone to fix. But a code is not a Lego figure! You can make it *run* and make it *talk*!

Hence, always have error reporting at full. It will help PHP to tell you where the apparent error is. But the irony, such errors that can be found by PHP are the *simplest* ones. The real job starts when your code *doesn't produce any errors but doesn't work* (or works incorrectly) either. And here we come into the wonderful world of debugging. 

Only one thing before we start: in order to be able to debug your application you need  to understand *what every part of your code is supposed to do*. This is very important. When you positively know what value every variable should have at any given moment, the rest is a piece of cake: just add debugging output to your code and then compare the expected and actual output!

And then you just go and check every part distinctly. Your program is not a solid black box, which you feed the data with and it gives you the result. It consists of many parts and the problem may occur on the every step. 

Say, there is an HTML form with a single field that always saves only a part of the data in the database, no matter how much words you enter. Well then, there are **four** distinct steps taking part here, each must be debugged individually:

- receiving the form data in PHP
- storing the data in a database
- reading the stored data back from the database
- displaying the data back in the HTML form

First, you need to check whether the data has made it to the PHP script. Just add `var_dump($_POST); die;` at the top of the form handler script, submit the form and then *view the page source* (Ctrl-U)

- in case it it doesn't display *any* var_dump output, then there is a problem with the form action which is leading to a different script
- in case it shows an empty array or wrong data - then there is a problem with the form method or fields
- in case it shows any errors, they have to be read/googled up and fixed, then the process repeated

in case it shows the proper field name followed by the full text - then the form works correctly. Delete var_dump() and die calls and proceed to saving into the database

- in case there were PHP errors during the save, they have to be read/googled up and fixed, then the process repeated

then we have to check the data stored, using PHPmyAdmin, or a database console or any other database client.

- in case the data in the database is partially saved, then you have to check the field length (that could be insufficient) or the table encoding (that may cut the data on the unsupported symbol).

in case the data is saved correctly then we have to check whether it has been gotten from the database correctly. Again, add `var_dump($data); die;` right after getting the data from the database

- in case it is showing any errors they have to be read/googled up and fixed, then the process repeated
- in case the data is not shown then probably you are selecting it wrong. Check the input, and whether your code actually reaches the place

in case the data is shown, then proceed to displaying it in the form. In case it is not shown correctly, *view the page source* (Ctrl-U). If your data is there - then it means you incorrectly formatted it. Like, `value` attribute is missing quotes and/or the data is not encoded using `htmlspecialchars`. Fix it and here you go - the problem solved!

This is just an example to show you the generic course of actions. Just apply this general principle to any problem situation and have any problem solved or at least have a certain question for a Q&A site. 

###The workflow

- always do one thing at a time. Do not try to write the whole application at once. Split the application into distinct blocks (or pages) and each page into stages. Then finish them one by one, always verifying each stage's result. Got to write a CRUD? Then split it into distinct Create, Read, Update and Delete pages. Then each script into four stages: Reading the data from database, showing an HTML form, getting data from the form, storing data into the database
- always realize what would be the result of your PHP code. Remember that your PHP doesn't render HTML. If some HTML created on the fly by your PHP code doesn't work as intended, don't blame PHP. Instead, write the desired HTML as is. Make sure it works. And only then turn to PHP, making it produce exactly the same HTML, comparing it with the reference code you wrote earlier.
  - the same for for SQL: writing a complex SQL query dynamically? Write the desired SQL first, make sure it works, then proceed to PHP code recreating exactly the same SQL, comparing it with the reference code you wrote earlier.

###The application logic/display logic separation

Is a very important concept. Yet it can be achieved by very simple means, without implementing any fancy stuff such as MVC. All you need is to separate every script into two parts: a part where only PHP code is used and not a single character is sent to the client; and a part where all the output starts, with occasional PHP to organize the control flow. 

First of all, why do you need it? It's simple. An HTML page is not the only possible outcome for your script. There also could be:

- an error. In this case a generic error page must be shown, as discussed in the previous chapters
- no content at all. For example, after processing a POST form, or for some other reasons, there could be just an HTTP header telling a client to request another page
- non-HTML response, such as JSON

Besides, if you are starting the HTML before starting to process the data, you won't be able to change significant parts of the page, such as a `<title>` tag, Open Graph widgets, or a custom javascript/CSS and such. 

Besides, such a separation makes your code much simpler and cleaner. It becomes much easier to read and edit, for both a programmer and HTML designer. 

In a *simplest* form, just have the common HTML header and footer in the separate files, then structure your code like this

    <?php
    // here goes all the logic, 
    // getting all the data to be displayed ready

    // then start the output
    include 'header.php';
    ?>
    now here goes the HTML for the page content
    <?php
    include 'footer.php' 


But that's really only for your first project. Learn to use a dedicated template system, such as [Twig](https://twig.symfony.com/), as soon as possible. The principle, however, would remain the same: 

- define the common layout Twig template for your site
- create a Twig template for the particular page that *extends* the layout template
- write the PHP code for the page
- and once the code is done, just tell Twig to load the template for the page 

###Absolute and relative paths

A relatively (pun not intended) small issue but it leads to many confusions. 

- a relative path is built from the current location and therefore should be avoided whenever possible
- an absolute path is built from the system root and therefore always preferred. Always begins from `/` (or a drive letter on Windows filesystems, e.g. `C:\`)
- a path in the filesystem begins from the filesystem root and not to be confused with the web-server root. Used for including PHP files, reading file contents and such.
- a path on the web server begins from the virtual server's root and not to be confused with the filesystem root. Used to tell the browser to read a specific resource from a web-server.

Examples feature *the same file* addressed different ways :

    /var/www/example.com/index.php # a filesystem absolute path
             example.com/index.php # a filesystem relative path
                         index.php # a filesystem relative path
                        /index.php # a web-server absolute path 
                         index.php # a web-server relative path 

                    
the `/var/www/example.com` part is called the DOCUMENT_ROOT and can be found in the `$_SERVER`array in order to help building an absolute filesystem path from the web-server path. A more detailed explanation can be found in the dedicated [article](https://phpdelusions.net/articles/paths). Just a couple hints that can solve 99% of problems:

- Just because web-server root is always a constant (`/`) it's no problem to make every web path absolute, e.g.
 - `<img src="/img/logo.png">`
 - `<a href="/about/">`
 - `<link rel="stylesheet" href="/assets/css/mystyle.css">`
 - `header("Location: /profile/")`
- in PHP, `$_SERVER['DOCUMENT_ROOT']` can help to address a file independently from the current file, like
 - `include $_SERVER['DOCUMENT_ROOT']."/config.php"`
- also, a technically absolute filesystem path  but still relative to the current file can be built `__DIR__` constant,
 - `file_get_contents(__DIR__."/data.csv")`

###Performance

There are so many questions about performance asked out of the blue. But any concern about performance should be only provoked by the real life issue and backed by the results of *profiling *. Profiling, in simple terms, is measuring the execution times of distinct parts of the code and finding the slowest one, called a *bottleneck*. Only certain bottlenecks have to be fixed performance-wise, not just random parts of the code. Remember, "*[Premature optimization](https://wiki.c2.com/?PrematureOptimization) is the root of all evil*" 

The only generic advise about performance that could be given is: "avoid processing the huge amounts of data". This applies mostly to the database interaction when a learner unknowingly can cause an avalanche, the most notable example is selecting all the rows from a database table *only to count them*.

###The request-response cycle

Some confusion is coming from the distributed nature of the server-browser system and the discrete nature of the PHP application. It is very important to understand that the execution of a PHP script is atomic:

- when a browser sends a request to the server, PHP processes the request, sends the response back and then *dies*
- PHP is only executed on the server, no PHP code makes it to the browser (and hence it's impossible to see the PHP source code)
- when PHP blocks are intertwined with HTML blocks, it doesn't mean that PHP block gets executed on the server, then HTML block in the browser and so on. PHP executes entirely, and send the complete HTML to the browser. By the moment when a browser starts to render HTML, PHP program is likely already dead
- understanding of the HTTP protocol is essential for a web-programmer. Here is a very clear but concise writeup, [HTTP Basics](https://www3.ntu.edu.sg/home/ehchua/programming/webprogramming/HTTP_Basics.html)

###Encoding

Is simple - just use UTF-8. All you need is to configure it properly. This is going to be a gist of the great answer on Stack Overflow, [UTF-8 all the way through](https://stackoverflow.com/a/279279/285587). In short, you have to make sure that all parts of your application are talking to each other using UTF-8 encoding:

- a MySQL table should be created using `DEFAULT CHARSET utf8mb4`
- a PHP script talking to a database should configure the connection to use the same charset,
 - `$pdo = new PDO('mysql:host=...;charset=utf8mb4')` for PDO 
 - `$mysqli->set_charset('utf8mb4');` for mysqli
- a PHP script talking to a browser must send the UTF-8 encoding in the Content-type header,
 - either by means of setting `default_charset = "utf-8"` in `php.ini` 
 - or manually sending the header using `header('Content-Type: text/html; charset=utf-8');`
- source files must be saved in UTF-8 (without BOM if there is an option) encoding
