# Regular Expressions from "Measuring the Absolute and Relative Prevalence of SQL Concatenations and SQL-Identifier Injections"

This repository contains the regular expressions used to identify SQL concatenation in Java, PHP, and C# projects posted on GitHub. This file cannot be run individually but instead can be used as part of
our crawler program (available here: https://github.com/glaverghetta/SQLcrawler) to build the complex regular expressions needed for our analysis.

Broadly speaking, there are three "types" of regular expressions: language-specific regex (e.g., identifying Java variables), SQL regex (e.g., for identifying SQL keywords such as SELECT), and 
dynamically generated language-specific regex combines the two other categories to create complex regex that identify concatenation.

For an example of a dynamically generated regex, take the following regex expression:

```java
public static final String CONCAT_VAR = WHITESPACE + QUOTE + "(?=" + WHITESPACE + "%s" + WHITESPACE + "%s" + ")";
```

The two "%s" strings are format string placeholders that can be replaced with language-specific regex to create the complex CONCAT_VAR regex expression that recognizes one type of
concatenation with a SQL string.  In this example, the first %s is substituted with a regex recognizing the target language's concatenation symbols (e.g., "+" in Java or "." in PHP), and 
the second %s is substituted with a regex for the target language's variable names (e.g., alphanumeric strings starting with a $ in PHP).

This CONCAT_VAR regex expression can then be combined with a multitude of SQL regex to recognize concatenation in various locations. For example, by combing with the SELECT regex,
we can recognize locations where a column identifier is being concatenated into a select statement.

The regular expressions are also designed to account for other abnormalities, including unusual whitespace or string interpolation, by mixing simple regex into larger, more complex expressions.
