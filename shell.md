# Shell Scripting Notes

## Shell Variables

Shell variables offer a powerful way to store and later access or modify information such as numbers, character strings, and other data structures by name. Let's look at some basic examples to get the idea.

```shell
    $ firstname=Jeff
    $ echo $firstname
    Jeff
```

The first line assigns the value Jeff to a new variable called firstname. The next line accesses and displays the value of the variable, using the echo command along with the special character $ in front of the variable name to extract its value, which is the string Jeff.

Thus, we have created a new shell variable called firstname for which the value is Jeff.

This is the most basic way to create a shell variable and assign it to a value all in one step.
Reading user input into a shell variable at the command line

Here's another way to create a shell variable, using the read command.
After entering

```shell
    $ read lastname
```

on the command line, the shell waits for you to enter some text:

```shell
    $ read lastname  
    Grossman  
    $ 
```

Now we can see that the value Grossman has just been stored in the variable lastname by the read command:

```shell
    $ read lastname  
    Grossman  
    $ echo $lastname  
    Grossman  
```

By the way, notice that you can echo the values of multiple variables at once.

```shell
    $ echo $firstname $lastname  
    Jeff Grossman  
```

As you will soon see, the read command is particularly useful in shell scripting. You can use it within a shell script to prompt users to input information, which is then stored in a shell variable and available for use by the shell script while it is running. You will also learn about command line arguments, which are values that can be passed to a script and automatically assigned to shell variables.

---

## Pipes

Put simply, pipes are commands in Linux which allow you to use the output of one command as the input of another.

Pipes `|` use the following format:

```shell
    [command 1] | [command 2] | [command 3] ... | [command n]
```

There is no limit to the number of times you can chain pipes in a row!

In this lab, you'll take a closer look at how you can use pipes and filters to solve basic data processing problems.


### Combining commands

Let's start with a commonly used example. Recall the following commands:

    sort - sorts the lines of text in a file and displays the result
    uniq - prints a text file with any consecutive, repeated lines collapsed to a single line

With the help of the pipe operator, you can combine these commands to print all the unique lines in a file.

Suppose you have the file pets.txt with the following contents:

```shell
    $ cat pets.txt
    goldfish
    dog
    cat
    parrot
    dog
    goldfish
    goldfish
```

If you only use sort on pets.txt, you get:

```shell
    $ sort pets.txt
    cat
    dog
    dog
    goldfish
    goldfish
    goldfish
    parrot
```

The file is sorted, but there are duplicated lines of "dog" and "goldfish".

On the other hand, if you only use uniq, you get:

```shell
    $ uniq pets.txt
    goldfish
    dog
    cat
    parrot
    dog
    goldfish
```

This time, you removed consecutive duplicates, but non-consecutive duplicates of "dog" and "goldfish" remain.

But by combining the two commands in the correct order - by first using sort then uniq - you get back:

```shell
    $ sort pets.txt | uniq
    cat
    dog
    goldfish
    parrot
```

Since sort sorts all identical items consecutively, and uniq removes all consecutive duplicates, combining the commands prints only the unique lines from pets.txt!
Applying a command to strings and files

Some commands such as tr only accept standard input - normally text entered from your keyboard - but not strings or filenames.

    tr (translate) - replaces characters in input text

    tr [OPTIONS] [target characters] [replacement characters]

In cases like this, you can use piping to apply the command to strings and file contents.

With strings, you can use echo in combination with tr to replace all the vowels in a string with underscores _:

```shell
    $ echo "Linux and shell scripting are awesome\!" | tr "aeiou" "_"
    L_n_x _nd sh_ll scr_pt_ng _r_ _w_s_m_!
```

To perform the complement of the operation from the previous example - or to replace all the consonants (any letter that is not a vowel) with an underscore - you can use the -c option:

```shell
    $ echo "Linux and shell scripting are awesome\!" | tr -c "aeiou" "_"
    _i_u__a_____e______i__i___a_e_a_e_o_e_
```

With files, you can use cat in combination with tr to change all of the text in a file to uppercase as follows:

```shell
    $ cat pets.txt | tr "[a-z]" "[A-Z]"
    GOLDFISH
    DOG
    CAT
    PARROT
    DOG
    GOLDFISH
    GOLDFISH
```

The possibilities are endless! For example, you could add uniq to the above pipeline to only return unique lines in the file, like so:

