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