
## Introduction to Regular expressions

<b>Regular expressions</b> are extremely useful in extracting information from text such as code, log files, spreadsheets, or even documents. 
- The first thing to recognize when using regular expressions is that everything is essentially a character, and we are writing patterns to match a specific sequence of characters (also known as a string).
- Most patterns use normal ASCII, which includes letters, digits, punctuation and other symbols on your keyboard like %#$@!.
- Unicode characters can also be used to match any type of international text.
- Characters include normal letters, but digits as well. In fact, numbers 0-9 are also just characters and if you look at an ASCII table, they are listed sequentially.

<b> Exercise : Matching Characters </b> <i> write a pattern that matches all three rows. </i>

  text 1: <b>abc</b>dedfg<br>
  text 2: <b>abc</b>de<br>
  text 3: <b>abc</b><br>
  
  Answer: the patern is: <b>abc</b>

* The character<b>\d</b> can be used in place of any digit from 0 to 9
* The preceding slash distinguishes it from the simple d character and indicates that it is a metacharacter.

<b> Exercise : Matching Characters </b> <i> write a pattern that matches all the digits in the strings below. </i>

  text 1: abc<b>123</b>xyz<br>
  text 2: define"<b>123</b>"<br>
  text 3: var g = <b>123</b>;<br>

  Answer: the patern is: <b>123</b>
  
 <h3> The Dot(.)</h3>
  
  - With regular expressions, you are often matching pieces of text that you don't know the exact contents of, other than the fact that they share a common pattern or structure (eg. phone numbers or zip codes).
  - Wildcard, which is represented by the . (dot) metacharacter, can match any single character (letter, digit, whitespace, everything).
  - In order to specifically match a period, you need to escape the dot by using a slash \. accordingly.

<b> Exercise : Matching Characters </b> <i> write a single pattern that can match the first three strings, but not the last (to be skipped) in the strings below. </i>

  Match: <b>cat.</b><br>
  Match: <b>896.</b><br>
  Match: <b>?=+.</b><br>
  Match: <b>abc1</b><br>
 
 Answer: the patern is: <b>...\\.</b> (the three dots, backslash and a dot)
 
 - There is a method for matching specific characters using regular expressions, by defining them inside square brackets. 
 - For example, the pattern [abc] will only match a single a, b, or c letter and nothing else.

<b> Exercise : Matching specific Characters </b> <i> write a pattern that match the first three strings, but not the last three strings in the strings below. </i>

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
<b> Exercise : Matching specific Characters </b> <i> write a pattern that matches only the live animals (hog, dog, but not bog) in the strings below. </i>
  Match : <b>hog</b><br>
  Match : <b>dog</b><br>
  skip  : <b>bog</b><br>

Answer: [^b]og
The simplest solution to match any line that ends in 'og' but is not 'bog' would be the expression [^b]og. 

### Character Ranges

- When using the square bracket notation, there is a shorthand for matching a character in list of sequential characters by using the dash to indicate a character range. 
- For example, the pattern [0-6] will only match any single digit character from zero to six, and nothing else. And likewise, [^n-p] will only match any single character except for letters n to p.
- Multiple character ranges can also be used in the same set of brackets, along with individual characters. 
- An example of this is the alphanumeric \w metacharacter which is equivalent to the character range <b>[A-Za-z0-9_]</b> and often used to match characters in English text.

<b> Exercise : Matching using Character Ranges </b> <i> write a pattern that use the bracket notation to match or skip each character from each line in the strings below. </i>

  Match : <b>Ana</b><br>
  Match : <b>Bob</b><br>
  Match : <b>Cpc</b><br>
  skip  : <b>aax</b><br>
  skip  : <b>bby</b><br>
  skip  : <b>ccz</b><br>
  
  <b>Solution: </b> All the characters are sequential, so you can use the different ranges in the expression [A-C][n-p][a-c] to match only the first three lines.
  
  ### Matching Repetitions of Characters
  
  - One way that we can handle repetitions of characters is to explicitly spell out exactly how many characters we want
      * eg. \d\d\d which would match exactly three digits.
  - We can also specify how many repetitions of each character we want using the curly braces notation.
  #### Example: 
  
      :<b>a{3}</b> will match the <b>a</b> character exactly three times.
      :<b>a{1,3}</b> will match the <b>a</b> character no more than 3 times, but no less than once.
      :<b>w{3}</b> three w's
      :<b>[wxy]{5}</b> five characters, each of which can be a w, x, or y
      :<b>.{2,6}</b> between two and six of any character
      
 <b> Exercise : Matching using curly brace notation </b> Write a pattern that matches only the first two spellings by using the curly brace notation above.
 
  Match : <b>wazzzzzup</b><br>
  Match : <b>wazzzup</b><br>
  skip  : <b>wazup</b><br>
  
 <b>Solution:</b> There are a couple 'z's in the first two lines we have to match, so the expression waz{3,5}up will match all strings with that many 'z's.
 
 ### Matching arbitrary number of characters
<b> Kleene Star and the Kleene Plus</b> is a pattern which essentially represents either 0 or more or 1 or more of the character that it follows (it always follows a character or group).
Example: 
  * <b>\d+</b> which ensures that the input string has at least one digit.
  * <b>a+</b> One or more a's
  * <b>[abc]+</b> one or more of any a, b, or c character
  * <b>.*</b> zero or more of any character
 
<b> Exercise : Matching using the star and plus metacharacters </b>.
  Match : <b>aaaabcc</b><br>
  Match : <b>aabbbbc</b><br>
  Match : <b>aacc</b><br>
  skip  : <b>a</b><br>
 
 <b>Solution : </b> 
 - There are at least two 'a's, zero or more 'b's, and at least one 'c' in each line to match, so you can use the expression 'aa+b*c+' to represent this exactly.
 - Alternatively, an even more restrictive expression would be a{2,4}b{0,4}c{1,2} which puts both an upper and lower bound on the number of each of the characters. 

### The ? (question mark) metacharacter 
- The ? (question mark) metacharacter denotes <b>optionality</b>
- Allows you to match either zero or one of the preceding character or group.
- You will have to escape it using a slash \? to match a plain question mark character in a string.
Example:
  * <b>ab?c</b> will match either the strings "abc" or "ac" because the b is considered optional.

<b> Exercise : Characters optional </b> Write a pattern that uses the optionality metacharacter to match only the lines where one or more files were found.

  Match : <b>1 file found?</b><br>
  Match : <b>2 files found?</b><br>
  Match : <b>24 files found?</b><br>
  skip  : <b>No files found?</b><br>
 <b>Solution : </b> 
 - We can use the meta-character '\d' to match the number of files and use the expression <b> \d+ files? found\?</b> to match all the lines where files were found.
  * Note that the first question mark applies to the preceding 's' character (for plurality), and the actual question mark at the end must be escaped to match the text.
  * 

