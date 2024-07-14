# Refactoring 2nd edtion

## Chapter 1 - key takeaways

> The true test of good code is how easy it is to change it.

> The key to effective refactoring is recognizing that you go faster when you take tiny steps, the code is never broken, and you can compose those small steps into substantial changes. Remember that—and the rest is silence.

> When programming, follow the camping rule: Always leave the code base healthier than when you found it. It will never be perfect, but it should be better.

We have used several refactorings, including [Extract Function](https://memberservices.informit.com/my_account/webedition/9780135425664/html/extractfunction.html), [Inline Variable](https://memberservices.informit.com/my_account/webedition/9780135425664/html/inlinevariable.html), [Move Function](https://memberservices.informit.com/my_account/webedition/9780135425664/html/movefunction.html), and [Replace Conditional with Polymorphism](https://memberservices.informit.com/my_account/webedition/9780135425664/html/replaceconditionalwithpolymorphism.html).

* In the beginning my refactoring has focused on adding enough structure to the function so that I can understand it and see it in terms of its logical parts. This is often the case early in refactoring. Breaking down complicated chunks into small pieces is important, as is naming things well. Now, I can begin to focus more on the functionality change I want to make.

### Extract function

#### Motivation

The argument that makes most sense to me, however, is the separation between intention and implementation. If you have to spend effort looking at a fragment of code and figuring out *what* it's doing, then you should extract it into a function and name the function after the “what.” Then, when you read it again, the purpose of the function leaps right out at you, and most of the time you won't need to care about how the function fulfills its purpose (which is the body of the function).

Often, I see fragments of code in a larger function that start with a comment to say what they do. The comment is often a good hint for the name of the function when I extract that fragment.

#### Mechanics

> As long as I've learned something, my time wasn't wasted.

* If a variable is only used inside the extracted code but is declared outside, move the declaration into the extracted code.
* Sometimes, I find that too many local variables are being assigned by the extracted code. It's better to abandon the extraction at this point. When this happens, I consider other refactorings such as [Split Variable](https://memberservices.informit.com/my_account/webedition/9780135425664/html/splitvariable.html) or [Replace Temp with Query](https://memberservices.informit.com/my_account/webedition/9780135425664/html/replacetempwithquery.html) to simplify variable usage and revisit the extraction later.
* But that's not an option to me if I'm programming in a language that doesn't allow **nested** functions. Then I'm faced, essentially, with the problem of extracting the function to the top level, which means I have to pay attention to any variables that exist only in the scope of the source function. These are the arguments to the original function and the temporary variables defined in the function.-------
* At this point you may be wondering, “What happens if more than one variable needs to be returned?”
* Here, I have several options. Usually I prefer to pick different code to extract. I like a function to return one value, so I would try to arrange for multiple functions for the different values. If I really need to extract with multiple values, I can form a record and return that—but usually I find it better to rework the temporary variables instead. Here I like using [Replace Temp with Query](https://memberservices.informit.com/my_account/webedition/9780135425664/html/replacetempwithquery.html) and [Split Variable](https://memberservices.informit.com/my_account/webedition/9780135425664/html/splitvariable.html).
* This raises an interesting question when I'm extracting functions that I expect to then move to another context, such as top level. I prefer small steps, so my instinct is to extract into a nested function first, then move that nested function to its new context. But the tricky part of this is dealing with variables and I don't expose that difficulty until I do the move. This argues that even though I can extract into a nested function, it makes sense to extract to at least the sibling level of the source function first, so I can immediately tell if the extracted code makes sense.
* At the moment, most people underprioritize refactoring—but there still is a balance. My rule is a variation on the camping rule: Always leave the code base healthier than when you found it. It will never be perfect, but it should be better.
* I still do it at every opportunity **compile-test-commit**. I do sometimes get tired of doing it—and give mistakes the chance to bite me. Then I learn and get back into the rhythm.

### Split variables

* Any variable with more than one responsibility should be replaced with multiple variables, one for each responsibility. Using a variable for two different things is very confusing for the reader.
