
## Introduction to Regular expressions

<b>Regular expressions</b> are extremely useful in extracting information from text such as code, log files, spreadsheets, or even documents. 
- The first thing to recognize when using regular expressions is that everything is essentially a character, and we are writing patterns to match a specific sequence of characters (also known as a string).
- Most patterns use normal ASCII, which includes letters, digits, punctuation and other symbols on your keyboard like %#$@!.
- Unicode characters can also be used to match any type of international text.
- Characters include normal letters, but digits as well. In fact, numbers 0-9 are also just characters and if you look at an ASCII table, they are listed sequentially.

<b> Exercise 1: Matching Characters </b> <i> write a pattern that matches all three rows. </i>

  text 1: <b>abc</b>dedfg<br>
  text 2: <b>abc</b>de<br>
  text 3: <b>abc</b><br>
  
  Answer: the patern is: <b>abc</b>

* The character<b>\d</b> can be used in place of any digit from 0 to 9
* The preceding slash distinguishes it from the simple d character and indicates that it is a metacharacter.

<b> Exercise 2: Matching Characters </b> <i> write a pattern that matches all the digits in the strings below. </i>

  text 1: abc<b>123</b>xyz<br>
  text 2: define"<b>123</b>"<br>
  text 3: var g = <b>123</b>;<br>

  Answer: the patern is: <b>123</b>
  
 <h3> The Dot(.)</h3>
  
  - With regular expressions, you are often matching pieces of text that you don't know the exact contents of, other than the fact that they share a common pattern or structure (eg. phone numbers or zip codes).
  - Wildcard, which is represented by the . (dot) metacharacter, can match any single character (letter, digit, whitespace, everything).
  - In order to specifically match a period, you need to escape the dot by using a slash \. accordingly.

<b> Exercise 3: Matching Characters </b> <i> write a single pattern that can match the first three strings, but not the last (to be skipped) in the strings below. </i>

  text 1: <b>cat.</b><br>
  text 2: <b>896.</b><br>
  text 3: <b>?=+.</b><br>
  text 4: <b>abc1</b><br>
 
 Answer: the patern is: <h3>...\.</h3>
