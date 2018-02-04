# Assign3
## Spring 2018

The purpose of this exercise is to get comfortable creating scripts using vim on a server, while teaching you the remainder of bash scripting you'll need as a foundation for this course. The scripts themselves are not particularly exciting but they are the building blocks you'll need.  You will be asked to create 11 scripts, which all come from http://dtg.usc.edu/tgrn510/index.php/bash-scripting/.  These are to all be placed in your "bin" directory. The results of running these should be put into a forked version of this github. 

The final question is considerably more difficult and requires you to put concepts together you have learned at this point.

 Completion means the scripts work in your home directory and you have put the results in your github where you will replace the placeholder text with the results from running the script. For exampler, you should replace

*REPLACE WITH RESULTS*

with codeblock text using backticks, which show both the program being called in trgn510.pmed.io and the result.  After, it should be:

`
davidcraig@trgn510:~/bin ls | test.sh
TEST.SH
`

### 1
Please create pointless.sh, changing from printing your hostname with $HOSTNAME, to your $USER

```
#!/bin/bash

#echo $HOSTNAME  # prints name of server you are on.

echo $USER
```

### 2
Please create quotequotes.sh, please add 1 additional lines that prints the process id of the current script using a special variable in a sentence: "The process id for this script is **235**'

```
#!/bin/bash

VARIABLE="The process ID for this script is $$"

echo $VARIABLE
```

### 3
Please create processes.sh.  Modify it such that it prints the top 5 CPU consuming processes

```
#!/bin/bash




ps -eo pid,user,%cpu --sort=-%cpu | head -6
```

### 4
Please create makeupper.sh.  Modify it to return lower case results, and change the name to makelower.sh

```
#!/bin/bash


cat /dev/stdin | tr  A-Z a-z
```

### 5
Referring to math.sh, create a script called add.sh that takes two inputs and adds them, **add.sh 5 3** would print 8

```
#!/bin/bash


num1=$1
num2=$2

sum=$( expr "$num1" + "$num2" )
echo "The sum is = $sum"
```

### 6
Create a program "mb_or_kb.sh", referring to bigornot.sh and useful.sh, create a script called big file that checks to see if the file exists provided as the first argument exists, and if it exists then gets the filesize, storing it as a variable. I have not provided you with a way to get filesize in exercise, and expect you to search web for a way.  The program then checks to see if the size is greater than 1,000,000.  If its less then 1,000,000, it prints the number of kilobytes (divide by 1000) followed by "kB".  If its greater than 1,000,000, then print the number of megabytes followed by "MB".

#You must provide the path of the file.

```
#!/bin/bash

# get filename
echo -n "Enter The Path of Your File: "
fileName=$path
read fileName

# make sure file exits for reading
if [ ! -f $fileName ]; then
  echo "Filename $fileName does not exists."
  exit 1
fi


sz=$(stat -c '%s' $fileName)
if [ $sz -lt 1000000 ]; then
    a=$( awk "BEGIN {print $sz / 1000.0}" )
    echo "Your file is $a KB in size"
elif [ $sz -gt 1000000 ]; then
    echo "Your file is $sz MB in size"
fi
```

### 7
Create a program "count_by_to.sh", referring to count.sh.  The file should take two arguments, and should count jumping by the first argument until the second argument is reached, starting at 0.  For example, *count.sh 2 10* would print 0 2 4 6 8 10

```
#!/bin/sh

 x=0

 while ( test $x -le $2 ); do
  echo $x
  x=$(($x+$1))

 done
 ```

### 9
Please create whatgene.sh.  Please edit such that the function print_gene, prints upper case of the input.

```
#!/bin/bash
# Setting a return status for a function
print_gene () {
    echo -n mygene; echo -n " " ; echo $1 | tr a-z A-Z
    return 1
}
print_gene PTEN
print_gene Jupiter
print_gene brca1
```

### 10
Please create a bash shell called "genotype.sh" that takes a VCF as argument 1, and prints space delimited chromosome, position, reference, alternative, and genotype for all genotypes in VCF.

#Takes about 2 minutes to load.

```
#!/bin/bash

num1=$1

awk '{OFS="\t"; if (!/^#/){print $10}}' $1 | cut -d ":" -f1 > file1.vcf

awk '{OFS="\t"; if (!/^#/){print $1,$2,$4"/"$5}}' $1 > file2.vcf

paste file2.vcf file1.vcf
```
