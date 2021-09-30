
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

  Match: <b>cat.</b><br>
  Match: <b>896.</b><br>
  Match: <b>?=+.</b><br>
  Match: <b>abc1</b><br>
 
 Answer: the patern is: <b>...\\.</b> (the three dots, backslash and a dot)
 
 - There is a method for matching specific characters using regular expressions, by defining them inside square brackets. 
 - For example, the pattern [abc] will only match a single a, b, or c letter and nothing else.

<b> Exercise 4: Matching specific Characters </b> <i> write a pattern that match the first three strings, but not the last three strings in the strings below. </i>

  Match : <b>can</b><br>
  Match : <b>man</b><br>
  Match : <b>fan</b><br>
  skip  : <b>dan</b><br>
  skip  : <b>ran</b><br>
  skip  : <b>pan</b><br>

Answer: [cmf]an
You can use the expression [cmf]an to match only 'can', 'man' and 'fan' without matching any other line.

- In some cases, we might know that there are specific characters that we don't want to match too, for example, we might only want to match phone numbers that are not from the area code 650.
- We can acomplish this by using the square brackets and the ^ (hat).
- For example, the pattern [^abc] will match any single character except for the letters a, b, or c.
<b> Exercise 4: Matching specific Characters </b> <i> write a pattern that matches only the live animals (hog, dog, but not bog) in the strings below. </i>
  Match : <b>hog</b><br>
  Match : <b>dog</b><br>
  skip  : <b>bog</b><br>

Answer: [^b]og
The simplest solution to match any line that ends in 'og' but is not 'bog' would be the expression [^b]og. 

