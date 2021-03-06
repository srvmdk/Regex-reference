# Regular Expression
This is all about fundamentals of implementing regular expression

---
<br />
<br />

## Basic set

### any single character (including spaces) is acceptable between foo & bar
> `.` (wildcard) represents single character
```js
/foo.bar/
```
<br />


### any number of characters (including spaces) are acceptable between foo & bar
> `.*` represents any number of characters
```js
/foo.*bar/
```
<br />

### any number of spaces are acceptable between foo & bar
> `\s` represents white space, `*` represents any number of characters
```js
/foo\s*bar/
```
<br />

### only _f, c, l_ are accepted as char followed by _oo_
> `[abc]` represents character class -> inclusion list
```js
/[fcl]oo/
```
<br />

### only char except _f, c, l_ are accepted as char followed by _oo_
> `[^abc]` represents except character class -> exclusion list
```js
/[^fcl]oo/
```
<br />

### charater in range _a to c_ are accepted as char followed by _oo_
> `[a-c]` represents character class range -> inclusion list
```js
/[a-c]oo/
```
<br />

### charater in range _a to c_ or _y_ or _z_ are accepted as char followed by _oo_
> `[a-c]` represents character class range -> inclusion list
```js
/[a-cyz]oo/
```
<br />

### charater in range _a to c_ or _A-C_ or _y_ or _z_ are accepted as char followed by _oo_
> `[a-c]` represents character class range -> inclusion list
```js
/[a-cA-Cyz]oo/
```
<br />

### charater outside range _a to c_ are accepted as char followed by _oo_
> `[a-c]` represents character outside class range -> exclusion list
```js
/[^a-c]oo/
```
<br />

### character with multiple x followed by a period and again followed by multiple y
> escaping with backslash
```js
/x*\.y*/
```
<br />

### string with x followed by any of `.`, `#`, `*` followed by y are acceptable
> if a period is inside char class, no need to use backslash
```js
/x[.#*]y/
```
<br />

### string with x followed by any of ^.#*\ followed by y are acceptable
> since `^` or `\` inside char class has different meaning we need to use backslash to exclude
```js
/x[.#*\^\\]y/
```
<br />

### string that starts with foo are acceptable
> `^` represents beginning fo a line
```js
/^foo.*/
```
<br />

### string that ends with bar are acceptable
> `$` represents end of a line
```js
/.*bar$/
```
<br />

### string that only contains foo are acceptable
```js
/^foo$/
```
<br />

---
<br />
<br />

## Extended set

### curly brace repeater - identify a 3 digit number
```js
/^[0-9]{3}$/
```
<br />

### accept string that contains min 3 letters and max 6 letters
```js
/^[a-z]{3,6}$/
```
<br />

### accept string that contains 4 or more repeatations of _ha_
```js
/(ha){4,}/
```
<br />

### accept string that contains 4 or less repeatations of _ha_
> `^` and `$` are used so that regex don't break the string and treat it as whole
```js
/^(ha){1,4}$/
```
<br />

### accept string that contains any repetation of _a_ inside _foo and bar_, no _zero occurances_
> `a+` considers 1 or more occurances of a, while `a*` considers 0 or more repetations
```js
/fooa+bar/
```
<br />

### in a web address _only 0 or 1 occurances_ of _s_ is applicable for _http://_ or _https://_
> `?` checks only 1 or 0 occurances of preceeding char
```js
/https?\/\//
new RegExp(`https?//`)
```
<br />

### accepts if log or ply is followed by wood
> pipe symbol `|` resembles _or operator_
```js
/(log|ply)wood/
```
<br />

---
<br />
<br />

## Find and replace with capture groups

### `()` represents capture group
> anything wrapped by capture group will be available in the array value
<br />

### replacing a pattern
> `g` will search for every match, if absent only first match is considered <br/>
> **input string:** 'I have 1024x720 & 123x456 monitor' <br/>
> **expected output:** "I have '1024px by 720px' & '123px by 456px' monitor" <br/>
```js
re = /([0-9]+)x([0-9]+)/g
input.replace(re, (...arg) => `'${arg[1]}px by ${arg[2]}px'`)

// get all the matches
input.match(re)
// ["1024x720", "123x456"]
```
<br />

### replacing first and last name
> **input string:** 'Sourav Modak' <br/>
> **expected output:** Modak, Sourav <br/>
```js
re = /([a-zA-z]+)\s([a-zA-z]+)/g
input.replace(re, (...arg) => `${arg[2]}, ${arg[1]}`)
```
<br />

### formating time in 12 hours format
> **input string:** "It's 8:32 in morning" <br/>
> **expected output:** "It's 32 mins past 8 in morning" <br/>
```js
re = /(1[0-2]|[0-9]):([1-5][0-9])/g
input.replace(re, (...args) => `${args[2]} mins past ${args[1]}`)

input.match(re)
//  ["8:32"]

re.exec(input)
// ["8:32", "8", "32"]
```
<br />

### masking phone number
> **input string:** "123-456-7890" <br/>
> **expected output:** "xxx-xxx-7890" <br/>
```js
re = /^([1-9][0-9]{2})-([0-9]{3})-([0-9]{4})$/
input.replace(re, (...args) => `xxx-xxx-${args[3]}`)js

re.exec(input)
// ["123-456-7890", "123", "456", "7890"]
```
<br />

### formatting a date
> **input string:** "Jan 30th 1982" / "Jan 5th 1982" <br/>
> **expected output:** "30-Jan-82" / "Jan 5th 1982" <br/>
```js
re = /([a-zA-Z]{3})\s([1-2]?[0-9]|3?[0-1])[a-z]{2}\s[0-9]{2}([0-9]{2})/
input.replace(re, (...args) => `${args[2]}-${args[1]}-${args[3]}`)
```
<br />

---