```shell
    $ sort pets.txt | uniq | tr "[a-z]" "[A-Z]"
    CAT
    DOG
    GOLDFISH
    PARROT
```

Extracting information from JSON Files:

Let's see how you can use this json file to get the current price of Bitcoin (BTC) in USD, by using grep command.

```json
    {
      "coin": {
        "id": "bitcoin",
        "icon": "https://static.coinstats.app/coins/Bitcoin6l39t.png",
        "name": "Bitcoin",
        "symbol": "BTC",
        "rank": 1,
        "price": 57907.78008618953,
        "priceBtc": 1,
        "volume": 48430621052.9856,
        "marketCap": 1093175428640.1146,
        "availableSupply": 18877868,
        "totalSupply": 21000000,
        "priceChange1h": -0.19,
        "priceChange1d": -0.4,
        "priceChange1w": -9.36,
        "websiteUrl": "http://www.bitcoin.org",
        "twitterUrl": "https://twitter.com/bitcoin",
        "exp": [
          "https://blockchair.com/bitcoin/",
          "https://btc.com/",
          "https://btc.tokenview.com/"
        ]
      }
    }
```

Copy the above output in a file and name it as Bitcoinprice.txt.

The JSON field you want to grab here is "price": [numbers].[numbers]". To get this, you can use the following grep command to extract it from the JSON text:

```shell
    grep -oE "\"price\"\s*:\s*[0-9]*?\.[0-9]*"
```

Let's break down the details of this statement:

    -o tells grep to only return the matching portion
    -E tells grep to be able to use extended regex symbols such as ?
    \"price\" matches the string "price"
    \s* matches any number (including 0) of whitespace (\s) characters
    : matches :
    [0-9]* matches any number of digits (from 0 to 9)
    ?\. optionally matches a .

Use the cat command to get the output of the JSON file and pipe it with the grep command to get the required output.

```shell
    cat Bitcoinprice.txt | grep -oE "\"price\"\s*:\s*[0-9]*?\.[0-9]*"
```

You can also extract information directly from URLs and retreive any specific data using such grep commands.

---

## Metacharacters

Metacharacters are characters having special meaning that the shell interprets as instructions.

| Metacharacter |	Meaning |
| ------------- |	------- |
| # |	Precedes a comment |
| ; |	Command separator |
| * |	Filename expansion wildcard |
| ? |	Single character wildcard in filename expansion |

### Pound #

The pound # metacharacter is used to represent comments in shell scripts or configuration files. Any text that appears after a # on a line is treated as a comment and is ignored by the shell.

```shell
    #!/bin/bash
    # This is a comment
    echo "Hello, world!"  # This is another comment
```

Comments are useful for documenting your code or configuration files, providing context, and explaining the purpose of the code to other developers who may read it. It's a best practice to include comments in your code or configuration files wherever necessary to make them more readable and maintainable.

### Semicolon ;

The semicolon ; metacharacter is used to separate multiple commands on a single command line. When multiple commands are separated by a semicolon, they are executed sequentially in the order they appear on the command line.

```shell
    $ echo "Hello, "; echo "world!"
    Hello,
    world!
```

As you can see from the example above, the output of each echo command is printed on separate lines and follows the same sequence in which the commands were specified.

The semicolon metacharacter is useful when you need to run multiple commands sequentially on a single command line.

### Asterisk *

The asterisk * metacharacter is used as a wildcard character to represent any sequence of characters, including none.

```shell
    ls *.txt
```

In this example, *.txt is a wildcard pattern that matches any file in the current directory with a .txt extension. The ls command lists the names of all matching files.

### Question mark ?

The question mark ? metacharacter is used as a wildcard character to represent any single character.

```shell
    ls file?.txt
```

In this example, file?.txt is a wildcard pattern that matches any file in the current directory with a name starting with file, followed by any single character, and ending with the .txt extension.


### Quoting

Quoting is a mechanism that allows you to remove the special meaning of characters, spaces, or other metacharacters in a command argument or shell script. You use quoting when you want the shell to interpret characters literally.

| Symbol | Meaning |
| ------ | ------- |
|  \  | Escape metacharacter interpretation |
| " " | Interpret metacharacters within string |
| ' ' | Escape all metacharacters within string |

### Backslash \

The backslash character is used as an escape character. It instructs the shell to preserve the literal interpretation of special characters such as `space`, `tab`, and `$`. For example, if you have a file with spaces in its name, you can use backslashes followed by a space to handle those spaces literally:

