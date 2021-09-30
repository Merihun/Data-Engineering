
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

* The character<b>\d</b> can be used in place of any digit from 0 to 9
* The preceding slash distinguishes it from the simple d character and indicates that it is a metacharacter.

<b> Exercise 2: Matching Characters </b> <i> write a pattern that matches all the digits in the strings below. </i>

  text 1: abc<b>123</b>xyz<br>
  text 2: define"<b>123</b>"<br>
  text 3: var g = <b>123</b>;<br>


