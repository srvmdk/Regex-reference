# Regular Expression
This is all about fundamentals of implementing regular expression
---

## Basic set

### any single character (including spaces) is acceptable between foo & bar
**`.` (wildcard) represents single character**
```
/foo.bar/
```__

### any number of characters (including spaces) are acceptable between foo & bar
**`.*` represents any number of characters**
```
/foo.*bar/
```__

### any number of spaces are acceptable between foo & bar
**`\s` represents white space, `*` represents any number of characters**
```
/foo\s*bar/
```__

### only _f, c, l_ are accepted as char followed by _oo_
**`[abc]` represents character class -> inclusion list**
```
/[fcl]oo/
```__

### only char except _f, c, l_ are accepted as char followed by _oo_
**`[^abc]` represents except character class -> exclusion list**
```
/[^fcl]oo/
```__

### charater in range _a to c_ are accepted as char followed by `oo`
**`[a-c]` represents character class range -> inclusion list**
```
/[a-c]oo/
```__

### charater in range `a to c` or `y` or `z` are accepted as char followed by `oo`
**`[a-c]` represents character class range -> inclusion list**
```
/[a-cyz]oo/
```__

### charater in range `a to c` or `A-C` or `y` or `z` are accepted as char followed by `oo`
**`[a-c]` represents character class range -> inclusion list**
```
/[a-cA-Cyz]oo/
```__

### charater outside range `a to c` are accepted as char followed by `oo`
**`[a-c]` represents character outside class range -> exclusion list**
```
/[^a-c]oo/
```__

### character with multiple x followed by a period and again followed by multiple y
**escaping with backslash**
```
/x*\.y*/
```__

### string with x followed by any of `.`, `#`, `*` followed by y are acceptable
**if a period is inside char class, no need to use backslash**
```
/x[.#*]y/
```__

### string with x followed by any of ^.#*\ followed by y are acceptable
**since `^` or `\` inside char class has different meaning we need to use backslash to exclude**
```
/x[.#*\^\\]y/
```__

### string that starts with foo are acceptable
**`^` represents beginning fo a line**
```
/^foo.*/
```__

### string that ends with bar are acceptable
**`$` represents end of a line**
```
/.*bar$/
```__

### string that only contains foo are acceptable
```
/^foo$/
```__

---

## Extended set ##

### curly brace repeater - identify a 3 digit number
```
/^[0-9]{3}$/
```__

### accept string that contains min 3 letters and max 6 letters
```
/^[a-z]{3,6}$/
```__

### accept string that contains 4 or more repeatations of _ha_
```
/(ha){4,}/
```__

### accept string that contains 4 or less repeatations of _ha_
**`^` and `$` are used so that regex don't break the string and treat it as whole**
```
/^(ha){1,4}$/
```__

### accept string that contains any repetation of _a_ inside _foo and bar_, no _zero occurances_
**`a+` considers 1 or more occurances of a, while `a*` considers 0 or more repetations**
```
/fooa+bar/
```__

### in a web address _only 0 or 1 occurances_ of `s` is applicable for `http://` or `https://`
**`?` checks only 1 or 0 occurances of preceeding char**
```
/https?\/\//
new RegExp(`https?//`)
```__

### accepts if log or ply is followed by wood
**pipe symbol `|` resembles _or operator_**
```
/(log|ply)wood/
```__

---

## Find and replace with capture groups

### `()` represents capture group
> anything wrapped by capture group will be available in the array value__

### replacing a pattern
**`g` will search for every match, if absent only first match is considered**
> **input string:** 'I have 1024x720 & 123x456 monitor'
> **expected output:** "I have '1024px by 720px' & '123px by 456px' monitor"
```__
re = /([0-9]+)x([0-9]+)/g
input.replace(re, (...arg) => `'${arg[1]}px by ${arg[2]}px'`)
```
> To get all the matches, eg. ["1024x720", "123x456"]
```
input.match(re)
```__

### replacing first and last name
> **input string:** 'Sourav Modak'
> **expected output:** Modak, Sourav
```
/([a-zA-z]+)\s([a-zA-z]+)/g
re = input.replace(re, (...arg) => `${arg[2]}, ${arg[1]}`)
```__

### formating time in 12 hours format
> **input string:** "It's 8:32 in morning"
> **expected output:** "It's 32 mins past 8 in morning"
```
re = /(1[0-2]|[0-9]):([1-5][0-9])/g
input.replace(re, (...args) => `${args[2]} mins past ${args[1]}`)
```
Different outputs for match & exec
```
input.match(re) //  ["8:32"]
re.exec(input) // ["8:32", "8", "32"]
```__

### masking phone number
> **input string:** "123-456-7890"
> **expected output:** "xxx-xxx-7890"
```
re = /^([1-9][0-9]{2})-([0-9]{3})-([0-9]{4})$/
input.replace(re, (...args) => `xxx-xxx-${args[3]}`)
re.exec(input) // ["123-456-7890", "123", "456", "7890"]
```__

### formatting a date
**input string:** "Jan 30th 1982" / "Jan 5th 1982"
**expected output:** "30-Jan-82" / "Jan 5th 1982"
```
re = /([a-zA-Z]{3})\s([1-2]?[0-9]|3?[0-1])[a-z]{2}\s[0-9]{2}([0-9]{2})/
input.replace(re, (...args) => `${args[2]}-${args[1]}-${args[3]}`)
```
