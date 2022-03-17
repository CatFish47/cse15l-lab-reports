# Lab Report 5 - Week 10

In this lab, we explore some of the problems with `MarkdownParse.java` found during week 9's lab.

In order to find therse problems, first, we took the output of running `MarkdownParse.java` on every test file and put them in a text file. This was done with both our own implementation of MarkdownParse as well as the MarkdownParse implementation provided that week.

All differences were found through the use of bash's `diff` method on the two files.

## The First Test

### The File

```
[link](foo
bar)
```
### Evaluating the Implementations

Our current implementation thinks that the entirety of what is in the paranthesis is a link, including the new line character.

The provided implementation thinks that there is no link.

The expected result of this one is no link. That is because it doesn't count as a link if there is a space or a new line character in the middle of the actual link. Thus, the provided implementation is correct.

### Analyzing the Bug

Since there should be no links returned that have a new line character in it or a space character in it, then the bug must be either that the code isn't checking for new line and space characters, or it is an it is broken. Judging from my `MarkdownParse.java` file, however, the bug seems to be the former.

![The bugged code](images/lab-report-5/bugged-line.png)

Above is the buggy line. As shown, it seems that the code is simply taking the substring from the open parenthesis to the close paranthesis without any checking for the validity of what is in the middle. Luckily the fix for this is relatively simple. In the code that adds the potential links to the list of links to return, simply make sure that the link doesn't contain any new line or space characters.

## The Second Test

### The File

```
[link](</my uri>)
```

### Evaluating the Implementations

Our current implementation thinks that the link is `</my uri>`.

The provided implementation thinks that there is no link.

The expected output is a singular link, `/my uri`, which makes neither implementation correct. It differs from our implementation in that there are no angle brackets, and it differs from the provided implementation in that it is indeed a link.

### Analyzing the Bug

I will be analyzing why the provided implementation finds no link at all. The problem with the provided solution is found in the lines of code as shown below.

![The bugged code](images/lab-report-5/bugged-line-2.png)

The if statement thinks that the content inside the paranthesis, `</my uri>`, is not a link because there contains a space in there. As the if statement says, it will only add the result to the list of links if there is no space found. Because of the angle brackets in the input, however, the space is allowed. Thus, while the correct output should be that there is one link, `/my uri`, this bug makes the program think there is no link.