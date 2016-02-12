# Introduction to Regular Expressions

This lesson is a derivative of this [lesson](https://github.com/ttimbers/studyGroup/blob/gh-pages/lessons/2015-07-21_regex/lesson.md) 
developed by [Bruno Grande](https://github.com/brunogrande).

## Motivation

We deal with a lot of text in our day-to-day lives, whether at work or at home. In doing 
so, we often wish to search or otherwise manipulate this text. You can typically achieve 
this manually if the amount of text isn't too large. However, this approach doesn't 
readily scale. This is where regular expressions come in. 

Regular expressions—or regex—offer a powerful means for detecting patterns in text. They 
are also flexible to match anything from the simplest pattern (*e.g.* a simple DNA motif) 
to the more complex ones (*e.g.* extracting variably formatted phone number from a block 
of text). You can either do simple searching or text manipulation. Regular expressions are
a tool that is implemented in many command line programs (e.g., grep and sed) and 
programming languages (*e.g.* python, R, perl, awk, *etc.*).

Once equipped with the knowledge of regular expressions, you'll wonder how you ever got 
things done without them. This lesson aims to teach you the way to think about regex, 
giving you a foundation you can then build upon by yourself. To this day, I still use the 
Internet to search more complex regex patterns. Once you understand the basics, it opens 
up a whole world of possibilities. 

## Setup

1. Open [regexr.com](http://regexr.com/). 
2. Erase all of the text in the "Expression" and "Text" text boxes.
3. Copy the appropriate text in the bottom text box on [regexr.com](http://regexr.com/). See each part below for the example text. 
4. Click on "flags" on the right hand side of the page and select both "global(g)" and "multiline(m)" from the dropdown menu.
5. Enjoy the lesson!

## Lesson

### Part 1: Basic Patterns

For Parts 1 and 2, we will use the following text. Please copy-paste it into the bottom 
textbox (labelled "Text") on [regexr.com](http://regexr.com/).com. 

```text
=== Parts 1 and 2 ===

TACTAGATGACTGCAGAATTCCCGACGTTAAGTACATTACCCCGTCCAGG
GCGCCGTTCAGGATCACGTTACCGCCATAAGATGGGAGGGATCCTTCTTC
TCCGCTGCGCCCACGCCAGTAGTGATTATATATACTCCTATAACCCTTCT
CCGGAGGCGGAAATCCGCCACGAATGAATTCGAGAATGTATTTCCCCGAC
AATCATTATATAGGGGCGCTCCTAAGCTTTTCCACTCGGTTGAGCCTGGT
```

A common technique in molecular biology is to digest or cut DNA with restriction enzymes. You can think of restriction enzymes as DNA scissors that only cut at specific places, depending on their recognition site. There are a great number of commercially available restriction enzymes, each with its own recognition site. I have included a few real and theoretical ones below. 

| Restriction Enzyme | Recognition Sequence |
| ------------------ | -------------------- |
| *Eco*RI            | `GAATTC`             |
| *Bam*HI            | `GGATCC`             |
| *Eco*RII           | `CCWGG`              |
| *Boom*HI           | `CTGCNNNN`           |

**Notes** 

* W = A or T 

* N = A, T, G or C

To search for *Eco*RI cutting sites, you can simply search using the recognition sequencing as the pattern. 

```text
GAATTC
```

**Warning:** Regex is case-sensitive. Accordingly, since our DNA sequence is in upper 
case, the pattern `GAATTC` will work, but not `gaattc`. 

This should unveil two *Eco*RI cutting sites. You can do the same for finding *Bam*HI 
cutting sites. 

```text
GGATCC
```

In this case, you should find one *Bam*HI cutting site. 

What if you're interested in doing a double digestion, using both *Eco*RI and *Bam*HI. In 
this case, you want to match either `GAATTC` or `GGATCC`. The "OR" operator in regex is 
the vertical bar (or pipe), `|`. Hence, here's how you can match all *Eco*RI and *Bam*HI 
cutting sites. 

```text
GAATTC|GGATCC
```

**Warning:** Whitespace in regex matters. Accordingly, if your pattern isn't matching as 
it should, make sure you don't have spaces or newlines trailing at the end. 

Now that we've found all *Eco*RI and *Bam*HI cutting sites, let's do the same for 
*Eco*RII. In this case, the recognition sequence is `CCWGG`, where W = A or T. If you were 
to search for `CCWGG`, you will notice that no matches are made. This is because regex 
isn't aware of the W convention. Instead, it's searching for the actual letter W. 

There is a way however to specify a range of possible characters to match. For this, you 
can use the square braquets notation, `[]`. In this case, you want to match `CC`, followed 
by either an A or a T, followed by `GG`. In regex, you can express this pattern as 
follows. 

```text
CC[AT]GG
```

This pattern should detect two *Eco*RII cutting sites, one being CCAGG and the other, 
CCTGG. 

If you wanted to detect *all* possible cutting sites, you can add *Eco*RII's recognition 
sequence to our list from before, namely:

```text
GAATTC|GGATCC|CC[AT]GG
```

----

#### Challenge Question 1

Refer back to the table of restriction enzymes above. Create a pattern to match *Boom*HI 
cutting sites. 

**Hint:** If you notice repetition in your pattern, you should consider using quantifiers. 
See cheat sheet or jump ahead a little to learn how to use quantifiers. 

*Answers to challenge questions are located at the bottom of this lesson.*

----

### Part 2: Quantifiers

For Part 2, we will use the same text as in Part 1 (see above). 

A common feature of DNA sequences are repeats, in which a stretch of sequence is repeated 
a variable number of times. The repeated sequence is usually relatively well defined, but 
the number of repeats can vary. 

The simplest kind of repeat is a stretch of the same nucleotide. For instance, to search 
for stretches of one or more T's, you may use the plus quantifier, `+`. This quantifier 
means that the preceding subpattern can be present one or more times. Similar quantifier 
are the asterisk, `*`, which signifies zero or more times, and the question mark, `?`, 
which signifies zero or one time.

In this case, matching stretches of one or more T's can be done as follows. 

```text
T+
```

You should see a number of matches in the DNA sequence, One wouldn't consider a single T 
to be a repeat. Let's say we want to search for two or more T's in a row. We may use the 
general quantifier notation using curly braces, `{}`. 

There are various ways of using the curly braces notation. 

* **An exact count:** `{3}` would match exactly three occurrences of the preceding subpattern.
* **A bounded range:** `{3,5}` would match three, four or five occurrences. 
* **An unbounded range:** `{3,}` would match three or more occurrences.

Hence, in our case, if we wish to match stretches of two or more T's in the DNA sequence, 
we can achieve this with the following pattern. 

```text
T{2,}
```

Now, let's match repeats of the dinucleotide TA. Unfortunately, the pattern `TA{2,}` 
doesn't work, because the quantifier only applies to the subpattern directly preceding it, 
namely `A` in this case. This pattern would match ant T following by two or more A's. 

To match TA repeats, we need to group the T and A together and apply the quantifier to 
that group. The parentheses notation, `()`, can be used to this effect. Accordinfly, our 
pattern becomes:

```text
(TA){2,}
```

As you can see, there are three instances of TA repeats in our example DNA sequence. 

----

#### Challenge Question 2

Match all of the phone number formats listed below. 

```text
415-555-1234
650-555-2345
202 555 4567
4035555678
1 416 555 9292
```

**Hint:** The question mark quantifier, `?`, mentioned earlier could be useful here. 

*Answers to challenge questions are located at the bottom of this lesson.*

----

What happens if you need to match characters that are already part of regular expression
syntax? For example, a phone number containing brackets: `(416)555-3456`

To do this we use the escape character before the the matching character, so to match 
this phone number exactly the pattern would become:

```text
\(416\)555-3456
```

#### Challenge Question 3

Modify your previous regular expression to match all of these phone number formats listed below. 

```text
415-555-1234
650-555-2345
202 555 4567
4035555678
1 416 555 9292
(416)555-3456
```

### Part 3: Character Classes

For Part 3, we will use the list of email addresses below. The first section are invalid 
emails, and the second section are valid emails. We will build our repertoire of regex 
knowledge in order to be able to only match valid emails (and exclude any invalid ones). 

```text
=== Parts 3 and 4 ===

# Valid emails
brunogrande@sfu.ca
BrunoGrande@sfu.ca
Bruno_Grande@sfu.ca
Bruno_Grande1991@sfu.ca
Bruno_Grande.1991@sfu.ca
Bruno_Grande-1991@sfu.ca
Bruno_Grande-1991@engage-sfu.ca
Bruno_Grande.1991@sfu.uk

# Invalid emails
bruno grande@sfu.ca
brunogrande@sfuca
brunogrande@-sfu.ca
brunograndesfu.ca
bruno_o'grande@sfu.ca
```

Let's start by focusing on matching the first part of valid emails. For future reference:

```text
brunogrande@sfu.ca
|---------| |----|
     |         `-> Second part
     `-> First part
```

Earlier, we saw that we can match one of many possible characters by using the square 
braquet notation, `[]`. For instance, if we wish to match a vowel, we could use `[aeiou]`. 

But what if we want to match any lower-case letter in the alphabet? The immediately 
obvious answer would be `[abcdefghijklmnopqrstuvwxyz]`. So, you can match what comes 
before the @ with:

```text
[abcdefghijklmnopqrstuvwxyz]+
```

Unfortunately, this matches any stretch of lower-case letters (one or more), while we wish 
to only match the first part of the email addresses. 

To accomplish this, we can make use of a very useful feature in regex: boundaries. There 
are a few ways of matching either line or word boundaries, as follows. 

* `^` = Line beginning
* `$` = Line ending
* `\b` = Word boundary

Knowing this, we can review our previous pattern to match any string of lower-case letters 
starting after the line beginning:

```text
^[abcdefghijklmnopqrstuvwxyz]+
```

However, we, programmers, hate typing. So, there is a shortcut in regex: you can specify 
ranges in square braquets using the hyphen, `-`. Knowing this, we can rewrite our pattern 
above as:

```text
^[a-z]+
```

This is obviously much better. Now, what if you wanted to match both lower- and upper-case 
letters? Well, you can specify more than one range between square braquets:

```text
^[A-Za-z]+
```

Now, with this previous pattern, we notice that we're starting to match email better and 
better. However, there is an obvious issue now: we need to also match the underscore, as 
it is a valid character in emails. This can be done by adding it between square braquets 
alongside the other ranges. 

```text
^[A-Za-z_]+
```

And now we need to do the same for digits:

```text
^[A-Za-z_0-9]+
```

Now, as you can imagine, this is a common pattern. We are basically matching typical word 
characters. In fact, there are a few common patterns for which "shortcuts" were created. 
Here are the main ones: 

* `\w` = `[A-za-z0-9_]` (Word characters)
* `\d` = `[0-9]` (Digits)
* `\s` = `[ \t\r\n\f]` (Whitespace characters)
* `.` = Any character except for the newline (`\n`)

With this, we can shorten our pattern to the following. Notice that we no longer need the 
square braquets, as they're implied. 

```text
^\w+
```

If we look at the email being matched, we continue to make progress. However, the period 
remains unmatched. However, you will notice that adding a period (`^\w.+`) causes every 
email to be fully matched. This is because the `+` quantifier applies to the subpattern 
immediately before, which is the period in this case. This causes the first character to 
me matched with `\w` and the rest of the emails matched with `.+`. 

ThisIn order to match either `\w` or `.`, we need to bring back the square braquets. 

```text
^[\w.]+
```

As expected, the emails containing a period are now being fully matched. However, the 
period (`.`) is supposed to match any character except for the newline. Why isn't it also 
matching the dash (and the rest of the emails, for that matter)?

Well, because the period is being used between square braquets, it's implied that you 
don't want to match anything. Otherwise, you wouldn't need the square braquets and would 
just use the period directly. So, in this case, the period is a literal period (nothing 
special about it). 

**Warning:** However, outside of square braquets, you'll need to escape the period (`\.`) 
if you wish to specifically match periods. 

If we also include the dash, our pattern now becomes:

```text
^[\w.-]+
```

We are now finally matching the first part of every valid emails. 

----

#### Challenge Question 4

Now, expand on this pattern to also match the entire emails. Remember: you wish to only 
match valid emails. Ensure that none of the invalid emails are matched. 

**Hint:** A URL (second part of emails, after the @) can consist of alphanumerical 
characters and dashes. However, a dash cannot be the first character. Also, there needs to 
be at least one period in a URL. The top-level domain (_e.g._ .com, .ca, .org) can only 
contain letters (no digits or dashes). 

*Answers to challenge questions are located at the bottom of this lesson.*

----

----

#### Challenge Question 5

Create a regex pattern that matches all of the number formats listed below. 

```text
3.14529
-255.34
128
1.9e10
123,340.00
```

*Answers to challenge questions are located at the bottom of this lesson.*

----

## Answers to Challenge Questions

#### Challenge Question 1

Based on what we learned so far, a correct pattern to match *Boom*HI cutting sites would be the following. 

```text
CTGC[ATGC][ATGC][ATGC][ATGC]
```

However, we have the same subpattern (`[ATGC]`) being repeated multiple times. In regex, we can use quantifiers to allow subpatterns to be repeated a given number of times using the curly braces notation, `{}`. In this case, we want the `[ATGC]` subpattern to be matched exactly four times. 

```text
CTGC[ATGC]{4}
```

#### Challenge Question 2

There are many approaches to solving this challenge. Here's my attempt:

```text
1? ?\d{3}[- ]?\d{3}[- ]?\d{4}
```

#### Challenge Question 3

There are many approaches to solving this challenge. Here's my attempt:

```text
1? ?\(?\d{3}\)?[- ]?\d{3}[- ]?\d{4}
```

#### Challenge Question 4

Starting with our pattern to match the emails' first part, we can match a `@` followed by 
characters making up a valid URL (`[A-Za-z0-9-]`). However, the first character of the URL 
cannot be a dash. Hence, the first subpattern after the @ cannot have the dash. 
Afterwards, you can match any of those characters (including the dash) any number of 
times. 

```text
^[\w.-]+@[A-Za-z0-9][A-Za-z0-9-]+
```

However, the previous pattern doesn't match the periods in the URL. We could add the 
period in the last range, but this makes it optional. As a result, it matches emails that 
don't have a period in the email's second part. 

To circumvent this issue, we need to specify the period outside of the square braquets 
(don't forget to escape it), followed by any number of letters. 

```text
^[\w.-]+@[A-Za-z0-9][A-Za-z0-9-]+\.[A-Za-z]+
```

And voilà! You are now only matching valid emails. 

#### Challenge Question 5

Again, there are multiple ways of solving this challenge. Here's two example solutions. 

```text
-?\d+\,?\d*\.?\d*e?\d*
```

or

```text
-?(\d+,?)+(\.\d+)?(e\d+)?
```