```shell
    touch file\ with\ space.txt
```

### Double quotes " "

When a string is enclosed in double quotes, most characters are interpreted literally, but metacharacters are interpreted according to their special meaning. For example, you can access variable values using the dollar $ character:

```shell
    $ echo "Hello $USER"
    Hello <username>
```

### Single quotes ' '

When a string is enclosed in single quotes, all characters and metacharacters enclosed within the quotes are interpreted literally. Single quotes alter the above example to produce the following output:

```shell
    $ echo 'Hello $USER'
    Hello $USER
```

Notice that instead of printing the value of $USER, single quotes cause the terminal to print the string "$USER".

### Input/Output redirection

| Symbol | Meaning |
| ------ | ------- |
| > 	| Redirect output to file, overwrite |
| >> 	| Redirect output to file, append |
| 2> 	| Redirect standard error to file, overwrite |
| 2>>   | Redirect standard error to file, append |
| < 	| Redirect file contents to standard input |

Input/output (IO) redirection is the process of directing the flow of data between a program and its input/output sources.

By default, a program reads input from standard input, the keyboard, and writes output to standard output, the terminal. However, using IO redirection, you can redirect a program's input or output to or from a file or another program.
Redirect output >

This symbol is used to redirect the standard output of a command to a specified file.

    ls > files.txt will create a file called files.txt if it doesn't exist, and write the output of the ls command to it.

    Warning: When the file already exists, the output overwrites all of the file's contents!

### Redirect and append output >>

This notation is used to redirect and append the output of a command to the end of a file. For example,

    ls >> files.txt appends the output of the ls command to the end of file files.txt, and preserves any content that already existed in the file.

### Redirect standard output 2>

This notation is used to redirect the standard error output of a command to a file. For example, if you run the ls command on a non-existing directory as follows,

    ls non-existent-directory 2> error.txt the shell will create a file called error.txt if it doesn't exist, and redirect the error output of the ls command to the file.

    Warning: When the file already exists, the error message overwrites all of the file's contents!

### Append standard error 2>>

This symbol redirects the standard error output of a command and appends the error message to the end of a file without overwriting its contents.

    ls non-existent-directory 2>> error.txt will append the error output of the ls command to the end of the error.txt file.

### Redirect input <

This symbol is used to redirect the standard input of a command from a file or another command. For example,

    sort < data.txt will sort the contents of the data.txt file.

### Command Substitution

Command substitution allows you to run command and use its output as a component of another command's argument. Command substitution is denoted by enclosing a command in either backticks (`command`) or using the $() syntax. When the encapsulate command is executed, its output is substituted in place, and it can be used as an argument within another command. This is particularly useful for automating tasks that require the use of a command's output as input for another command.

For example, you could store the path to your current directory in a variable by applying command substitution on the pwd command, then move to another directory, and finally return to your original directory by invoking the cd command on the variable you stored, as follows:

```shell
    $ here=$(pwd)
    $ cd path_to_some_other_directory
    $ cd $here
```

### Command Line Arguments

Command line arguments are additional inputs that can be passed to a program when the program is run from a command line interface. These arguments are specified after the name of the program, and they can be used to modify the behavior of the program, provide input data, or provide output locations. Command line arguments are used to pass arguments to a shell script.

For example, the following command provides two arguments, arg1, and arg2, that can be accessed from within your Bash script:

```shell
    $ ./MyBashScript.sh arg1 arg2
```

---

## Conditionals

Conditionals, or if statements, are a way of telling a script to do something only under a specific condition.

Bash script conditionals use the following if-then-else syntax:

```shell
    if [ condition ]
    then
        statement_block_1  
    else
        statement_block_2  
    fi
```

If the condition is `true`, then Bash executes the statements in statement_block_1 before exiting the conditional block of code. After exiting, it will continue to run any commands after the closing `fi`.

Alternatively, if the condition is `false`, Bash instead runs the statements in statement_block_2 under the else line, then exits the conditional block and continues to run commands after the closing `fi`.

    Tips:

        You must always put spaces around your condition within the square brackets [ ].
        Every if condition block must be paired with a fi to tell Bash where the condition block ends.
        The else block is optional but recommended. If the condition evaluates to false without an else block, then nothing happens within the if condition block. Consider options such as echoing a comment in statement_block_2 to indicate that the condition was evaluated as false.

