
# Coding Style Guides/Best Practices

Writing readable code is an art and following style guides is one way of ensuring our code is always clean, readable and consistent. Language specific style guides exist but general coding standards apply to all programming languages. These are not to be blindly followed; strive to understand these and ask when in doubt.

## General
-   Don't duplicate the functionality of a built-in library (Note that this is not the case if the task at hand is purely algorithmic).
-   Don't swallow exceptions or "fail silently".
-   Don't write code that guesses at future functionality.
-   Exceptions should be exceptional.

## Naming

-   Make use of meaningful variable names. Avoid abbreviations.
-   Name the enumeration parameter the singular of the collection. e.g when looping over a collection of books, say for book in books and not for b in books. It’s clearer this way.
-   Name variables created by a factory after the factory (`userFactory` or `UserFactory` creates user).
-   Name variables, methods, and classes to reveal intent. Intent describes the function of the class, method or variable. A typical example is if we were describing a class method to sort a collection by a certain parameter. It’s preferred to define this as `obj.sort_by(param)` or `obj->sort_by(param)`. It makes code more readable and requires less documentation.
-   Treat acronyms as words in names (`XmlHttpRequest` not `XMLHTTPRequest`), even if the acronym is the entire name (class `Html` not class `HTML`).
-   Suffix variables holding a factory with _factory (user_factory)
    

## Formatting

-   Avoid inline comments.
-   Avoid deep indentation. (max 3 level)
-   Break long lines after 80 characters.
-   Delete trailing whitespace.
-   Don't include spaces after `(, [` or before `], ).`
-   Don't misspell.
-   Don't vertically align tokens on consecutive lines.
-   Do not leave commented out code within production code.
-   If you break up an argument list, keep the arguments on their own lines and closing parenthesis on its own line. *Example 1, Example 2.*
-   If you break up a hash/dictionary/associative array, keep the elements on their own lines and closing curly brace on its own line.
-   Indent continued lines two spaces.
-   Indent private methods equal to public methods.
-   If you break up a chain of method invocations, keep each method invocation on its own line. Place the . at the end of each line, except the last. Example.
-   Use 2 space indentation (no tabs) or indentation specific to your stack.
-   Use an empty line between methods or the required number of spaces specified in the respective style guide.
-   Use empty lines around multi-line blocks.
-   Use spaces around operators, except for unary operators, such as !.
-   Use spaces after commas, after colons and semicolons, around `{ and before }.`
-   Use Unix-style line endings `(\n).`
-   Use uppercase for SQL keywords and lowercase for SQL identifiers.
    

## Code Organization

-   Ensure to always take advantage of inbuilt language functionality like packages, modules etc. to organise the different parts of your code.
-   Order methods so that caller methods are earlier in the file than the methods they call.
-   Order methods so that methods are as close as possible to other methods they call.
-   For language specific style guides, see below.
    
*Note: It is helpful to run your code through a linter to easily help point you to areas where code is not conforming to style guides. Better still, plugins exist for various IDEs to help with this.*

JavaScript - [Guide](https://eslint.org/docs/rules/)  
HTML - [Guide](https://www.w3schools.com/html/html5_syntax.asp)   
Vue - [Guide](https://eslint.vuejs.org/rules/)  
PHP - [Guide](https://www.php-fig.org/psr/psr-2/)  
Python - [Guide](https://www.python.org/dev/peps/pep-0008/)  
Java - [Guide](https://google.github.io/styleguide/javaguide.html)  
