# C# Code Style

## Naming convention

We've got most of naming conventions from default R# rules.

### Type definitions

| Name      | Template           | Example          | Description               |
| --------- | ------------------ | ---------------- | ------------------------- |
| Interface | I\<PascalCaseName> | IMyConverter     | Singular noun.            |
| Interface | I\<PascalCaseName>\<postfix> <br> \<postfix>: \<s(es)\|List\|Array\|Collection\|Dict\|…>          | IMyProductList   | When describing a contract as a custom collection, the postfix that defines the type of collection must be added to the name.<br> If the collection type cannot be determined then name must be plural noun. |
| Class         | \<PascalCaseName>                     | MyConverter       | Singular noun.            |
| Class         | \<PascalCaseName>\<postfix> <br> \<postfix>: \<s(es)\|List\|Array\|Collection\|Dict\|…>          | MyProductList   | When implementing a class as a custom collection, the postfix that defines the type of collection must be added to the name.<br> If the collection type cannot be determined then the name must be a plural noun. |
| Structure     | \<PascalCaseName>                     | MyStructure       | Singular noun.            |
| Enum          | \<PascalCaseName>                     | Status            | [Singular noun for Enums. Plural noun for Flags.](https://docs.microsoft.com/en-us/dotnet/standard/design-guidelines/names-of-classes-structs-and-interfaces?redirectedfrom=MSDN#naming-enumerations) |
| Generic Type parameter | T\<PascalCaseName>                     | IConverter<**TIn**, **TOut**> | Singular noun. If there is only one Type parameter and there is no constraints (`where` statement) you can name this type as just `T`. |
| Custom attribute class | \<PascalCaseName>Attribute | RequiredAttribute | Singular noun. |

### Type members (strict)

| Name | Template | Example | Description |
| ---- | -------- | ------- | ----------- |
| private field (any: const, readonly, static) | _\<camelCaseName> | _myField | Singular noun. Plural noun for collections. |
| protected / internal / public field | \<PascalCaseName> | MyField | Singular noun. Plural noun for collections. |
| Property | \<PascalCaseName> | MyProperty | Singular noun. Plural noun for collections.  |
| Method   | \<PascalCaseName> | Execute    | Must begin with a verb and denote an action. |
| Asynchronous Method   | \<PascalCaseName>Async | GetReportAsync    | Must begin with a verb and denote an action. |
| Enum member   | \<PascalCaseName> | InProgress    | |

## Type members order

Here is the recommended order for type members but you can change this order in the team if every team member agrees:

* Fields
* Properties
* Constructors
* Methods

## Code comments

* Summary comments are required for public interfaces, classes, methods, and properties.
* The documentation text within the summary tag must end with a period.
* Here is a list of possible summaries for constructors (update your [R# code template](https://www.jetbrains.com/resharper/features/code_templates.html) (ReSharper → Tools → Templates explorer) to put this comment automatically when you write `ctor` + TAB in Visual Studio (or [create your own VS snippet](https://www.telerik.com/blogs/visual-studio-tip-creating-your-own-code-snippets))):
  * Initializes a new instance.
  * Initializes a new instance of the MyClass.
  * Initializes a new instance of the \<see cref="MyClass"/> class.

**Important to note:**

* The comment should not be only for comment. The comment should give a basic idea of the interface, class, etc. without learning the implementation. (Why and what it does or what it describes).
* Has updated code - update comment.
* Comments for interfaces. Its methods and properties should describe the contract, not the implementation.
* If there are more than one constructors you can describe them more detailed, for understanding the difference of initialization by different constructors.
* You can omit the comment of the method result part if the comment to the method itself gives an understanding about the result.
* You can omit the comment of the method parameters if all the parameter names are obvious to understand. If at least one parameter is not obvious, then it is necessary to specify comments for all params.
* You can use [the following extension VS 2017/2019](https://marketplace.visualstudio.com/items?itemName=MattLaceyLtd.CollapseComments&ssr=false#overview) for collapsing only comments (Ctrl+M, Ctrl+C).

## Spaces / Tabs / Empty lines

* We use tabs to indent. So please configure Visual Studio to use tabs by default for all .cs files. In order to do this:
  * You have to open 'Tools' → 'Options' menu item;
  * Expand 'Text editor' → 'C#' → 'Tabs';
  * Set 'Tab size' and 'Indent size' to '4'. Mark 'Keep tabs' radio button.
  * Save these settings.
* If you have multi-line sentence, new lines should start with a single-tab indent. The exception could be made for multi-line string constants.
* The code must not contain multiple blank lines in a row ([StyleCop SA1507 rule](https://github.com/Visual-Stylecop/Visual-StyleCop/wiki/SA1507)).
* A closing curly bracket must not be preceded by a blank line ([StyleCop SA1508 rule](https://github.com/Visual-Stylecop/Visual-StyleCop/wiki/SA1508)).
* An opening curly bracket must not be followed by a blank line ([StyleCop SA1509 rule](https://github.com/Visual-Stylecop/Visual-StyleCop/wiki/SA1509)).
* You must preserve an empty line at the end of a file.
* There are a lot of other agreements with spaces. All of them configured in ReSharper settings, so please use them and don't forget to press `Ctrl+Alt+Enter` for automatic reformating the code.

## Curly braces

* Opening and closing brackets must be placed on its own line.
* Always put curly braces to `for`, `foreach`, `try`-`catch`-`finaly`, etc statements even if you have just one line.
* As for `if` statement:
  * Curly braces are always required in case of body with multiple lines even if it's just one sentence (for example `throw` statement splitted to multiple lines).
  * Curly braces are always required if any block of an `if` / `else if` / ... / `else` compound statement uses braces or if a single statement body spans multiple lines.
  * Actually recommendation is to always use curly braces. But as for `if(...) return;`, `if(...) break;`, `if(...) throw ...;` statements you could change this rule if everybody in the team agrees with it.

## Usage of the var keyword

Use `var` keyword everywhere as the compiler will allow. If variable type is not obvious (due to call a method) and you suppose that it's better for understanding to write specific type - do it.

## General recommendations

This section contains recommendations for writing C# code. There are always a lot of exceptions on each of this recommendation but please try to follow these rules and use your best judgment when you decide to not use this recommendation.

### Write code not for yourself, write code for colleagues

You write once, and the rest of us read it dozens of times (the first will be the reviewer). Show your mannerliness and make it easier for colleagues :-). Strive to write a linear code. If it is not, strive to describe in comments why this is done. For example, you have implemented some design patterns (not linear), in this case, you can write to a summary comment the name of the generally known design pattern. It helps your reader.

### Internal instead Public

In projects (especially in libraries), strive to set the internal access modifier for type definitions, if the type is not really for public use (in other assemblies). This will allow a more clear separation public code from the internal one and safely change it for the external environment.

Note: In the MVC application, the controllers must be public, respectively, everything that is injected into the constructor (DI) of controllers must also be public. The solution is to make the interfaces as public and leave the implementation as internal.

### DI: Check that injected dependencies not null and write it to the private readonly field

It is necessary to check all injected dependencies for null in the body of the constructor. This will allow eliminating NullReffenceExceptions while the application is running since exceptions will throw at the stage of application initialization by the IOC container when the application is started.

All injected dependencies must be written to private readonly fields.

### Do not use the property as method

Calculable properties should not contain heavy operations.

https://docs.microsoft.com/en-us/visualstudio/code-quality/ca1024-use-properties-where-appropriate?view=vs-2019

### Method: common input parameter, and specific result

The input parameters should be the most common, and the output params the most narrow. But! You should not convert the result for this, for example, you should not convert IEnumerable to a List, and you should not return a List as IEnumerable.

https://docs.microsoft.com/en-us/visualstudio/code-quality/ca1011-consider-passing-base-types-as-parameters?view=vs-2019

### Beware of static

You should not create static types, fields, properties, and methods for no apparent reason. Complicates testing and reusing, increases code coupling and other common causes.

The exception may be a private static method: https://docs.microsoft.com/en-us/visualstudio/code-quality/ca1822-mark-members-as-static?view=vs-2019

### File name == Class name

The file name must match the name of the class, interface, structure, or enumeration. One file - one type definition.

In case of generic types, put a name of the type (without generic parameters) to the file name. But in case you have two classes, one is generic and one is not, then use the syntax [StyleCop is suggesting](https://github.com/DotNetAnalyzers/StyleCopAnalyzers/blob/master/documentation/SA1649.md) - `Class1{T}.cs`.

### Method parameters ordering

Parameters in methods and constructors should be sorted by importance (as possible).

For example:

* `void ChangeCampaignTitle(string locale, string title, int id)` – wrong.
* `void ChangeCampaignTitle(int id, string locale, string title)` – right.

### Try to avoid a Helper classes

As a rule, a Helper class is so abstract that it can have methods of any kind. It is not at all clear what this class is responsible for. And over time it grows to incredible sizes. Also, be careful with the name Manager, as this is rather abstract and can eventually turn into a helper :-)

Below is a table of some type names and their methods for replacing Helper classes:

| Name       | Methods                            | Description                                                     |
| ---------- | ---------------------------------- | --------------------------------------------------------------- |
| Provider   | Get                                | Get some value(s).                                              |
| Factory    | Create                             | Create instances.                                               |
| Validator  | Validate                           | Validate model and throw an exception if validation failed.     |
| Checker    | Check                              | Checks model and return bool type result checking.              |
| Builder    | Build(BuildPart), GetResult        | Builder design pattern.                                         |
| Repository | Get, Save, Update, Delete          | Methods for data access of DB (for example).                    |
| Command    | Execute, Undo                      | Command design pattern.                                         |
| Strategy   | Execute                            | Strategy design pattern.                                        |
| Visitor    | Visit                              | Visitor design pattern.                                         |
| Composite  | AddChild, RemoveChild, GetChild, … | Composite design pattern.                                       |
| Mapper     | Map                                | Maps fields from source model to destination model.             |
| Converter  | Convert                            | Creates the destination model from the source model.            |
| Extensions |                                    | The name of the type for which the extensions are written, then the postfix Extensions. The exception could be made for interfaces. Extensions for different types must be in different files. Example: MyClassExtensions. |

### Avoid out and ref parameters

https://docs.microsoft.com/en-us/visualstudio/code-quality/ca1021-avoid-out-parameters?view=vs-2019

https://docs.microsoft.com/en-us/visualstudio/code-quality/ca1045-do-not-pass-types-by-reference?view=vs-2019

### Avoid empty interfaces

The interface should declare any members or implement two or more other interfaces.

If your design includes empty interfaces that types are expected to implement, you are probably using an interface as a marker or a way to identify a group of types. If this identification will occur at run time, the correct way to accomplish this is to use a custom attribute. Use the presence or absence of the attribute, or the properties of the attribute, to identify the target types. If the identification must occur at compile time, then it is acceptable to use an empty interface.

https://docs.microsoft.com/en-us/visualstudio/code-quality/ca1040-avoid-empty-interfaces?view=vs-2019

### Properties should not be write-only

https://docs.microsoft.com/en-us/visualstudio/code-quality/ca1044-properties-should-not-be-write-only?view=vs-2019

### Do not declare visible instance fields (applicable for classes only)

https://docs.microsoft.com/en-us/visualstudio/code-quality/ca1051-do-not-declare-visible-instance-fields?view=vs-2019

Additional recommendation: use only private instance fields.

### Avoid excessive inheritance

https://docs.microsoft.com/en-us/visualstudio/code-quality/ca1501-avoid-excessive-inheritance?view=vs-2019

### Avoid excessive class coupling

https://docs.microsoft.com/en-us/visualstudio/code-quality/ca1506-avoid-excessive-class-coupling?view=vs-2019

### Rethrow to preserve stack details

Use `throw;` instead `throw e;`

https://docs.microsoft.com/en-us/visualstudio/code-quality/ca2200-rethrow-to-preserve-stack-details?view=vs-2019

### Do not raise reserved exception types

The following exception types are too general to provide sufficient information to the user:

* System.Exception
* System.ApplicationException
* System.SystemException

The following exception types are reserved and should be thrown only by the common language runtime:

* System.ExecutionEngineException
* System.IndexOutOfRangeException
* System.NullReferenceException
* System.OutOfMemoryException

https://docs.microsoft.com/en-us/visualstudio/code-quality/ca2201-do-not-raise-reserved-exception-types?view=vs-2019

### Do not raise exceptions in exception clauses

Shouldn't throw an exception from a finally, filter, or fault clause (catch). When an exception is raised in an exception clause, it significantly increases the difficulty of debugging.

https://docs.microsoft.com/en-us/visualstudio/code-quality/ca2219-do-not-raise-exceptions-in-exception-clauses?view=vs-2019

### Best practices for exceptions

Here is [the detailed page on best practices for exceptions](https://docs.microsoft.com/en-us/dotnet/standard/exceptions/best-practices-for-exceptions).

A list of recommendations (read details by the link above):
* Use try/catch/finally blocks to recover from errors or release resources;
* Handle common conditions without throwing exceptions;
* Design classes so that exceptions can be avoided;
* Throw exceptions instead of returning an error code;
* Use the predefined .NET exception types;
* End exception class names with the word Exception;
* Use grammatically correct error messages;
* In custom exceptions, provide additional properties as needed;
* Restore state when methods don't complete due to exceptions.


### Override GetHashCode on overriding Equals

https://docs.microsoft.com/en-us/visualstudio/code-quality/ca2218-override-gethashcode-on-overriding-equals?view=vs-2019

### Do not return `null` for collections

Return an empty collection instead of `null`. This eliminates the need to check the collection for `null`, and then for emptiness. In model classes collections must be initialized by default to empty collections.

### When the method returns more than one variable use tuples

When we want to return more than one variable from the method we have these options:

* Use out or ref parameters;
* Use an anonymous object or create a specific class or struct for this case;
* Use tuples.

In the most likely cases, we need to return more than one variable in some private logic. And for this case, a better solution is tuples, as it's a built-in feature in C# language. Also, starting from C#7 we can use named tuples and destructuring. This makes the code more clean and better understandable.

### Remove unused code

Always remove the code if you are sure that it is not used. Never commit commented code as Git features can be used to restore it.

## Recommendations for Code Review

### Break feature updates and refactoring into separate pushes

For a more convenient Code Review try to break your features into separate pushes: feature updates, code reformat and refactoring. It is desirable that the feature update be the first commit or the last.
Note: Create PR and after that do several git push actions because if you do this before PR creation it will look for everybody as just one update.

### In case of new files

The most complete observance of Code Style / Code Design is necessary.

### In case of changed files

All changes must be within Code Style / Code Design. Also, to achieve the goals described above, it is desirable that the modified file after the change becomes at least a little bit closer to our Code Style / Code Design. But refactoring should be refactoring, i.e. do not change the work logic the application as a whole.

> Thanks for reading this.
