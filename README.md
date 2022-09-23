# MarkMeDownForARegex

Regular expressions (RegEx) can be used to interact with text. They're extremely handy for a large number of things, including but not limited to concisely searching through text, or for validating user input. In general terms they define some set of search parameters to be applied to a query. Unfortunately they're not easy to understand at a glance.

## Summary

This tutorial will cover the following expression as an example:  
`/^(https?:\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?$/`

The layman might guess based on the https visible at the start that this expression might have something to do with a URL, and they'd be right! It checks a URL to see whether or not it fulfills a set of parameters. The specifics of how it does this and what those parameters are is covered below.

## Table of Contents

- [Anchors](#anchors)
- [Quantifiers](#quantifiers)
- [Grouping Constructs](#grouping-constructs)
- [Bracket Expressions](#bracket-expressions)
- [Character Classes](#character-classes)
- [The OR Operator](#the-or-operator)
- [Flags](#flags)
- [Character Escapes](#character-escapes)

## Regex Components

Fortunately, not everything about RegEx will be new to the average user. Parentheses (()) function in exactly the same way they normally do, by defining groups of expressions. And the "\\" can also function as an escape character, so `\/` is /, `\.` is .

### Anchors
`/`<mark>^</mark>`(https?:\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?`<mark>$</mark>`/`

True to their name, regex Anchors *anchor* the expression to specific positions on the string.  
The `^` anchor refers to the start of the string, and the `$` anchor refers to the end of it.  
If one wanted to check whether or not a string began with the letter "H", they could use `^H`, and if they wanted to check whether a string ended with a "d", they could use `d$`.

So, lets start from the beginning.
Given that parentheses define groups of expressions, and that anchors match to positions on a string, we can figure out the first parts of the example expression.

<mark>^</mark>`(https?:\/\/)?`

From the `^` we know that it's looking at the beginning of the string, and we know that it's looking for something that satisfies whatever parameters are defined by the group `(https?:\/\/)?`

This looks very much like the start of a URL, but you may be asking yourself, "what are those question marks?"

### Quantifiers

`/^(https`<mark>?</mark>`:\/\/)`<mark>?</mark>`([\da-z\.-]`<mark>+</mark>`)\.([a-z\.]`<mark>{2,6}</mark>`)([\/\w \.-]`<mark>*</mark>`)`<mark>*</mark>`\/`<mark>?</mark>`$/`

The question marks mark something as optional. That is, if there is a question mark after some statement, it's equivalent to saying "this part might be present, and it might not be." It's a subtype of a broader category of components known as "quantifiers"

Quantifiers match to something being present some number of times. There's 4 types: `?`, `+`, `*` and `{}`. The last one (`{}`) can be used in three different ways, and all of them can either be "Greedy" or "Lazy". However that distinction is not relevant to this example.

* `?`: Modifies an expression to say it matches zero or one time. (It's present or it's not, but it's not present more than once.)

* `+`: Matches one or more times. (Says something matching the modified expression is present at least once, but maybe more times.)

* `*`: Matches zero or more times. (Says something matching the modified expression may be present any number of times, or it may not be present at all.)

and finally

* `{}`: Is a general use quantifier. It comes in three types {n}, {n,}, and {n,m}, which correspond to matching exactly n number of times, matching at least n number of times, and matching between n and m number of times. (For example: {3,5} would be looking for something that matches between 3 and 5 times inclusive, and {3,} would be looking for something that matches at least three times.) The first three expressions (`?`, `+`, and `*`) can be rewritten in terms of this quantifier. Matching 0 or 1 times could be rewritten as {0,1}, matching at least once but potentially more times could be rewritten as {1,} and matching zero or more times can be rewritten as {0,}.

So, to break down the first part of the example expression:

`^(https?:\/\/)?`

both the s in https and the entire http(s) prefix are optional. In plain language, the URL might begin with http, it might begin with https and it might not begin with http(s):// at all. Note that the ? following the s only applies to the s, but the ? following the ) applies to everything within the parentheses.

`([\da-z\.-]`<mark>+</mark>`)`

Then, it's looking for something that will be present at least once, but perhaps more times.

`([a-z\.]`<mark>{2,6}</mark>`)`

Following that, it's looking for something that will be present between 2 and 6 times.

`([\/\w \.-]`<mark>*</mark>`)`

Then it's searching for something that will be present zero or more times.

Skipping to the end:
`\/`<mark>?</mark>`$/`
The expression is looking for a string that may or may not end (remember the `$` is the anchor for the end of the string) in a `/`

### Character Classes

Some special sets of characters match to broad categories of character, and for each there is also an inverse.

`\d` matches to any digit, and `\D` matches to any non digit, for example. This is important to remember going into the next session.

### Bracket Expressions

`/^(https?:\/\/)?(`<mark>`[`</mark>`\da-z\.-`<mark>`]`</mark>`+)\.(`<mark>`[`</mark>`a-z\.`<mark>`]`</mark>`{2,6})(`<mark>`[`</mark>`\/\w \.-`<mark>`]`</mark>`*)*\/?$/`

The contents of brackets specify the types of characters allowed to be present within a particular sub expression. It can be used in three ways

* \[abc\] Brackets can be used to define an explicit list of characters. \[a\] would allow only a, \[crhq\] would allow only the letters c, r, h, and q.

* \[a-c\] They can be used to define a range of characters. a-c allows letters a-c. e-l allows letters e through l.

* \[^a\] Lastly a ^ can be added to specify the inverse. While \[a\] allows only the letter a, \[^a\] specifies that the letter a may not be present. \[^a-c\] specifies that the range of letters between a and c (inclusive) may not be present.

From the example expression:

`([\da-z\.-]+)`

The character set within the brackets allows any digit, any lowercase letters from a-z, periods, and hyphens (-).
The + indicates that characters matching the preceding set will be present one or more times.

`([a-z\.]{2,6})`

The character set within these brackets allows lowercase letters from a-z, and periods.
The quantifier specifies that there will be between 2 and 6 (inclusive) characters matching this set.

`([\/\w \.-]*)`

Lastly, this character set allows forward slashes (`\/`), periods, spaces, and any "word character".
The word character is indicated by `\w` which is similar to how `\d` defines any digit. A word character is any lowercase, uppercase, digit, or underscore.
The quantifier indicates that characters matching the allowed set may be present zero or more times.


At this point we actually have everything we need to understand the expression.

`/^(https?:\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?$/`

`/^(https?:\/\/)?`
Matches to a string that may begin with `http://`, `https://` or may be missing the http tag entirely.

`([\da-z\.-]+)`
That must be followed by at least one character matching a digit, a lowercase letter, a period, or a hyphen. (This is the address. If the URL being checked was github.com, this section would refer to "github")

`\.([a-z\.]{2,6})`
The first part of the address must then be followed by a period and between 2 and 6 characters consisting of only lowercase letters and additional periods. (This is the ".com", or the ".net", or the ".co.uk").

`([\/\w \.-]*)*`
The suffix (be it .com or anything else) can then optionally be followed by any number of characters matching the provided set. (If the URL being checked was github.com/jamesyoungGHusername this sub-expression would check the /jamesyoungGHusername). The final * indicates that any number of expressions matching this description may be present.

Finally,   
`\/?$/`
Indicates that the end of the string may or may not have a trailing \/ character.


### The OR Operator

One operator not used in this example is the OR operator "|" which works in the same way as it does in programming. a|b accepts the characters a or b. Ten|10 accepts "Ten" or "10".

### Flags

Lastly, the example above is enclosed by slashes. Every part of a regular expression must be enclosed by these slashes EXCEPT any flags that are used. They're placed after the final slash character, and they can specify any special rules for that expression.

Flags are tricky, as they're used to indicate special behaviors. The docs describing their use can be found [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions#advanced_searching_with_flags).

For example, adding an i to the end of the expression indicates that it is not sensitive to case. 


## Author

[James Young](https://github.com/jamesyoungGHusername) completed a degree in economics only to discover that his true love was programming. He should have figured it out when he was going out of his way to find every excuse to write code. He is a full stack developer with approximately 7 years of experience mostly working on whimsical personal projects.
