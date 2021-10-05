(note adapted from geeksforGeeks)
## Array Basics in Shell Scripting 

- An array is a systematic arrangement of the same type of data. 
- Shell script Array is a variable which contains multiple values may be of same type or different type since by default in shell script everything is treated as a string. 
- An array is zero-based ie indexing start with 0.

## Declaring Array

We can declare an array in a shell script in different ways.

1. Indirect Declaration: <b>ARRAYNAME[INDEXNR]=value</b>
    - assigned a value in a particular index of Array Variable. 
    - No need to first declare.

2. Explicit Declaration : <b>declare -a ARRAYNAME</b>

- First We declare array then assign the values.

3. Compound Assignment : <b>ARRAYNAME=(value1 value2  .... valueN)</b> or <b>[indexnumber=]string</b>
-  We declare array with a bunch of values. 
-  We can add other values later too.
example: ARRAYNAME=([1]=10 [2]=20 [3]=30)


## Print Array Value in Shell Script : echo ${ARRAYNAME[*]}

- To Print All elements: <b>[@] & [*]</b> means All elements of Array.
- To print elements from a particular index: echo ${ARRAYNAME[WHICH_ELEMENT]:STARTING_INDEX}
- To print elements in range: echo ${ARRAYNAME[WHICH_ELEMENT]:STARTING_INDEX:COUNT_ELEMENT}

## To count Length of in Array

To count the length of a particular element in Array.
Use #(hash) to print length of particular element

## To Search in Array

arr[@] : All Array Elements.
/Search_using_Regular_Expression/ : Search in Array
Search Returns 1 if it found the pattern else it return zero. It does not alter the original array elements.

## delete Array Variable in Shell Script

To delete index-1 element

### unset ARRAYNAME[1]
To delete the whole Array 
### unset ARRAYNAME

## Appending to arrays
Appent to an arrray using ### array+=(elements)


#! /bin/bash
# To declare static Array
arr=(prakhar ankit 1 rishabh manish abhinav)

# To print all elements of array
echo ${arr[@]}	 # prakhar ankit 1 rishabh manish abhinav
echo ${arr[*]}	 # prakhar ankit 1 rishabh manish abhinav
echo ${arr[@]:0} # prakhar ankit 1 rishabh manish abhinav
echo ${arr[*]:0} # prakhar ankit 1 rishabh manish abhinav

# To print first element
echo ${arr[0]}	 # prakhar
echo ${arr}		 # prakhar

# To print particular element
echo ${arr[3]}	 # rishabh
echo ${arr[1]}	 # ankit

# To print elements from a particular index
echo ${arr[@]:0} # prakhar ankit 1 rishabh manish abhinav
echo ${arr[@]:1} # ankit 1 rishabh manish abhinav
echo ${arr[@]:2} # 1 rishabh manish abhinav
echo ${arr[0]:1} # rakhar

# To print elements in range
echo ${arr[@]:1:4} # ankit 1 rishabh manish
echo ${arr[@]:2:3} # 1 rishabh manish
echo ${arr[0]:1:3} # rak

# Length of Particular element
echo ${#arr[0]}	 # 7
echo ${#arr}	 # 7

# Size of an Array
echo ${#arr[@]}	 # 6
echo ${#arr[*]}	 # 6

# Search in Array
echo ${arr[@]/*[aA]*/} # 1

# Replacing Substring Temporary
echo ${arr[@]//a/A}	 # prAkhAr Ankit 1 rishAbh mAnish AbhinAv
echo ${arr[@]}		 # prakhar ankit 1 rishabh manish abhinav
echo ${arr[0]//r/R}	 # pRakhaR

