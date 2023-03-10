# Validating with Regular Expressions

## Learning Goals

- Learn about the `Pattern` class in the `java.util.regex` package.
- Discuss how to validate input using regular expressions.

## Introduction

Now that we know a little bit more about regular expressions and how to write
them, we can actually use them to validate a `String` object in Java! But first,
we will look at a class called `Pattern` in Java!

## The `Pattern` Class

The Java `Pattern` class holds a compiled representation of a regular
expression. To make use of the regular expressions we learned in the last
lesson, we will first need to compile them as a `Pattern` instance to use them
in Java. Let's look at how we can construct a `Pattern` instance:

```java
import java.util.regex.Pattern;

public class RegexExample {
    public static void main(String[] args) {
        Pattern regexPattern = Pattern.compile("[Hh]m?");
        System.out.println(regexPattern.pattern());
    }
}
```

Notice how we can call the static `compile()` method and provide our regex as
a `String` to instantiate the `Pattern` object. If we want to get the pattern,
we can call the `pattern()` method which will return a `String` representation
of the regular expression:

```text
[Hh]m?
```

Now what if we want to see if the regular expression we provided matches a
given text? We can use the static `matches()` method in the `Pattern` class! The
`matches()` method takes in two parameters: the regular expression pattern we
are looking for and the test string. If it finds the regular expression in the
test string, then the method will return true, otherwise, it will return false.

```java
import java.util.regex.Pattern;

public class RegexExample {
    public static void main(String[] args) {
        boolean isMatch = Pattern.matches("[Hh]m?", "Hm");
        System.out.println(isMatch);
    }
}
```

The regex used in this example is the same regex we used in the last lesson!
Here we, are looking for a pattern where there is either an `H` or `h` followed
by zero or one `m` occurrences. If we provide a test string of `Hm`, we should
return true!

```text
true
```

But if we modified the code above to this:

```java
import java.util.regex.Pattern;

public class RegexExample {
    public static void main(String[] args) {
        boolean isMatch = Pattern.matches("[Hh]m?", "Hmm");
        System.out.println(isMatch);
    }
}
```

This should return false since there is more than one `m` occurrence.

## Validating with Regex

The `Pattern` class' `matches()` method is a perfect way to validate the user's
input is in the correct format!

Let's look at an example where we are verifying if someone typed in a 10-digit
phone number in the proper format. We will accept either of these formats:

- (XXX) XXX-XXXX
- (XXX)XXX-XXXX

Let's also assume any of the digits (0-9) are acceptable phone numbers too for
now!

Before we write the code, go ahead into Regex 101 and see if you can write a
regular expression pattern that would match either of these formats! One hint is
you will need to escape the parentheses. To do so, place a `\` in front of them.
So: `\(` and `\)`. If you need an example of a test string, you can use
`(555) 555-1234`. Try not to look at the code below when you write the regex!

Were you able to come up with a regular expression pattern? Great if you got it!

Now we'll look at some code to see how we can validate a phone number in Java:

```java
import java.util.regex.Pattern;

public class RegexExample {
    public static void main(String[] args) {
        String phoneNumber1 = "(555) 555-1234";
        String phoneNumber2 = "(555)555-1234";
        String phoneNumber3 = "555-555-1234";
        String phoneNumber4 = "5555551234";
        String phoneNumber5 = "(555) 555-12345";

        String phonePattern = "\\(\\d\\d\\d\\) ?\\d\\d\\d-\\d\\d\\d\\d";
        System.out.println(Pattern.matches(phonePattern, phoneNumber1));
        System.out.println(Pattern.matches(phonePattern, phoneNumber2));
        System.out.println(Pattern.matches(phonePattern, phoneNumber3));
        System.out.println(Pattern.matches(phonePattern, phoneNumber4));
        System.out.println(Pattern.matches(phonePattern, phoneNumber5));
    }
}
```

If we were to run the code above, the result would look like this:

```text
true
true
false
false
false
```

We can see the first two phone numbers are valid given the format and the last
three phone numbers are invalid due to their formatting, or they have too many
numbers.

Let us walk through this code a little more now:

- Create 5 phone number examples.
- Create the phone number pattern using a regular expression.
  - Notice the extra backslashes in `"\\(\\d\\d\\d\\) ?\\d\\d\\d-\\d\\d\\d\\d"`.
    - When we were writing our regular expression patterns in the last lesson,
      we only needed one backslash in front of the `d` to show that we are
      looking for a digit from 0-9.
    - This is where we need to remember our escape sequences from the Characters
      and Strings lesson from the last module. In Java, the backslash tells Java
      we are going to be entering in an escape sequence and to escape the
      following character. But there is no `\d` in Java. Therefore, we need to use
      the escape sequence `\\` to tell Java we really mean to put a backslash
      here.
    - So if we were to actually print the `String` object with the value
      "\\d\\d\\d", it would really print `\d\d\d` since it escaped the `\`
      character.
- Call the `matches()` method with the `phonePattern` and each of the phone
  number examples.
- Print the results to see if there are any matches.

Another valid regular expression we could use is:
`"\\(\\d{3}\\) ?\\d{3}-\\d{4}"`. Instead of writing out `\\d` three or four
times, we can match the exact token by placing a quantifier directly after the
token we're trying to match. If we use this regular expression instead in our
`RegexExample` program:

```java
import java.util.regex.Pattern;

public class RegexExample {
    public static void main(String[] args) {
        String phoneNumber1 = "(555) 555-1234";
        String phoneNumber2 = "(555)555-1234";
        String phoneNumber3 = "555-555-1234";
        String phoneNumber4 = "5555551234";
        String phoneNumber5 = "(555) 555-12345";

        String phonePattern = "\\(\\d{3}\\) ?\\d{3}-\\d{4}";
        System.out.println(Pattern.matches(phonePattern, phoneNumber1));
        System.out.println(Pattern.matches(phonePattern, phoneNumber2));
        System.out.println(Pattern.matches(phonePattern, phoneNumber3));
        System.out.println(Pattern.matches(phonePattern, phoneNumber4));
        System.out.println(Pattern.matches(phonePattern, phoneNumber5));
    }
}
```

The code above will now print out the same output that we saw before:

```text
true
true
false
false
false
```

## Resources

- [Regex 101](https://regex101.com/)
- [Java 17 Pattern Class](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/regex/Pattern.html)
