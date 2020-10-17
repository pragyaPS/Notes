https://medium.com/better-programming/everything-you-need-to-know-about-regular-expressions-in-javascript-59807f758cbd

What can u do with the regular expressions
 - Can check if a text contains a particular substring or pattern
 - can find and return those pattern matches
 - Can capture this subStrings out of the text
 - Can modify the captured substrings.

 JavaScript’s Regular expression engine is based on Perl5 regular expression Grammar.

Try regular expression:
 https://regex101.com/r/kjpTjX/1

|   |   |   |
|---|---|---|
| A single character of A,B,or C  | [abc] | `/[abc]+/g` a bb ccc|
|A char except a,b or c | [^abc]| `/[^abc]+/g`|  
|  A char in the range:a-z | [a-z] | `/[a-z]+/g` |
|  A char in the range:a-z orA-Z | [a-z] | `/[a-z]+/g` |
|  Any whitespace char | \s | `/\s/g` |
|  Any **non** whitespace char | \S | `/\S+/` |
|  Any digit | \d | `/\d/g` |
|  Any **non** digit | \D | `/\D+/` |
|  Any word charactert | \w | `/\w+/g` |
|  Any **non** word charactert | \W | `/\W+/g` |
|  Capture everything enclosed| (...) | `/(he)+/g` |
|  Match either a or b| (a|b) | `/(a|b)/g` |
|  Zero or one of a | a? | `/ba?/g` |
|  Zero or more of a | a* | `/ba*/g` |
|  One or more of a | a+| `/a+/g` |
|  Exactly 3 of a | a{3}| `/a{3}/g` |
|  3 or more of a | a{3,}| `/a{3,}/` |
|  start of string | ^| `/^\w+/` |
|  End of string | $ | `/\w+$/` |

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions/Cheatsheet


How Do You Define a Regular Expression in JavaScript?

A regular expression is an object that describes a pattern of characters. In JavaScript, you can define regular expressions in two different ways:

A regular expression is an object that describes a paatern of characters. In javascript we can define regular expressions in two ways
 - Using Regular expression literal, within a pair of slash 
  ``` const myPattern = /Med[a-zA-Z]*/ ```
 - Or by constructing an instance of the `RegExp` object: 
  ``` const myPattern = new RegExp("Med[a-zA-Z]*"); ```

both formats results in the same regex being created in the variable myPattern

In addition to the expression itself, five options can be incorporated in a regex:
- i : Makes the regex case insensitive. For example, /Medi[a-zA-Z]*/i would match all cases.
- g: Matches all occurrences of the pattern. When g is not specified the regex would match only the first occurrence.
- m : Allows matches across multiple lines of a text.
- y : Enables sticky matching; a regex attempts sticky matching in a string by trying to match from the last matching position.
- u : Allows the use of Unicode point escapes \u….

const myPattern = new RegExp (“Medi[a-zA-Z]*”, "ig");
is equivalent to
const myPattern = /Medi[a-zA-Z]*/ig;


### Exact matching

Any alphanumeric character that’s not a special meta-character or an operator will match itself in a regex.
In our previous example, /Medi[a-zA-Z]*/: M is a character that matches itself, same as with e,d and i.
Placing one a character after another indicates that we are looking for M followed by e, followed by d, followed by i. In such an example, Medo will not match.

### Matching a class of characters

An [abc] character set will match any character in that set either a or b or c. If we were to add a ^ just after the brackets, the set [^abc] will match any character except a, b and c.

### How to Use Captures and Backreferences

When parentheses surround a part of a regex, it creates a capture.
Say we want to match an HTML tag, we can use a regular expression that looks like this: /<([a-z]\w*)\b[^>]*>/
Let’s break it down.
An HTML tag starts with the character < and ends with >.
[a-z] will match any lowercase alphabetical character.
\w* will match any alphanumeric character including underscore.
\b assert position at a word boundary.
[^>]* matches any character except >.
This regex will match : <div>, <span>, <something>, etc
In this example, we’re capturing the name of the tag, ([a-z]\w*), that this part of the regex will match. For example, div, span, something.



let's say we want our regex to match a correct HTML element, that starts with a tag and ends with the same tag closed: <div>something</div>.
In this case, we want to be able to reference the tag we captured previously. The notation of the backreference is the backslash followed by the number of the capture to be referenced: \1.
In our example, we have only one capture. To match a full HTML element, the regex will look like this: /<([a-z]\w*)\b[^>]*>.*?<\/\1>/.
It might seem confusing at first, but let’s break it down:
.*? will match any character, except for line terminators.
<\/\1> will match a closing HTML tag: < matches its literal self, \/ matches a / character. \1 refers back to our capture, and > matches again it’s literal self.
This regex will match the following strings: <div> <i>something</i> </div>, <span>my span</span>, etc.
A string like <div> something</other> will not match.
Without the backreference, we will not be able to match a simple HTML element.