In the following example, the condition is checking whether the number of command-line arguments read by some Bash script, `$#`, is equal to 2.

```shell
    if [[ $# == 2 ]]
    then
      echo "number of arguments is equal to 2"
    else
      echo "number of arguments is not equal to 2"
    fi
```

Notice the use of the double square brackets, which is the syntax required for making integer comparisons in the condition `[[ $# == 2 ]]`.

You can also make string comparisons. For example, assume you have a variable called string_var that has the value "Yes" assigned to it. Then the following statement evaluates to `true`:

```shell
    `[ $string_var == "Yes" ]`
```

Notice you only need single square brackets when making string comparisons.

You can also include multiple conditions to be satified by using the "and" operator && or the "or" operator `||`. For example:

```shell
    if [ condition1 ] && [ condition2 ]
    then
        echo "conditions 1 and 2 are both true"
    else
        echo "one or both conditions are false"
    fi
```

```shell
    if [ condition1 ] || [ condition2 ]
    then
        echo "conditions 1 or 2 are true"
    else
        echo "both conditions are false"
    fi
```

## Logical operators

The following logical operators can be used to compare integers within a condition in an `if` condition block.

### ==: is equal to

If a variable a has a value of 2, the following condition evaluates to `true`; otherwise it evalutes to `false`.

```shell
    $a == 2
```

### !=: is not equal to

If a variable a has a value different from 2, the following statement evaluates to `true`. If its value is 2, then it evalutes to `false`.

```shell
    a != 2
```

    Tip: The ! logical negation operator changes true to false and false to true.

### <=: is less than or equal to

If a variable a has a value of 2, then the following statement evaluates to `true`:

```shell
    a <= 3
```

and the following statement evalutates to `false`:

```shell
    a <= 1
```

Alternatively, you can use the equivalent notation `-le` in place of `<=`:

```shell
    a=1
    b=2
    if [ $a -le $b ]
    then
       echo "a is less than or equal to b"
    else
       echo "a is not less than or equal to b"
    fi
```

We've only provided a small sampling of logical operators here. You can explore resources such as the Advanced Bash-Scripting Guide to find out more.
Arithmetic calculations

You can perform integer addition, subtraction, multiplication, and division using the notation $(()).
For example, the following two sets of commands both display the result of adding 3 and 2.

```shell
    echo $((3+2))
```

or

```shell
    a=3
    b=2
    c=$(($a+$b))
    echo $c
```

Bash natively handles integer arithmetic but does not handle floating-point arithmetic. As a result, it will always truncate the decimal portion of a calculation result.

For example:

```shell
    echo $((3/2))
```

prints the truncated integer result, 1, not the floating-point number, 1.5.

The following table summarizes the basic arithmetic operators:

| Symbol |	Operation |
| + |	addition |
| - |	subtraction |
| * |	multiplication |
| / |	division |

    Table: Arithmetic operators

## Arrays

The array is a Bash built-in data structure. An array is a space-delimited list contained in parentheses. To create an array, declare its name and contents:

```shell
    my_array=(1 2 "three" "four" 5)
```

This statement creates and populates the array my_array with the items in the parentheses: 1, 2, "three", "four", and 5.

You can also create an empty array by using:

```shell
    declare -a empty_array
```

If you want to add items to your array after creating it, you can add to your array by appending one element at a time:

```shell
    my_array+=("six")
    my_array+=(7)
```

This adds elements "six" and 7 to the array my_array.

By using indexing, you can access individual or multiple elements of an array:

```shell
    # print the first item of the array:
    echo ${my_array[0]}
    # print the third item of the array:
    echo ${my_array[2]}
    # print all array elements:
    echo ${my_array[@]}
```

    Tip: Note that array indexing starts from 0, not from 1.

## for loops

You can use a construct called a for loop along with indexing to iterate over all elements of an array.

For example, the following for loops will continue to run over and over again until every element is printed:

```shell
    for item in ${my_array[@]}; do
      echo $item
    done
```

or

```shell
    for i in ${!my_array[@]}; do
      echo ${my_array[$i]}
    done
```

The for loop requires a ; do component in order to cycle through the loop. Additionally, you need to terminate the for loop block with a done statement.

Another way to implement a for loop when you know how many iterations you want is as follows. For example, the following code prints the number 0 through 6.

```shell
    N=6
    for (( i=0; i<=$N; i++ )) ; do
      echo $i
    done
```

