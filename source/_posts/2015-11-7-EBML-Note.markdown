---
layout: post
title:  "EMBL Course Note"
tags: [programming, git, python, linux]
categories: programming
---


<!-- MarkdownTOC -->

- 04112015
    - Unix Shell
- 05112015
    - part one: python
    - part two: git
    - part three: numpy and pandas
- 06112015
    - Part one: Python unit test
    - Part two: clusters
    - Part Three: web development

<!-- /MarkdownTOC -->


<a name="None"></a>
# 04112015

<a name="None"></a>
## Unix Shell

1. Useful command on file and dir

``` bash
# return the name of the user
whoami

# print current working directory
pwd

# change diretory
cd
# ./ stands for current directory
# ../ one level up
# just cd take you to the root, equal to cd ~ or cd HOME

# list files
# with -l extended
ls

# unzip zip file
unzip

# man
man

#catenate, print whatever in the file
cat

# first or last 10 line
head [-n count]
tail

# foo --help for help

# make a new directory
mkdir name

# copy files
cp [file] [target directory]

# move files
mv file1 file2 dir/
mv *.txt dir/

# remove files or dir
# -i for asking before deleting every file
# -f force
# -r recursive
rm
rmdir

# to clear
clear
```


```bash
# search in file
grep [keyword] file
touch [filename] # create a new file
wc -l # word count -line
```
___


2. pipeline

every tool is for one function and do that well

create new file by `>` (*watch out for overright*)
`>>` for append
`|` connect tools to create pipe

`grep -i author *.pdb | grep -i -c woodcock`

3. put them in a script

``` bash
#!/bin/bash
'commands goes here'
# save as foobar.sh, then run with bash
```

<a name="None"></a>
# 05112015

<a name="None"></a>
## part one: python

create virtual environment py34

``` bash
conda create -n py34 python=3.4 anaconda

source activate py34

ipython notebook
```

``` python
help()
dir()
```

glob module

``` python
import glob
files = glob.glob('*.txt')
```

strip, rstrip, lstrip

``` python
a_string = 'aaabbbaaccaa'
a_string.strip('a')  # gives 'bbbaacc' strip both sides
a_string.rstrip('a')  # gives 'aaabbaacc' strip from right
a_string.lstrip('a')  # gives 'bbaaccaa'  strip from left
```

____

<a name="None"></a>
## part two: git

[handout](https://git.embl.de/dinkel/linuxcommandline/raw/software-carpentry/git_beginner/_build/latex/gitintro.pdf)

good way of storing files with version control (changes, history, restore , conflicts and etc.)


### concept

**commit**: a commit is a recorded set of changes in your project's files. Try to group logical sets of changes together into one commit - don't mix changes are unrelated.

**repository**: a history of all commits

working dir -- stage -- repo


### config and command

``` bash
# can be set per project
git config --global user.name "name"
git config --global user.email "email@g.com"
git config --global core.editor nano
git config --global merge.tool kdiff3

git config --list
```

create local server in repos at local root, then clong into working repo

``` bash
git init --bare
```

- work flow

``` bash
git add
git commit
git status
git commit --amend
git push
```

- deal with conflict
when push, git will tell if there is any conflicts

if found, deal with the specific conflicts, then add, commit and push again

- revert to history version

``` bash
git log
git checkout <hash> [filename]
```

- branch

``` bash
git branch [new branch name]
git branch -a  # show all the branches
git checkout [branch name]

# merge master into feature than fastforward (merge back)
git merge
git branch -d [branch name]
git push -u origin [branch name] # push local branch to remote
```

____

<a name="None"></a>
## part three: numpy and pandas
[toby email](tobyhodge@embl.de)

### arrays

- basic

``` python
import numpy as np

myArray = np.array(range(10))

# filter
myArray[myArray>7]
myArray == 3
myArray[myArray.nonzero()]

# calculation on each element
myArray / 2

# calculation of array
myArray.sum()
myArray.mean()
myArray.cumsum()
myArray.var()
```

- multidimension

``` python
# multi-dimension
myMultiArray = np.array([range(i, i+3) for i in range(5)])
myMultiArray.shape

# transpose
myMultiArray.tranpose()
myMultiArray.T

# flatten
myMultiArray.flatten()

# reshape
myMultiArray.reshape(3, 5)

#dot *
arrayA * arrayB
arrayA.dot(arrayB)
```

- create array

``` python
# create array of ones or zeros for initialization
arrayOfOne = np.ones((2,2,2))
arrayOfZeros = np.zeros((2,5))

# array of range
rangeArray = np.arange(10)

# spaced array
startPoint = 5
endPoint = 30
numOfElement = 5
sapcedArray = np.linspace(startPoint, endPoint, numOfElement)

# create array from file
arrayFromFile = np.fromfile(filename, dtype, sep)
```

### matplot

for ipython `%matplotlib inline`

``` python
from matplotlib import pyplot as plt
intervals = np.linspace(0, 2*np.pi, 101)
plt.plot(intervals, np.sin(intervals))
plt.plot(intervals, np.cos(intervals))
plt.show()

plt.subplot(2,2,1) # or plt.subplot(221)
```

<a name="None"></a>
# 06112015

<a name="None"></a>
## Part one: Python unit test

### assertion

```python
assert()

```

### SOME CONCEPTS AND NOMENCLATURE
- **pre-conditions** What must be true *before* calling a function.
- **post-conditions** What will be true *after* calling a function.
- **invariants** What the function *does not* change.

### Test

#### Using nosetests
(doc of nosetests)[https://nose.readthedocs.org/en/latest/]

``` python
def add_double(x, y):
    '''Returns the double of the sum of its inputs'''
    return 2. * (x + y)

def test_smoke():
    assert add_double(1, 1) == 4
```

Now, run on the *Terminal*: `nosetest -v [scriptname]`

1. Type of cases
    - moke test just check it runs (smoke test)
    - Case testing test a "known case" (like a control in the wet lab)
    - Corner/edge cases check "complex" cases.
    - Regression testing create a test when you find a bug.
    - Integration test test that different parts work together.

2. TESTING PHILOSOPHIES
    - **test driven development** Write tests first (All code is guilty until proven innocent)
    - Test everything. Test it twice
    - To test all the codes are covered by tests: [coveralls](https://coveralls.io/)
    - Continuous integration
    - Regression testing only
    - Ad-hoc testing

3. WRITING TESTABLE CODE
    - Using testing makes your code look different
    (Mostly better, but also just different)
    - Split data-loading/computation/plotting (hard to test plotting)

<a name="None"></a>
## Part two: clusters

SGE system

### USING AN INTERACTIVE SESSION
Create a file in your home directory:
`echo "Hello World" > file.txt`
Allocate a node for computation:
`qrsh`

(On LSF, we would use `bsub -Is /bin/bash`)
We now depend on the cluster being free(ish).
Verify that your file is there. Create a new one.
Exit and verify that your new file is also there.

### RUNNING OUR FIRST JOB ON THE QUEUE
(1) Create a file called script.sh with the following content:

```bash
#!/bin/bash

echo $HOSTNAME
echo "My job ran"
```

(2) Make it executable:
`chmod +x script.sh`

(3) Submit it:
`qsub ./script.sh`

<a name="None"></a>
## Part Three: web development

flask
