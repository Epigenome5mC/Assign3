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
veronico@trgn510:~/bin/assig3 ./pointless.sh 
veronico
```

```
#!/bin/bash

#echo $HOSTNAME  # prints name of server you are on.

echo $USER
```

### 2
Please create quotequotes.sh, please add 1 additional lines that prints the process id of the current script using a special variable in a sentence: "The process id for this script is **235**'

```
veronico@trgn510:~/bin/assig3 ./quotequotes.sh 
The process ID for this script is 26615
```


```
#!/bin/bash

VARIABLE="The process ID for this script is $$"

echo $VARIABLE
```

### 3
Please create processes.sh.  Modify it such that it prints the top 5 CPU consuming processes

```
veronico@trgn510:~/bin/assig3 ./processes.sh 
  PID USER     %CPU
26279 varshagi 26.2
    1 root      0.0
    2 root      0.0
    3 root      0.0
    5 root      0.0
    
```



```
#!/bin/bash




ps -eo pid,user,%cpu --sort=-%cpu | head -6
```

### 4
Please create makeupper.sh.  Modify it to return lower case results, and change the name to makelower.sh

```
veronico@trgn510:~/bin/assig3 ./makelower.sh 
BANANA
banana
```


```
#!/bin/bash


cat /dev/stdin | tr  A-Z a-z
```

### 5
Referring to math.sh, create a script called add.sh that takes two inputs and adds them, **add.sh 5 3** would print 8

```
veronico@trgn510:~/bin/assig3 ./add.sh 4 5
The sum is = 9
```


```
#!/bin/bash


num1=$1
num2=$2

sum=$( expr "$num1" + "$num2" )
echo "The sum is = $sum"
```

### 6
Create a program "mb_or_kb.sh", referring to bigornot.sh and useful.sh, create a script called big file that checks to see if the file exists provided as the first argument exists, and if it exists then gets the filesize, storing it as a variable. I have not provided you with a way to get filesize in exercise, and expect you to search web for a way.  The program then checks to see if the size is greater than 1,000,000.  If its less then 1,000,000, it prints the number of kilobytes (divide by 1000) followed by "kB".  If its greater than 1,000,000, then print the number of megabytes followed by "MB".

```
veronico@trgn510:~/bin/assig3 ./mb_or_kb.sh 
Enter The Path of Your File: /user/veronico/bin/assig3/upper.sh
Your file is 0.275 KB in size
```

You must provide the path of the file.

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
veronico@trgn510:~/bin/assig3 ./count_by_to.sh 4 20
0
4
8
12
16
20
```


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
veronico@trgn510:~/bin/assig3 ./whatgene.sh 
mygene PTEN
mygene JUPITER
mygene BRCA1
```


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

```
veronico@trgn510:~/bin/assig3 ./genotype.sh HG001_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X-SOLID_CHROM1-X_v.3.3.2_highconf_PGandRTGphasetransfer.vcf | head
1	239339	A/G	0/1
1	239482	G/T	0/1
1	240147	C/T	0/1
1	240898	T/G	0/1
1	567239	CG/C	1|1
1	568745	C/CCA	0/1
1	837214	G/C	0|1
1	838153	CA/C	1|1
1	842825	A/G	1|1
1	845283	G/T	1|1
```


Takes about 2 minutes to load.

```
#!/bin/bash

num1=$1

awk '{OFS="\t"; if (!/^#/){print $10}}' $1 | cut -d ":" -f1 > file1.vcf

awk '{OFS="\t"; if (!/^#/){print $1,$2,$4"/"$5}}' $1 > file2.vcf

paste file2.vcf file1.vcf
```