You can use for loops to accomplish all sorts of things. For example, you could count the number of items in an array or sum up its elements, as the following Bash script does:

```shell
    #!/usr/bin/env bash
    # initialize array, count, and sum
    my_array=(1 2 3)
    count=0
    sum=0
    for i in ${!my_array[@]}; do
      # print the ith array element
      echo ${my_array[$i]}
      # increment the count by one
      count=$(($count+1))
      # add the current value of the array to the sum
      sum=$(($sum+${my_array[$i]}))
    done
    echo $count
    echo $sum
```

---


## Cheat Sheet

### Bash shebang

```shell
    #!/bin/bash
```

### Get the path to a command

```shell
    which bash
```

### Pipes, filters, and chaining

Chain filter commands together using the pipe operator:

```shell
    ls | sort -r
```

Pipe the output of manual page for ls to head to display the first 20 lines:

```shell
    man ls | head -20
```

Use a pipeline to extract a column of names from a csv and drop duplicate names:

```shell
    cut -d "," -f1 names.csv | sort | uniq
```

### Working with shell and environment variables:

List all shell variables:

```shell
    set
```

Define a shell variable called my_planet and assign value Earth to it:

```shell
    my_planet=Earth
```

Display value of a shell variable:

```shell
    echo $my_planet
```

Reading user input into a shell variable at the command line:

```shell
    read first_name
```

    Tip: Whatever text string you enter after running this command gets stored as the value of the variable first_name.

List all environment variables:

```shell
    env
```

Environment vars: define/extend variable scope to child processes:

```shell
    export my_planet
    export my_galaxy='Milky Way'
```

### Metacharacters

Comments #:

```shell
    # The shell will not respond to this message
```

Command separator ;:

```shell
    echo 'here are some files and folders'; ls
```

File name expansion wildcard *:

```shell
    ls *.json
```

Single character wildcard ?:

```shell
    ls file_2021-06-??.json
```

### Quoting

Single quotes '' - interpret literally:

```shell
    echo 'My home directory can be accessed by entering: echo $HOME'
```

Double quotes "" - interpret literally, but evaluate metacharacters:

```shell
    echo "My home directory is $HOME"
```

Backslash \ - escape metacharacter interpretation:

```shell
    echo "This dollar sign should render: \$"
```

### I/O Redirection

Redirect output to file and overwrite any existing content:

```shell
    echo 'Write this text to file x' > x
```

Append output to file:

```shell
    echo 'Add this line to file x' >> x
```

Redirect standard error to file:

```shell
    bad_command_1 2> error.log
```

Append standard error to file:

```shell
    bad_command_2 2>> error.log
```

Redirect file contents to standard input:

```shell
    $ tr “[a-z]” “[A-Z]” < a_text_file.txt
```

The input redirection above is equivalent to:

```shell
    $cat a_text_file.txt | tr “[a-z]” “[A-Z]”
```

### Command Substitution

Capture output of a command and echo its value:

```shell
    THE_PRESENT=$(date)
    echo "There is no time like $THE_PRESENT"
```

Capture output of a command and echo its value:

```shell
    echo "There is no time like $(date)"
```

Command line arguments

```shell
    ./My_Bash_Script.sh arg1 arg2 arg3
```

### Batch vs. concurrent modes

Run commands sequentially:

```shell
    start=$(date); ./MyBigScript.sh ; end=$(date)
```

Run commands in parallel:

```shell
    ./ETL_chunk_one_on_these_nodes.sh  & ./ETL_chunk_two_on_those_nodes.sh
```

### Scheduling jobs with cron

Open crontab editor:

```shell
    crontab -e
```

Job scheduling syntax:

```shell
    m  h  dom  mon  dow   command
```

    (minute, hour, day of month, month, day of week)

    Tip: You can use the * wildcard to mean "any".

Append the date/time to a file every Sunday at 6:15 pm:

```shell
    15 18 * * 0 date >> sundays.txt
```

Run a shell script on the first minute of the first day of each month:

```shell
    1  0 1 * * ./My_Shell_Script.sh
```

Back up your home directory every Monday at 3:00 am:

```shell
    0 3 * * 1  tar -cvf my_backup_path\my_archive.tar.gz $HOME\
```

Deploy your cron job:

    Close the crontab editor and save the file.

List all cron jobs:

```shell
    crontab -l
```

### Conditionals

if-then-else syntax:

