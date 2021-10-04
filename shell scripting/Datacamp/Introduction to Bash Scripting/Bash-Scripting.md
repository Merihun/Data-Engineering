
1. Introduction and refresher
Welcome to this course on Bash scripting. I am Alex, a data scientist from Sydney Australia.

2. Introduction to the course
In this course we will cover; Moving from one-at-a-time commands in a terminal to full scripts. Creating and using variables and various data types in Bash scripts. Controlling your scripts using tools such as if-statements and for-loops. and writing functions and learning to schedule scripts with cron.

3. Why Bash scripting? (Bash)
Let's first review what Bash is and why it matters. Bash stands for bourne again shell and is a pun on it being a replacement for the bourne shell. It is old (developed in the 80's) but is a very popular shell and often the default for unix and mac computer systems. Unix systems are the backbone of the internet and servers and so important for running large ML models and for data pipelines and architectures. All major cloud companies have command-line interfaces to their products.

4. Why Bash scripting? (scripting!)
So why Bash scripting? Rather than copy-pasting your Bash commands one-by-one you can save and execute your program with a single command. You also have easy access to powerful programming constructs mentioned on earlier slides.

5. Expected knowledge
This course will expect you to have certain knowledge such as: Understanding the shell or command-line. You should have used basic commands such as cat, grep, sed and others before. If you are rusty, don't worry - let's revise now!

6. Shell commands refresher
Here are some important shell commands. This is not exhaustive, but a brief reminder. grep or egrep filters your input, perhaps from another program or command using regex pattern matching. cat opens up file contents to be used line-by-line. Perhaps viewed or piped onwards. tail and head give you the first or last lines using the n flag. wc does a word or line count depending on using the flag w or l. and sed does a string replacement using regex pattern-matching.

### 7. A reminder of REGEX
A few times already we have mentioned pattern matching. Regex, which is an abbreviation of regular expressions are therefore very useful in Bash scripting. There are a variety of uses for regex in Bash scripting and so it is important that you revise this skill and use sites such as regex101.com to test your expressions.

### 8. Some shell practice
Let's practice some of those basic shell commands with an example. Imagine that you had a text file called fruits.txt with the following 3 lines of data. Using the grep command with a letter to match on, we would get this return. That is, all the lines containing an a.

9. Some shell practice
If we changed the letter a for the letter p, then we would only match on apple. When doing matching, recall that square parentheses create a set and using the caret symbol inverses that, asking to match on anything but those. If we created a set with p and c and re-ran the match we would return anything with a p or c inside. That is, apple and carrot.

10. Some shell practice
In the command line you likely have used pipes to push the output of one command into the next. If we had a file with many lines, we may take advantaged of this by piping the file contents to sort and then uniq with the flag c The first command sorts the data alphabetically and the next line will do a distinct count. It is important to sort first, since uniq only considers adjacent for its distinct operation. If we wanted the top n fruits, we could use wc with the l flag and then head as follows.

