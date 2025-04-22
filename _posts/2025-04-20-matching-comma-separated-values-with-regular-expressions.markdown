---
layout: post-with-tags
title:  "Matching comma-separated values with regular expressions"
date:   2025-04-20
tags:   regexp
---
<figure>
	<img src="{% link assets/csv_regexp.png %}" class="article-image" style="object-position: 0% center" />
	<figcaption>Ernst Florens Friedrich Chladni, illustration in 'Entdeckungen über die Theorie des Klanges'</figcaption>
</figure>

Some days ago I had to move some data from a MySQL database to Google BigQuery. What was stored as a string of values separated by commas in the former was expected as an array type in the latter.
And so, as anyone with a problem on their hands, I turned to regular expressions.

## A naive, but working solution

Just so we get a feel for the problem we're trying to solve, let's say this is what was stored in the MySQL database:

```
foo,bar,1.2
```

and I was expecting to match `foo`, `bar` and `1.2` so I could transform them to an array using the [REGEXP_EXTRACT_ALL](https://cloud.google.com/bigquery/docs/reference/standard-sql/string_functions#regexp_extract_all) function.

After some minutes of mucking about I had something working:

```
(?<=^|,).*?(?=$|,)
```

There are three parts to this regular expression:

1. `(?<=^|,)` This finds the beginning boundary of the field we're trying to match. It's a [positive lookbehind](/2024/03/31/positive-and-negative-lookahead-and-lookbehind.html#-positive-lookbehind): find before the current position either the beginning of the string (`^`) or a comma (`,`). 
2. `.*?` This is the actual field we're interested in matching. It matches any number of characters, lazily. 
3. `(?=$|,)` This finds the ending boundary of the field we're trying to match. It's a [positive lookahead](/2024/03/31/positive-and-negative-lookahead-and-lookbehind.html#-positive-lookahead): find after the current position either the end of the string (`$`) or a comma (`,`).

## Refining the solution

The previous regular expression works, and is indeed what I used to solve my problem. But the problem (matching a comma-separated series of values) seemed interesting enough to dig a bit deeper, and I found some problems to my initial approach:

- Part 2 (`.*?`) takes a lot of steps: every time it matches one character it tries to move forward towards part 3 (`(?=$|,)`), as it matches as lazily as possible. Each time it realizes it cannot move forward (as it finds neither the end of the string (`$`) nor a comma (`,`)) it has to go back. This gets worse as the length of the field grows.
	- It can be replaced by `[^,]*`. This will match greedily, but stop as soon as it hits a comma (`,`). This way we're still sure it only matches the contents of a field.

Our regexp now looks like this:
```
(?<=^|,)[^,]*(?=$|,)
```

- [RFC 4180 establishes](https://www.rfc-editor.org/rfc/rfc4180#page-2) that we should accept fields enclosed by double quotes (`"`). Our new part 2 (`[^,]*`) does capture them but also includes the double quotation marks (e.g. instead of capturing `Hello` it captures `"Hello"`).
	- The way to solve this is to add an alternative that _matches_ the enclosing double quotation marks but only _captures_ what's inside the quotes, like this: `"([^"]*)"`. We then put both `"([^"]*)"` and `[^,]*` inside a non-capturing group.
	- This, as a nice bonus, also lets us have commas (`,`) inside our double quotes enclosed fields.

> ⚠ The RFC also specifies that double quotation marks (`"`) can be used within already double quoted fields if they are preceded by an extra double quotation mark, e.g. `foo,"foo ""and"" bar",bar`.

> This is an exercise I leave for a later time, or to the reader.

This is now our regexp:
```
(?<=^|,)(?:"([^"]*)"|([^,]*))(?=$|,)
```

- Part 3 is not really necessary. We can also simplify part 1 to a non-capturing group like so `(?:^|,)`.
	- In addition, we can also save some steps by switching the alternatives around like this `(?:,|^)`, as most of the time we'll find a comma (`,`) and not the beginning of the string (`^`).

The final version of the regexp:
```
(?:,|^)(?:"([^"]*)"|([^,]*))
```

There are only two parts:

1. `(?:,|^)` This finds the beginning boundary of the field we're trying to match. It's a non-capturing group that finds either a comma (`,`) or the beginning of the string (`^`).
2. `(?:"([^"]*)"|([^,]*))` This is the actual field we're interested in matching. It's a non-capturing group with two alternatives:
	1. `"([^"]*)"` Match a quotation mark (`"`), any number of characters that are *not* a quotation mark, and then another quotation mark.
	2. `([^,]*)` Match any number of characters that are not a comma (`,`).