```shell
    if [[ $# == 2 ]]
    then
      echo "number of arguments is equal to 2"
    else
      echo "number of arguments is not equal to 2"
    fi
```

'and' operator &&:

```shell
    if [ condition1 ] && [ condition2 ]
```

'or' operator ||:

```shell
    if [ condition1 ] || [ condition2 ]
```

### Logical operators

| Operator | Definition                  |
| -------- | ----------                  |
| ==       | is equal to                 |
| !=       | is not equal to             |
| <        | is less than                |
| >        | is greater than             |
| <=       | is less than or equal to    |
| >=       | is greater than or equal to |


### Arithmetic calculations

Integer arithmetic notation:

```shell
    $(())
```

### Basic arithmetic operators:

| Symbol |	Operation |
| ------ | ---------- |
| + | addition        |
| - | subtraction     |
| * | multiplication  |
| / | division        |


Display the result of adding 3 and 2:

```shell
    echo $((3+2))
```

Negate a number:

```shell
    echo $((-1*-2))
```

### Arrays

Declare an array that contains items 1, 2, "three", "four", and 5:

```shell
    my_array=(1 2 "three" "four" 5)
```

Add an item to your array:

```shell
    my_array+="six"
    my_array+=7
```

Declare an array and load it with lines of text from a file:

```shell
    my_array=($(echo $(cat column.txt)))
```

### for loops

Use a for loop to iterate over values from 1 to 5:

```shell
    for i in {0..5}; do
        echo "this is iteration number $i"
    done
```

Use a for loop to print all items in an array:

```shell
    for item in ${my_array[@]}; do
      echo $item
    done
```

Use array indexing within a for loop, assuming the array has seven elements:

```shell
    for i in {0..6}; do
        echo ${my_array[$i]}
    done
```

---

## Examples

### 1. Create a Bash script file and make it executable.

```shell
echo '#!/bin/bash' > bash_script.sh
chmod u+x bash_script.sh
```

### 2. Using conditional statements and logical operators
    Create a simple Bash script containing a conditional statement to handle the following tasks: 

```shell
#!/bin/bash

echo 'Do you like Bash scripting?'
echo -n "Enter \"y\" for yes, \"n\" for no"
read response

if [ "$response" == "y" ]
then
    echo "Great!"
elif [ "$response" = "n" ]
then
   echo "Sorry!"
else
   echo "Your response must be either 'y' or 'n'."
   echo "Please re-run the script to try again."
fi
```

### 3. Performing basic mathematical calculations and numerical logical comparisons
    Create a Bash script that performs basic arithmetic calculations on two integers entered by the user. 
    Use logical comparisons to determine which calculation leads to the greatest result.

```shell
#!/bin/bash

echo -n "Enter an integer: "
read n1
echo -n "Enter another integer: "
read n2

sum=$(($n1+$n2))
product=$(($n1*$n2))

echo "The sum of $n1 and $n2 is $sum"
echo "The product of $n1 and $n2 is $product."

if [ $sum -lt $product ]
then
   echo "The sum is less than the product."
elif [[ $sum == $product ]]
then
   echo "The sum is equal to the product."
elif [ $sum -gt $product ]
then
   echo "The sum is greater than the product."
fi
```

### 4. Using arrays for storing and accessing data within for loops
    Create a report based on a supplied dataset using the CSV format. 
    Extract the columns of the dataset into separate arrays and create a new column using arithmetic and array logic.
    Combine the dataset with the new column and save the resulting report as a CSV file.

    Download the CSV data
```shell
csv_file="URL_Address"
wget $csv_file
```

    Display the CSV file to understand what it looks like
```shell
cat arrays_table.csv
```

```shell
#!/bin/bash

csv_file="./arrays_table.csv"

# parse table columns into 3 arrays
column_0=($(cut -d "," -f 1 $csv_file))
column_1=($(cut -d "," -f 2 $csv_file))
column_2=($(cut -d "," -f 3 $csv_file))

# print first array
echo "Displaying the first column:"
echo "${column_0[@]}"

## Create a new array as the difference of columns 1 and 2
# initialize array with header
column_3=("column_3")

# get the number of lines in each column
nlines=$(cat $csv_file | wc -l)
echo "There are $nlines lines in the file"

# populate the array
for ((i=1; i<$nlines; i++)); do
  column_3[$i]=$((column_2[$i] - column_1[$i]))
done
echo "${column_3[@]}"

## Combine the new array with the csv file
# first write the new array to file
# initialize the file with a header
echo "${column_3[0]}" > column_3.txt
for ((i=1; i<nlines; i++)); do
  echo "${column_3[$i]}" >> column_3.txt
done
paste -d "," $csv_file column_3.txt > report.csv
```

---

## Project: Historical Weather Forecast Comparison to Actuals

    1. Create report log and add a header
```shell
touch rx_poc.log
echo -e "year\tmonth\tday\thour\tobs_tmp\tfc_temp">rx_poc.log
```

    2. Write a bash script that downloads the raw weather data, and extracts and loads the required data
```shell
#! /bin/bash

# create a datestamped filename for the raw wttr data:
today=$(date +%Y%m%d)
weather_report=raw_data_$today

# download today's weather report from wttr.in:
city=Casablanca
curl wttr.in/$city --output $weather_report

# use command substitution to store the current day, month, and year in corresponding shell variables:
hour=$(TZ='Morocco/Casablanca' date -u +%H) 
day=$(TZ='Morocco/Casablanca' date -u +%d) 
month=$(TZ='Morocco/Casablanca' date +%m)
year=$(TZ='Morocco/Casablanca' date +%Y)

# extract all lines containing temperatures from the weather report and write to file
grep °C $weather_report > temperatures.txt

# extract the current temperature 
obs_tmp=$(head -1 temperatures.txt | tr -s " " | xargs | rev | cut -d " " -f2 | rev)

# extract the forecast for noon tomorrow
fc_temp=$(head -3 temperatures.txt | tail -1 | tr -s " " | xargs | cut -d "C" -f2 | rev | cut -d " " -f2 |rev)

# create a tab-delimited record
# recall the header was created as follows:
# header=$(echo -e "year\tmonth\tday\thour_UTC\tobs_tmp\tfc_temp")
# echo $header>rx_poc.log

record=$(echo -e "$year\t$month\t$day\t$obs_tmp\t$fc_temp")
# append the record to rx_poc.log
echo $record>>rx_poc.log
```

    3. Make the script exacutable
```shell
chmod u+x rx_poc.sh
```

---

 ## Project: Backup Shell Script
 
    Create a shell script called which runs every day and automatically backs up any encrypted password files that have been updated in the past 24 hours.
```shell
#!/bin/bash

# Check if the number of arguments is correct
if [[ $# != 2 ]]; then
  echo "Usage: $0 target_directory_name destination_directory_name"
  exit 1
fi

# Check if argument 1 and argument 2 are valid directory paths
if [[ ! -d $1 ]] || [[ ! -d $2 ]]; then
  echo "Invalid directory path provided"
  exit 1
fi

# Variables for directories
targetDirectory="$1"
destinationDirectory="$2"

# Print directories
echo "Target Directory: $targetDirectory"
echo "Destination Directory: $destinationDirectory"

# Current timestamp
currentTS=$(date +%s)

# Save original working directory
origAbsPath=$(pwd)

# Change to target directory
if ! cd "$origAbsPath/$targetDirectory"; then
  echo "Error: Unable to change directory to $targetDirectory"
  exit 1
fi

# Timestamp for 24 hours ago
yesterdayTS=$(( currentTS - 86400 ))

# Array to store files to backup
toBackup=()

# Iterate over files in target directory
for file in *; do
  if [[ -f $file ]] && (( $(date -r $file +%s) > yesterdayTS )); then
    toBackup+=("$file")
  fi
done

# Check if there are files to backup
if [[ ${#toBackup[@]} -gt 0 ]]; then
  # Create tar.gz archive in current directory
  cd "$origAbsPath"  # Return to original directory
  if ! tar -czvf "$backupFileName" -C "$targetDirectory" "${toBackup[@]}"; then
    echo "Error: Failed to create backup archive"
    exit 1
  fi
  echo "Backup archive created: $backupFileName"
else
  echo "No files to backup within the last 24 hours."
  exit 0  # Exit gracefully if no files need to be backed up
fi

# Move backup archive to destination directory
if ! mv "$backupFileName" "$destinationDirectory/"; then
  echo "Error: Failed to move backup archive to $destinationDirectory/"
  exit 1
fi
echo "Backup archive moved to: $destinationDirectory/$backupFileName"

# Script execution completed successfully
echo "Backup process completed successfully."
exit 0


```
