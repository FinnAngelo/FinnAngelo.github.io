---
layout: post
title: "SonarAnalyzer.CSharp and a monster GlobalSuppressions"
categories: Sonarqube dotnet 
published: true
---

I've been working with a new codebase and to ensure that it stays nice we are using `SonarAnalyser.CSharp` which is _frekkin' awesome!_

----------------------------------------

## TL;DR

+ [Links](#Links)
+ [Add `Directory.Build.props` with references to `SonarAnalyser.CSharp` and the shared `GlobalSupressions.cs` file](#Add-Directory-Build-props-with-references-to-SonarAnalyser-CSharp-and-the-shared-GlobalSupressionscs--file]
+ [Add `GlobalSupression.cs` file](#add-globalsupressioncs-file")
+ [Generate the `GlobalSupressions.cs` file](#Generate-the-GlobalSupressionscs-file)
+ [Credits](#Credits)

----------------------------------------

## Links

- [SonarQube docs](https://docs.sonarqube.org/latest/user-guide/rules/)
- [SonarSource Github repo](https://github.com/SonarSource)
- [SonarSource csharp rules](https://rules.sonarsource.com/csharp)

-----------------------------------------

## Add `Directory.Build.props` with references to `SonarAnalyser.CSharp` and the shared `GlobalSupressions.cs` file

Put this `Directory.Build.props` file in the same directory as the solution file

```xml
<?xml version="1.0" encoding="utf-8"?>
<Project>

  <!-- Static Analysis -->
  <PropertyGroup>
    <TreatWarningsAsErrors>True</TreatWarningsAsErrors>
    <WarningsAsErrors />
  </PropertyGroup>
    
  <ItemGroup>
    <PackageReference Include="SonarAnalyzer.CSharp" Version="8.22.0.31243">
      <PrivateAssets>all</PrivateAssets>
      <IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>
    </PackageReference>
    <Compile Include="$(SolutionDir)/GlobalSuppressions.cs" Link="GlobalSuppressions.cs" />
  </ItemGroup>

</Project>
```

----------------------------------------

## Add `GlobalSupression.cs` file

Put this monster `GlobalSupression.cs` file in the same directory as the solution file

```csharp
// This file is used by Code Analysis to maintain SuppressMessage
// attributes that are applied to this project.
// Project-level suppressions either have no target or are given
// a specific target and scoped to a namespace, type, member, etc.

using System.Diagnostics.CodeAnalysis;


```csharp
[assembly: SuppressMessage("Blocker Bug", "S1048: If Finalize or an override of Finalize throws an exception, and the runtime is not hosted by an application that overrides the default policy, th", Justification = "<Pending>")]
[assembly: SuppressMessage("Blocker Bug", "S2190: Recursion happens when control enters a loop that has no exit. This can happen a method invokes itself, when a pair of methods invoke each other,", Justification = "<Pending>")]
[assembly: SuppressMessage("Blocker Bug", "S2275: Because composite format strings are interpreted at runtime, rather than validated by the compiler, they can contain errors that lead to unexpect", Justification = "<Pending>")]
[assembly: SuppressMessage("Blocker Bug", "S2857: Badly formed SQL is likely to cause errors at runtime.", Justification = "<Pending>")]
[assembly: SuppressMessage("Blocker Bug", "S2930: When writing managed code, you don't need to worry about allocating or freeing memory: The garbage collector takes care of it. For efficiency rea", Justification = "<Pending>")]
[assembly: SuppressMessage("Blocker Bug", "S2931: An IDisposable object should be disposed (there are some rare exceptions where not disposing is fine, most notably Task). If a class has an IDisp", Justification = "<Pending>")]
[assembly: SuppressMessage("Blocker Bug", "S3464: Recursion is acceptable in methods, where you can break out of it. But with class types, you end up with code that will compile but not run if yo", Justification = "<Pending>")]
[assembly: SuppressMessage("Blocker Bug", "S3693: It may be a good idea to raise an exception in a constructor if you're unable to fully flesh the object in question, but not in an exception cons", Justification = "<Pending>")]
[assembly: SuppressMessage("Blocker Bug", "S3869: Not surprisingly, the SafeHandle.DangerousGetHandle method is dangerous. That's because it may not return a valid handle. Using it can lead to le", Justification = "<Pending>")]
[assembly: SuppressMessage("Blocker Bug", "S3889: Thread.Suspend and Thread.Resume can give unpredictable results, and both methods have been deprecated. Indeed, if Thread.Suspend is not used ver", Justification = "<Pending>")]
[assembly: SuppressMessage("Blocker Bug", "S4159: In the Attributed Programming Model, the ExportAttribute declares that a part \"exports\", or provides to the composition container, an object that", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Bug", "S2551: Shared resources should not be used for locking as it increases the chance of deadlocks. Any other thread could acquire (or attempt to acquire) t", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Bug", "S2952: It is possible in an IDisposable to call Dispose on class members from any method, but the contract of Dispose is that it will clean up all unman", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Bug", "S3449: Numbers can be shifted with the << and >> operators, but the right operand of the operation needs to be an int or a type that has an implicit con", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Bug", "S4275: Properties provide a way to enforce encapsulation by providing public, protected or internal methods that give controlled access to private field", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Bug", "S4277: Marking a class with PartCreationPolicy(CreationPolicy.Shared), which is part of Managed Extensibility Framework (MEF), means that a single, shar", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Bug", "S4583: Calling the BeginInvoke method of a delegate will allocate some resources that are only freed-up when EndInvoke is called. This is why you should", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Bug", "S4586: Returning null from a non-async Task/Task<T> method will cause a NullReferenceException at runtime. This problem can be avoided by returning Task", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Bug", "S1145: if statements with conditions that are always false have the effect of making blocks of code non-functional. if statements with conditions that a", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Bug", "S1244: Floating point math is imprecise because of the challenges of storing such values in a binary representation. Even worse, floating point math is", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Bug", "S1656: There is no reason to re-assign a variable to itself. Either this statement is redundant and should be removed, or the re-assignment is a mistake", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Bug", "S1697: When either the equality operator in a null test or the logical operator that follows it is reversed, the code has the appearance of safely null-", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Bug", "S1751: A loop with at most one iteration is equivalent to the use of an if statement to conditionally execute one piece of code. If the initial intentio", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Bug", "S1764: Using the same value on either side of a binary operator is almost always a mistake. In the case of logical operators, it is either a copy/paste", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Bug", "S1848: There is no good reason to create a new object to not do anything with it. Most of the time, this is due to a missing piece of code and so could", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Bug", "S1862: A chain of if/else if statements is evaluated from top to bottom. At most, only one branch will be executed: the first one with a condition that", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Bug", "S2114: Passing a collection as an argument to the collection's own method is either an error - some other argument was intended - or simply nonsensical", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Bug", "S2123: A value that is incremented or decremented and then not stored is at best wasted code and at worst a bug.", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Bug", "S2201: When the call to a function doesn't have any side effects, what is the point of making the call if the results are ignored? In such case, either", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Bug", "S2225: Calling ToString() on an object should always return a string. Returning null instead contravenes the method's implicit contract.", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Bug", "S2251: A for loop with a counter that moves in the wrong direction is not an infinite loop. Because of wraparound, the loop will eventually reach its st", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Bug", "S2252: If a for loop's condition is false before the first loop iteration, the loop will never be executed. Such loops are almost always bugs, particula", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Bug", "S2259: A reference to null should never be dereferenced/accessed. Doing so will cause a NullReferenceException to be thrown. At best, such an exception", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Bug", "S2583: Conditional expressions which are always true or false can lead to dead code. Such code is always buggy and should never be used in production.", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Bug", "S2688: NaN is not equal to anything, even itself. Testing for equality or inequality against NaN will yield predictable results, but probably not the on", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Bug", "S2757: The use of operators pairs ( =+, =- or =! ) where the reversed, single operator was meant (+=, -= or !=) will compile and run, but not produce th", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Bug", "S2758: When the second and third operands of a ternary operator are the same, the operator will always return the same value regardless of the condition", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Bug", "S2761: Calling the ! or ~ prefix operator twice does nothing: the second invocation undoes the first. Such mistakes are typically caused by accidentally", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Bug", "S2995: Using Object.ReferenceEquals to compare the references of two value types simply won't return the expected results most of the time because such", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Bug", "S2996: When an object has a field annotated with ThreadStatic, that field is shared within a given thread, but unique across threads. Since a class' sta", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Bug", "S2997: Typically you want to use using to create a local IDisposable variable; it will trigger disposal of the object when control passes out of the blo", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Bug", "S3005: When a non-static class field is annotated with ThreadStatic, the code seems to show that the field can have different values for different calli", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Bug", "S3168: An async method with a void return type is a \"fire and forget\" method best reserved for event handlers because there's no way to wait for the met", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Bug", "S3172: In C#, delegates can be added together to chain their execution, and subtracted to remove their execution from the chain.", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Bug", "S3244: It is possible to subscribe to events with anonymous delegates, but having done so, it is impossible to unsubscribe from them. That's because the", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Bug", "S3249: Making a base call in an overriding method is generally a good idea, but not in GetHashCode and Equals for classes that directly extend object be", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Bug", "S3263: Static field initializers are executed in the order in which they appear in the class from top to bottom. Thus, placing a static field in a class", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Bug", "S3343: Caller information attributes (CallerFilePathAttribute, CallerLineNumberAttribute, and CallerMemberNameAttribute) provide a way to get informatio", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Bug", "S3346: An assertion is a piece of code that's used during development when the compilation debug mode is activated. It allows a program to check itself", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Bug", "S3453: A class with only private constructors can't be instantiated, thus, it seems to be pointless code.", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Bug", "S3466: Generally, writing the least code that will readably do the job is a good thing, so omitting default parameter values seems to make sense. Unfort", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Bug", "S3598: When declaring a Windows Communication Foundation (WCF) OperationContract method one-way, that service method won't return any result, not even a", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Bug", "S3603: Marking a method with the Pure attribute specifies that the method doesn't make any visible changes; thus, the method should return a result, oth", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Bug", "S3610: Calling GetType() on a nullable object returns the underlying value type. Thus, comparing the returned Type object to typeof(Nullable<SomeType>)", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Bug", "S3655: Nullable value types can hold either a value or null. The value held in the nullable type can be accessed with the Value property, but .Value thr", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Bug", "S3903: Types are declared in namespaces in order to prevent name collisions and as a way to organize them into the object hierarchy. Types that are defi", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Bug", "S3923: Having all branches in a switch or if chain with the same implementation is an error. Either a copy-paste error was made and something different", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Bug", "S3926: Fields marked with System.Runtime.Serialization.OptionalFieldAttribute are serialized just like any other field. But such fields are ignored on d", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Bug", "S3927: Serialization event handlers that don't have the correct signature will simply not be called, thus bypassing any attempts to augment the automate", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Bug", "S3949: Numbers are infinite, but the types that hold them are not. Each numeric type has hard upper and lower bounds. Try to calculate or assign numbers", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Bug", "S3981: The size of a collection and the length of an array are always greater than or equal to zero. So testing that a size or length is greater than or", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Bug", "S3984: Creating a new Exception without actually throwing it is useless and is probably due to a mistake.", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Bug", "S4143: It is highly suspicious when a value is saved for a key or index and then unconditionally overwritten. Such replacements are likely errors.", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Bug", "S4210: When an assembly uses Windows Forms (classes and interfaces from the System.Windows.Forms namespace) its entry point should be marked with the ST", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Bug", "S4260: When creating a custom Markup Extension that accepts parameters in WPF, the ConstructorArgument markup must be used to identify the discrete prop", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Bug", "S4428: The PartCreationPolicyAttribute attribute, which is part of the Managed Extensibility Framework (MEF), is used to specify how the exported object", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Bug", "S1206: There is a contract between Equals(object) and GetHashCode(): If two objects are equal according to the Equals(object) method, then calling GetHa", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Bug", "S1226: While it is technically correct to assign to parameters from within method bodies, doing so before the parameter value is read is likely a bug. I", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Bug", "S2183: Shifting an integral number by 0 is equivalent to doing nothing but makes the code confusing for maintainers.", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Bug", "S2184: When division is performed on ints, the result will always be an int. You can assign that result to a double, float or decimal with automatic typ", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Bug", "S2328: GetHashCode is used to file an object in a Dictionary or Hashtable. If GetHashCode uses non-readonly fields and those fields change after the obj", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Bug", "S2345: Flags enumerations should not rely on the language to initialize the values of their members. Implicit initialization will set the first member t", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Bug", "S2674: You cannot assume that any given stream reading call will fill the byte[] passed in to the method with the number of bytes requested. Instead, yo", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Bug", "S2934: While the properties of a readonly reference type field can still be changed after initialization, those of a readonly value field, such as a str", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Bug", "S2955: When constraints have not been applied to restrict a generic type parameter to be a reference type, then a value type, such as a struct, could al", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Bug", "S3397: object.Equals() overrides can be optimized by checking first for reference equality between this and the parameter. This check can be implemented", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Bug", "S3456: ToCharArray can be omitted when the operation on the array could have been done directly on the string, such as when iterating over the character", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Bug", "S3887: Using the readonly keyword on a field means that it can't be changed after initialization. However, when applied to collections or arrays, that's", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Bug", "S4158: When a collection is empty it makes no sense to access or iterate it. Doing so anyway is surely an error; either population was accidentally omit", Justification = "<Pending>")]
[assembly: SuppressMessage("Blocker Code Smell", "S1147: Calling Environment.Exit(exitCode) or Application.Exit() terminates the process and returns an exit code to the operating system..", Justification = "<Pending>")]
[assembly: SuppressMessage("Blocker Code Smell", "S1451: Each source file should start with a header stating file ownership and the license which must be used to distribute the application.", Justification = "<Pending>")]
[assembly: SuppressMessage("Blocker Code Smell", "S2178: The use of non-short-circuit logic in a boolean context is likely a mistake - one that could cause serious program errors as conditions are evalu", Justification = "<Pending>")]
[assembly: SuppressMessage("Blocker Code Smell", "S2187: There's no point in having a test class without any test methods.This could lead a maintainer to assume a class is covered by tests even though i", Justification = "<Pending>")]
[assembly: SuppressMessage("Blocker Code Smell", "S2306: Since C# 5.0, async and await are contextual keywords. Contextual keywords do have a particular meaning in some contexts, but can still be used a", Justification = "<Pending>")]
[assembly: SuppressMessage("Blocker Code Smell", "S2368: Exposing methods with multidimensional array parameters requires developers to have advanced knowledge about the language in order to be able to", Justification = "<Pending>")]
[assembly: SuppressMessage("Blocker Code Smell", "S2387: Having a variable with the same name in two unrelated classes is fine, but do the same thing within a class hierarchy and you'll get confusion at", Justification = "<Pending>")]
[assembly: SuppressMessage("Blocker Code Smell", "S2437: Certain bit operations are just silly and should not be performed because their results are predictable.", Justification = "<Pending>")]
[assembly: SuppressMessage("Blocker Code Smell", "S2699: A test case without assertions ensures only that no exceptions are thrown. Beyond basic runnability, it ensures nothing about the behavior of the", Justification = "<Pending>")]
[assembly: SuppressMessage("Blocker Code Smell", "S2953: Dispose as a method name should be used exclusively to implement IDisposable.Dispose to prevent any confusion.", Justification = "<Pending>")]
[assembly: SuppressMessage("Blocker Code Smell", "S3060: There's no valid reason to test this with is. The only plausible explanation for such a test is that you're executing code in a parent class cond", Justification = "<Pending>")]
[assembly: SuppressMessage("Blocker Code Smell", "S3237: In property and indexer set methods, and in event add and remove methods, the implicit value parameter holds the value the accessor was called wi", Justification = "<Pending>")]
[assembly: SuppressMessage("Blocker Code Smell", "S3427: The rules for method resolution are complex and perhaps not properly understood by all coders. Having overloads with optional parameter values ma", Justification = "<Pending>")]
[assembly: SuppressMessage("Blocker Code Smell", "S3433: A method is detected as test method if marked with one of the following attributes [TestMethod] or [DataTestMethod] (for mstest), [Fact] or [Theo", Justification = "<Pending>")]
[assembly: SuppressMessage("Blocker Code Smell", "S3443: If you call GetType() on a Type variable, the return value will always be typeof(System.Type). So there's no real point in making that call. The", Justification = "<Pending>")]
[assembly: SuppressMessage("Blocker Code Smell", "S3875: The use of == to compare two objects is expected to do a reference comparison. That is, it is expected to return true if and only if they are the", Justification = "<Pending>")]
[assembly: SuppressMessage("Blocker Code Smell", "S3877: It is expected that some methods should be called with caution, but others, such as ToString, are expected to \"just work\". Throwing an exception", Justification = "<Pending>")]
[assembly: SuppressMessage("Blocker Code Smell", "S4462: Making blocking calls to async methods transforms something that was intended to be asynchronous into a synchronous block. Doing so can cause dea", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Code Smell", "S1006: Default arguments are determined by the static type of the object. If a default argument is different for a parameter in an overriding method, th", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Code Smell", "S1067: The complexity of an expression is defined by the number of &&, || and condition ? ifTrue : ifFalse operators it contains.", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Code Smell", "S1163: Throwing an exception from within a finally block will mask any exception which was previously thrown in the try or catch block, and the masked's", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Code Smell", "S1186: There are several reasons for a method not to have a method body:", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Code Smell", "S121: While not technically incorrect, the omission of curly braces can be misleading, and may lead to the introduction of errors during maintenance.", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Code Smell", "S1215: Calling GC.Collect is rarely necessary, and can significantly affect application performance. That's because it triggers a blocking operation tha", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Code Smell", "S126: This rule applies whenever an if statement is followed by one or more else if statements; the final else if should be followed by an else stateme", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Code Smell", "S131: The requirement for a final default clause is defensive programming. The clause should either take appropriate action, or contain a suitable comm", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Code Smell", "S134: Nested if, switch, for, foreach, while, do, and try statements are key ingredients for making what's known as \"Spaghetti code\".", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Code Smell", "S1541: The cyclomatic complexity of methods and properties should not exceed a defined threshold. Complex code can perform poorly and will in any case b", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Code Smell", "S1699: Calling an overridable method from a constructor could result in failures or strange behaviors when instantiating a subclass which overrides the", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Code Smell", "S1821: Nested switch structures are difficult to understand because you can easily confuse the cases of an inner switch as belonging to an outer stateme", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Code Smell", "S1944: Inappropriate casts are issues that will lead to unexpected behavior or runtime errors, such as InvalidCastExceptions. The compiler will catch ba", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Code Smell", "S1994: It can be extremely confusing when a for loop's counter is incremented outside of its increment clause. In such cases, the increment should be mo", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Code Smell", "S2197: When the modulus of a negative number is calculated, the result will either be negative or zero. Thus, comparing the modulus of a variable for eq", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Code Smell", "S2223: A static field that is neither constant nor read-only is not thread-safe. Correctly accessing these fields from different threads needs synchroni", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Code Smell", "S2290: Field-like events are events that do not have explicit add and remove methods. The compiler generates a private delegate field to back the event,", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Code Smell", "S2291: Enumerable.Sum() always executes addition in a checked context, so an OverflowException will be thrown if the value exceeds MaxValue even if an u", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Code Smell", "S2302: Because parameter names could be changed during refactoring, they should not be spelled out literally in strings. Instead, use nameof(), and the", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Code Smell", "S2330: Array covariance is the principle that if an implicit or explicit reference conversion exits from type A to B, then the same conversion exists fr", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Code Smell", "S2339: Constant members are copied at compile time to the call sites, instead of being fetched at runtime.", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Code Smell", "S2346: Consistent use of \"None\" in flags enumerations indicates that all flag values are cleared. The value 0 should not be used to indicate any other s", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Code Smell", "S2360: The overloading mechanism should be used in place of optional parameters for several reasons:", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Code Smell", "S2365: Most developers expect property access to be as efficient as field access. However, if a property returns a copy of an array or collection, it wi", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Code Smell", "S2479: Non-encoded control characters and whitespace characters are often injected in the source code because of a bad manipulation. They are either inv", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Code Smell", "S2692: Most checks against an IndexOf value compare it with -1 because 0 is a valid index. Any checks which look for values > 0 ignore the first element", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Code Smell", "S2696: Correctly updating a static field from a non-static method is tricky to get right and could easily lead to bugs if there are multiple class insta", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Code Smell", "S2701: There's no reason to use literal boolean values in assertions. Doing so is at best confusing for maintainers, and at worst a bug.", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Code Smell", "S3215: Needing to cast from an interface to a concrete type indicates that something is wrong with the abstractions in use, likely that something is mis", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Code Smell", "S3216: After an awaited Task has executed, you can continue execution in the original, calling thread or any arbitrary thread. Unless the rest of the co", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Code Smell", "S3217: The foreach statement was introduced in the C# language prior to generics to make it easier to work with the non-generic collections available at", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Code Smell", "S3218: It's possible to name the members of an inner class the same as the static members of its enclosing class - possible, but a bad idea. That's beca", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Code Smell", "S3265: enums are usually used to identify distinct elements in a set of values. However enums can be treated as bit fields and bitwise operations can be", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Code Smell", "S3353: Marking a variable that is unchanged after initialization const is an indication to future maintainers that \"no this isn't updated, and it's not", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Code Smell", "S3447: The use of ref or out in combination with [Optional] is both confusing and contradictory. [Optional] indicates that the parameter doesn't have to", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Code Smell", "S3451: The use of [DefaultValue] with [Optional] has no more effect than [Optional] alone. That's because [DefaultValue] doesn't actually do anything; i", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Code Smell", "S3600: Adding params to a method override has no effect. The compiler accepts it, but the callers won't be able to benefit from the added modifier.", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Code Smell", "S3776: Cognitive Complexity is a measure of how hard the control flow of a method is to understand. Methods with high Cognitive Complexity will be diffi", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Code Smell", "S3871: The point of having custom exception types is to convey more information than is available in standard types. But custom exception types must be", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Code Smell", "S3874: Passing a parameter by reference, which is what happens when you use the out or ref parameter modifiers, means that the method will receive a poi", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Code Smell", "S3904: If no AssemblyVersionAttribute is provided, the same default version will be used for every build. Since the version number is used by The .NET F", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Code Smell", "S3937: The use of punctuation characters to separate subgroups in a number can make the number more readable. For instance consider 1,000,000,000 versus", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Code Smell", "S3972: Code is clearest when each statement has its own line. Nonetheless, it is a common pattern to combine on the same line an if and its resulting th", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Code Smell", "S3973: In the absence of enclosing curly braces, the line immediately after a conditional is the one that is conditionally executed. By both convention", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Code Smell", "S3998: A thread acquiring a lock on an object that can be accessed across application domain boundaries runs the risk of being blocked by another thread", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Code Smell", "S4000: The IntPtr and UIntPtr types are used to access unmanaged memory, usually in order to use C or C++ libraries. If such a pointer is not secured by", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Code Smell", "S4015: Changing an inherited member to private will not prevent access to the base class implementation.", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Code Smell", "S4019: When a method in a derived class has the same name as a method in the base class but with a signature that only differs by types that are weakly", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Code Smell", "S4025: Having a field in a child class with a name that differs from a parent class' field only by capitalization is sure to cause confusion. Such child", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Code Smell", "S4039: When a base type explicitly implements a public interface method, that method is only accessible in derived types through a reference to the curr", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Code Smell", "S4487: Private fields only used to store values without reading them later is a case of dead store. So changing the value of such field is useless and m", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Code Smell", "S4524: switch can contain a default clause for various reasons: to handle unexpected values, to show that all the cases were properly considered.", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Code Smell", "S4635: Looking for a given substring starting from a specified offset can be achieved by such code: str.Substring(startIndex).IndexOf(char1). This works", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Code Smell", "S5034: ValueTask<TResult> was introduced in .NET Core 2.0 to optimize memory allocation when functions return their results synchronously.", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Code Smell", "S927: The name of a parameter in an externally visible. This rule raises an issue when method override does not match the name of the parameter in the", Justification = "<Pending>")]
[assembly: SuppressMessage("Info Code Smell", "S1135: TODO tags are commonly used to mark places where some more code is required, but which the developer wants to implement later.", Justification = "<Pending>")]
[assembly: SuppressMessage("Info Code Smell", "S1309: This rule allows you to track the usage of the SuppressMessage attributes and #pragma warning disable mechanism.", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S103: Having to scroll horizontally makes it harder to get a quick overview and understanding of any piece of code.", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S104: A source file that grows too much tends to aggregate too many responsibilities and inevitably becomes harder to understand and therefore to maint", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S106: When logging a message there are several important requirements which must be fulfilled:", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S1066: Merging collapsible if statements increases the code's readability.", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S107: A long parameter list can indicate that a new structure should be created to wrap the numerous parameters or that the function is doing too many", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S108: Most of the time a block of code is empty when a piece of code is really missing. So such empty block must be either filled or removed.", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S109: A magic number is a number that comes out of nowhere, and is directly used in a statement. Magic numbers are often used, for instance to limit th", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S110: Inheritance is certainly one of the most valuable concepts in object-oriented programming. It's a way to compartmentalize and reuse code by creat", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S1110: The use of parentheses, even those not required to enforce a desired order of operations, can clarify the intent behind a piece of code. But redu", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S1117: Overriding or shadowing a variable declared in an outer scope can strongly impact the readability, and therefore the maintainability, of a piece", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S1118: Utility classes, which are collections of static members, are not meant to be instantiated.", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S112: Throwing such general exceptions as Exception, SystemException, ApplicationException, IndexOutOfRangeException, NullReferenceException, OutOfMemo", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S1121: Assignments within sub-expressions are hard to spot and therefore make the code less readable. Ideally, sub-expressions should not have side-effe", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S1123: The Obsolete attribute can be applied with or without arguments, but marking something Obsolete without including advice as to why it's obsolete", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S1134: FIXME tags are commonly used to mark places where a bug is suspected, but which the developer wants to deal with later.", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S1144: private or internal types or private members that are never executed or referenced are dead code: unnecessary, inoperative code that should be re", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S1151: The switch statement should be used only to clearly define some new branches in the control flow. As soon as a case clause contains too many stat", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S1168: Returning null instead of an actual array or collection forces callers of the method to explicitly test for nullity, making them more complex and", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S1172: Unused parameters are misleading. Whatever the values passed to such parameters, the behavior will be the same.", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S1200: According to the Single Responsibility Principle, introduced by Robert C. Martin in his book \"Principles of Object Oriented Design\", a class shou", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S122: For better readability, do not put more than one statement on a single line.", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S125: Programmers should not comment out code as it bloats programs and reduces readability.", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S127: A for loop stop condition should test the loop counter against an invariant value (i.e. one that is true at both the beginning and ending of ever", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S138: A function that grows too large tends to aggregate too many responsibilities.", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S1479: When switch statements have large sets of case clauses, it is usually an attempt to map two sets of data. A Dictionary should be used instead to", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S1607: When a test fails due, for example, to infrastructure issues, you might want to ignore it temporarily. But without some kind of notation about wh", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S1696: NullReferenceException should be avoided, not caught. Any situation in which NullReferenceException is explicitly caught can easily be converted", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S1854: A dead store happens when a local variable is assigned a value that is not read by any subsequent instruction. Calculating or retrieving a value", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S1871: Having two cases in the same switch statement or branches in the same if structure with the same implementation is at best duplicate code, and at", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S2234: When the names of parameters in a method call match the names of the method arguments, it contributes to clearer, more readable code. However, wh", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S2326: Type parameters that aren't used are dead code, which can only distract and possibly confuse developers during maintenance. Therefore, unused typ", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S2327: When multiple, adjacent try statements have duplicate catch and/or finally blocks, they should be merged to consolidate the catch/finally logic f", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S2357: Fields should not be part of an API, and therefore should always be private. Indeed, they cannot be added to an interface for instance, and valid", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S2372: Property getters should be simple operations that are always safe to call. If exceptions need to be thrown, it is best to convert the property to", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S2376: Properties with only setters are confusing and counterintuitive. Instead, a property getter should be added if possible, or the property should b", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S2436: A method or class with too many type parameters has likely aggregated too many responsibilities and should be split.", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S2589: If a boolean expression doesn't change the evaluation of the condition, then it is entirely unnecessary, and can be removed. If it is gratuitous", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S2681: Curly braces can be omitted from a one-line block, such as with an if statement or for loop, but doing so can be misleading and induce bugs.", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S2743: A static field in a generic type is not shared among instances of different closed constructed types, thus LengthLimitedSingletonCollection<int>.", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S2933: readonly fields can only be assigned in a class constructor. If a class has a field that's not marked readonly but is only set in the constructor", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S2971: In the interests of readability, code that can be simplified should be simplified. To that end, there are several ways IEnumerable language integ", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S3010: Assigning a value to a static field in a constructor could cause unreliable behavior at runtime since it will change the value for all instances", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S3011: This rule raises an issue when reflection is used to change the visibility of a class, method or field, and when it is used to directly update a", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S3169: There's no point in chaining multiple OrderBy calls in a LINQ; only the last one will be reflected in the result because each subsequent call com", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S3246: In the interests of making code as usable as possible, interfaces and delegates with generic parameters should use the out and in modifiers when", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S3262: Overriding methods automatically inherit the params behavior. To ease readability, this modifier should be explicitly used in the overriding meth", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S3264: Events that are not invoked anywhere are dead code, and there's no good reason to keep them in the source.", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S3358: Just because you can do something, doesn't mean you should, and that's the case with nested ternary operations. Nesting ternary operators results", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S3366: In single-threaded environments, the use of this in constructors is normal, and expected. But in multi-threaded environments, it could expose par", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S3415: The standard assertions library methods such as AreEqual and AreSame in MSTest and NUnit, or Equal and Same in XUnit, expect the first argument t", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S3431: It should be clear to a casual reader what code a test is testing and what results are expected. Unfortunately, that's not usually the case with", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S3442: Since abstract classes can't be instantiated, there's no point in their having public or internal constructors. If there is basic initialization", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S3445: When rethrowing an exception, you should do it by simply calling throw; and not throw exc;, because the stack trace is reset with the second synt", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S3457: Because composite format strings are interpreted at runtime, rather than validated by the compiler, they can contain errors that lead to unexpect", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S3597: The ServiceContract attribute specifies that a class or interface defines the communication contract of a Windows Communication Foundation (WCF)", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S3880: Finalizers come with a performance cost due to the overhead of tracking the life cycle of objects. An empty one is consequently costly with no be", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S3881: The IDisposable interface is a mechanism to release unmanaged resources, if not implemented correctly this could result in resource leaks or more", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S3885: The parameter to Assembly.Load includes the full specification of the dll to be loaded. Use another method, and you might end up with a dll other", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S3898: If you're using a struct, it is likely because you're interested in performance. But by failing to implement IEquatable<T> you're loosing perform", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S3900: A publicly accessible method can be called from anywhere, which means you should validate parameters to be within the expected constraints. In ge", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S3902: Using Type.Assembly to get the current assembly is nearly free in terms of performance; it's a simple property access. On the other hand, Assembl", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S3906: Delegate event handlers (i.e. delegates used as type of an event) should have a very specific signature:", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S3908: Since .Net Framework version 2.0 it is not necessary to declare a delegate that specifies a class derived from System.EventArgs. The System.Event", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S3909: The NET Framework 2.0 introduced the generic interface System.Collections.Generic.IEnumerable<T> and it should be preferred over the older, non g", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S3925: The ISerializable interface is the mechanism to control the type serialization process. If not implemented correctly this could result in an inva", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S3928: Some constructors of the ArgumentException, ArgumentNullException, ArgumentOutOfRangeException and DuplicateWaitObjectException classes must be f", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S3956: System.Collections.Generic.List<T> is a generic collection that is designed for performance and not inheritance. For example, it does not contain", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S3966: Disposing an object twice, either with the using keyword or by calling Dispose directly, in the same method is at best confusing and at worst err", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S3971: GC.SuppressFinalize requests that the system not call the finalizer for the specified object. This should only be done when implementing Dispose", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S3990: Assemblies should conform with the Common Language Specification (CLS) in order to be usable across programming languages. To be compliant an ass", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S3992: Assemblies should explicitly indicate whether they are meant to be COM visible or not. If the ComVisibleAttribute is not present, the default is", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S3993: When defining custom attributes, System.AttributeUsageAttribute must be used to indicate where the attribute can be applied. This will determine", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S3994: String representations of URIs or URLs are prone to parsing and encoding errors which can lead to vulnerabilities. The System.Uri class is a safe", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S3995: String representations of URIs or URLs are prone to parsing and encoding errors which can lead to vulnerabilities. The System.Uri class is a safe", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S3996: String representations of URIs or URLs are prone to parsing and encoding errors which can lead to vulnerabilities. The System.Uri class is a safe", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S3997: String representations of URIs or URLs are prone to parsing and encoding errors which can lead to vulnerabilities. The System.Uri class is a safe", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S4002: This rule raises an issue when a disposable type contains fields of the following types and does not implement a finalizer:", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S4004: A writable collection property can be replaced by a completely different collection. Making it readonly prevents that while still allowing indivi", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S4005: String representations of URIs or URLs are prone to parsing and encoding errors which can lead to vulnerabilities. The System.Uri class is a safe", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S4016: If an enum member's name contains the word \"reserved\" it implies it is not currently used and will be change in the future. However changing an e", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S4017: A nested type is a type argument that is also a generic type. Calling a method with such a nested type argument requires complicated and confusin", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S4035: When a class implements the IEquatable<T> interface, it enters a contract that, in effect, states \"I know how to compare two instances of type T", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S4050: When implementing operator overloads, it is very important to make sure that all related operators and methods are consistent in their implementa", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S4055: String literals embedded in the source code will not be localized properly.", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S4057: When you create a DataTable or DataSet, you should set the locale explicitly. By default, the locale for these types is the current culture. For", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S4059: Properties and Get method should have names that makes them clearly distinguishable.", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S4070: This rule raises an issue when an externally visible enumeration is marked with FlagsAttribute and one, or more, of its values is not a power of", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S4142: There are valid cases for passing a variable multiple times into the same method call, but usually doing so is a mistake, and something else was", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S4144: When two methods have the same implementation, either it was a mistake - something else was intended - or the duplication was intentional, but ma", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S4200: Native methods are functions that reside in libraries outside the virtual machine. Being able to call them is useful for interoperability with ap", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S4214: Methods marked with the System.Runtime.InteropServices.DllImportAttribute attribute use Platform Invocation Services to access unmanaged code and", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S4220: When raising an event, two arguments are expected by the EventHandler delegate: Sender and event-data. There are three guidelines regarding these", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S4456: Because of the way yield methods are rewritten by the compiler (they become lazily evaluated state machines) any exceptions thrown during the par", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S4457: Because of the way async/await methods are rewritten by the compiler, any exceptions thrown during the parameters check will happen only when the", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S4581: When the syntax new Guid() (i.e. parameterless instantiation) is used, it must be that one of three things is wanted:", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S881: The use of increment and decrement operators in method calls or in combination with other arithmetic operators is not recommended, because:", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Code Smell", "S907: goto is an unstructured control flow statement. It makes code less readable and maintainable. Structured control flow statements such as if, for,", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S100: Shared naming conventions allow teams to collaborate efficiently. This rule checks whether or not method and property names are PascalCased. To r", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S101: Shared naming conventions allow teams to collaborate efficiently. This rule checks whether or not type names are using PascalCase. To reduce nois", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S105: Developers should not need to configure the tab width of their text editors in order to be able to read source code.", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S1075: Hardcoding a URI makes it difficult to test a program: path literals are not always portable across operating systems, a given absolute path may", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S1104: Public fields in public classes do not respect the encapsulation principle and has three main disadvantages:", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S1109: Shared coding conventions make it possible for a team to efficiently collaborate. This rule makes it mandatory to place a close curly brace at th", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S1116: Empty statements, i.e. ;, are usually introduced by mistake, for example because:", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S1125: Redundant Boolean literals should be removed from expressions to improve readability.", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S1128: Although unnecessary using won't change anything to the produced application, removing them:", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S113: Some tools work better when files end with an empty line.", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S1155: Using .Count() to test for emptiness works, but using .Any() makes the intent clearer, and the code more readable. However, there are some cases", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S1185: Overriding a method just to call the same method from the base class without performing any other actions is useless and misleading. The only tim", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S1192: Duplicated string literals make the process of refactoring error-prone, since you must be sure to update all occurrences.", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S1199: Nested code blocks can be used to create a new scope and restrict the visibility of the variables defined inside it. Using this feature in a meth", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S1210: When you implement IComparable or IComparable<T> on a class you should also override Equals(object) and overload the comparison operators (==, !=", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S1227: break; is an unstructured control flow statement which makes code harder to read.", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S1264: When only the condition expression is defined in a for loop, and the initialization and increment expressions are missing, a while loop should be", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S1301: switch statements and expressions are useful when there are many different cases depending on the value of the same expression.", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S1449: string.ToLower(), ToUpper, IndexOf, LastIndexOf, and Compare are all culture-dependent, as are some (floating point number and DateTime-related)", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S1450: When the value of a private field is always assigned to in a class' methods before being read, then it is not being used to store class informati", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S1481: If a local variable is declared but not used, it is dead code and should be removed. Doing so will improve maintainability because developers wil", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S1643: StringBuilder is more efficient than string concatenation, especially when the operator is repeated over and over as in loops.", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S1659: Declaring multiple variable on one line is difficult to read.", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S1694: The purpose of an abstract class is to provide some heritable behaviors while also defining methods which must be implemented by sub-classes.", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S1698: Using the equality == and inequality != operators to compare two objects generally works. The operators can be overloaded, and therefore the comp", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S1858: Invoking a method designed to return a string representation of an object which is already a string is a waste of keystrokes. Similarly, explicit", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S1905: Unnecessary casting expressions make the code harder to read and understand.", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S1939: An inheritance list entry is redundant if:", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S1940: It is needlessly complex to invert the result of a boolean comparison. The opposite comparison should be made instead.", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S2148: Beginning with C# 7, it is possible to add underscores ('_') to numeric literals to enhance readability. The addition of underscores in this mann", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S2156: The difference between private and protected visibility is that child classes can see and use protected members, but they cannot see private ones", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S2219: To check the type of an object there are several options:", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S2221: Catching System.Exception seems like an efficient way to handle multiple possible exceptions. Unfortunately, it traps all exception types, includ", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S2292: Trivial properties, which include no logic but setting and getting a backing field should be converted to auto-implemented properties, yielding c", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S2325: Methods and properties that don't access instance data can be static to prevent any misunderstanding about the contract of the method.", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S2333: Unnecessary keywords simply clutter the code and should be removed. Specifically:", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S2342: Shared naming conventions allow teams to collaborate efficiently. This rule checks that all enum names match a provided regular expression.", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S2344: The information that an enumeration type is actually an enumeration or a set of flags should not be duplicated in its name.", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S2386: public static mutable fields of classes which are accessed directly should be protected to the degree possible. This can be done by reducing the", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S2486: When exceptions occur, it is usually a bad idea to simply ignore them. Instead, it is better to handle them properly, or at least to log them.", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S2737: A catch clause that only rethrows the caught exception has the same effect as omitting the catch altogether and letting it bubble up automaticall", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S2760: When the same condition is checked twice in a row, it is either confusing - why have separate checks? - or an error - some other condition should", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S3052: The compiler automatically initializes class fields, auto-properties and events to their default values before setting them with any initializati", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S3220: The rules for method resolution are complex and perhaps not properly understood by all coders. The params keyword can make method declarations ov", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S3234: GC.SuppressFinalize asks the Common Language Runtime not to call the finalizer of an object. This is useful when implementing the dispose pattern", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S3235: Redundant parentheses are simply wasted keystrokes, and should be removed.", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S3236: Caller information attributes: CallerFilePathAttribute and CallerLineNumberAttribute provide a way to get information about the caller of a metho", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S3240: In the interests of keeping code clean, the simplest possible conditional syntax should be used. That means", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S3241: Private methods are clearly intended for use only within their own scope. When such methods return values that are never used by any of their cal", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S3242: When a derived type is used as a parameter instead of the base type, it limits the uses of the method. If the additional functionality that is pr", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S3247: Because the is operator performs a cast if the object is not null, using is to check type and then casting the same argument to that type, necess", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S3251: partial methods allow an increased degree of flexibility in programming a system. Hooks can be added to generated code by invoking methods that d", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S3253: Since the compiler will automatically invoke the base type's no-argument constructor, there's no need to specify its invocation explicitly. Also,", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S3254: Specifying the default parameter values in a method call is redundant. Such values should be omitted in the interests of readability.", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S3256: Using string.Equals to determine if a string is empty is significantly slower than using string.IsNullOrEmpty() or checking for string.Length ==", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S3257: Unnecessarily verbose declarations and initializations make it harder to read the code, and should be simplified.", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S3261: Namespaces with no lines of code clutter a project and should be removed.", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S3376: Adherence to the standard naming conventions makes your code not only more readable, but more usable. For instance, class FirstAttribute : Attrib", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S3400: There's no point in forcing the overhead of a method call for a method that always returns the same constant value. Even worse, the fact that a m", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S3440: There's no point in checking a variable against the value you're about to assign it. Save the cycles and lines of code, and simply perform the as", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S3441: When an anonymous type's properties are copied from properties or variables with the same names, it yields cleaner code to omit the new type's pr", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S3444: When an interface inherits from two interfaces that both define a member with the same name, trying to access that member through the derived int", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S3450: There is no point in providing a default value for a parameter if callers are required to provide a value for it anyway. Thus, [DefaultParameterV", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S3458: Empty case clauses that fall through to the default are useless. Whether or not such a case is present, the default clause will be invoked. Such", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S3459: Fields and auto-properties that are never assigned to hold the default values for their types. They are either pointless code or, more likely, mi", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S3532: The default clause should take appropriate action. Having an empty default is a waste of keystrokes.", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S3604: Fields, properties and events can be initialized either inline or in the constructor. Initializing them inline and in the constructor at the same", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S3626: Jump statements, such as return, yield break, goto, and continue let you change the default flow of program execution, but jump statements that d", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S3717: NotImplementedException is often used to mark methods which must be implemented for the overall functionality to be complete, but which the devel", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S3872: The name of a method should communicate what it does, and the names of its parameters should indicate how they're used. If a method and its param", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S3876: Strings and integral types are typically used as indexers. When some other type is required, it typically indicates design problems, and potentia", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S3897: The IEquatable<T> interface has only one method in it: Equals(<T>). If you've already written Equals(T), there's no reason not to explicitly impl", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S3962: The value of a static readonly field is computed at runtime while the value of a const field is calculated at compile time, which improves perfor", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S3963: When a static constructor serves no other purpose that initializing static fields, it comes with an unnecessary performance cost because the comp", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S3967: A jagged array is an array whose elements are arrays. It is recommended over a multidimensional array because the arrays that make up the element", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S4018: The best way to determine the type of a generic method is by inference based on the type of argument that is passed to the method. This is not po", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S4022: By default the storage type of an enum is Int32. In most cases it is not necessary to change this. In particular you will not achieve any perform", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S4023: Empty interfaces are usually used as a marker or a way to identify groups of types. The preferred way to achieve this is to use custom attributes", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S4026: It is important to inform the ResourceManager of the language used to display the resources of the neutral culture for an assembly. This improves", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S4027: Exceptions types should provide the following constructors:", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S4040: Certain characters, once normalized to lowercase, cannot make a round trip. That is, they can not be converted from one locale to another and the", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S4041: When a type name matches the name of a publicly defined namespace, for instance one in the .NET framework class library, it leads to confusion an", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S4047: When a reference parameter (keyword ref) is used, the passed argument type must exactly match the reference parameter type. This means that to be", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S4049: Properties are accessed like fields which makes them easier to use.", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S4052: With the advent of .NET framework version 2, certain practices have become obsolete.", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S4056: When a System.Globalization.CultureInfo or IFormatProvider object is not supplied, the default value that is supplied by the overloaded member mi", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S4058: Many string operations, the Compare and Equals methods in particular, provide an overload that accepts a StringComparison enumeration value as a", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S4060: The .NET framework class library provides methods for retrieving custom attributes. Sealing the attribute eliminates the search through the inher", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S4061: A method using the VarArgs calling convention is not Common Language Specification (CLS) compliant and might not be accessible across programming", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S4069: Operator overloading is convenient but unfortunately not portable across languages. To be able to access the same functionality from another lang", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S4136: For clarity, all overloads of the same method should be grouped together. That lets both users and maintainers quickly understand all the current", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S4201: There's no need to null test in conjunction with an is test. null is not an instance of anything, so a null check is redundant.", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S4225: Creating an extension method that extends object is not recommended because it makes the method available on every type. Extensions should be app", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S4226: It makes little sense to create an extension method when it is possible to just add that method to the class itself.", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S4261: According to the Task-based Asynchronous Pattern (TAP), methods returning either a System.Threading.Tasks.Task or a System.Threading.Tasks.Task<T", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Code Smell", "S818: Using upper case literal suffixes removes the potential ambiguity between \"1\" (digit 1) and \"l\" (letter el) for declaring literals.", Justification = "<Pending>")]
[assembly: SuppressMessage("Blocker Security Hotspot", "S2068: Because it is easy to extract strings from an application source code or binary, credentials should not be hard-coded. This is particularly true", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Security Hotspot", "S2245: Using pseudorandom number generators (PRNGs) is security-sensitive. For example, it has led in the past to the following vulnerabilities:", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Security Hotspot", "S2257: The use of a non-standard algorithm is dangerous because a determined attacker may be able to break the algorithm and compromise whatever data ha", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Security Hotspot", "S4502: A cross-site request forgery (CSRF) attack occurs when a trusted user of a web application can be forced, by an attacker, to perform sensitive ac", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Security Hotspot", "S4784: Using regular expressions is security-sensitive. It has led in the past to the following vulnerabilities:", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Security Hotspot", "S4787: Encrypting data is security-sensitive. It has led in the past to the following vulnerabilities:", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Security Hotspot", "S4790: Cryptographic hash algorithms such as MD2, MD4, MD5, MD6, HAVAL-128, HMAC-MD5, DSA (which uses SHA-1), RIPEMD, RIPEMD-128, RIPEMD-160, HMACRIPEMD", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Security Hotspot", "S4792: Configuring loggers is security-sensitive. It has led in the past to the following vulnerabilities:", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Security Hotspot", "S4818: Using sockets is security-sensitive. It has led in the past to the following vulnerabilities:", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Security Hotspot", "S4823: Using command line arguments is security-sensitive. It has led in the past to the following vulnerabilities:", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Security Hotspot", "S4829: Reading Standard Input is security-sensitive. It has led in the past to the following vulnerabilities:", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Security Hotspot", "S5042: Successful Zip Bomb attacks occur when an application expands untrusted archive files without controlling the size of the expanded data, which ca", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Security Hotspot", "S5332: Clear-text protocols as ftp, telnet or non secure http are lacking encryption of transported data. They are also missing the capability to build", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Security Hotspot", "S5443: Operating systems have global directories where any user has write access. Those folders are mostly used as temporary storage areas like /tmp in", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Security Hotspot", "S1313: Hardcoding IP addresses is security-sensitive. It has led in the past to the following vulnerabilities:", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Security Hotspot", "S2077: Formatted SQL queries can be difficult to maintain, debug and can increase the risk of SQL injection when concatenating untrusted values into the", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Security Hotspot", "S2612: In Unix, \"others\" class refers to all users except the owner of the file and the members of the group assigned to this file.", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Security Hotspot", "S5693: Rejecting requests with significant content length is a good practice to control the network traffic intensity and thus resource consumption in o", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Security Hotspot", "S5753: ASP.NET 1.1+ comes with a feature called Request Validation, preventing the server to accept content containing un-encoded HTML. This feature com", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Security Hotspot", "S5766: Deserialization process extracts data from the serialized representation of an object and reconstruct it directly, without calling constructors.", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Security Hotspot", "S2092: When a cookie is protected with the secure attribute set to true it will not be send by the browser over an unencrypted HTTP request and thus can", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Security Hotspot", "S2255: Using cookies is security-sensitive. It has led in the past to the following vulnerabilities:", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Security Hotspot", "S3330: When a cookie is configured with the HttpOnly attribute set to true, the browser guaranties that no client-side script will be able to read it. I", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Security Hotspot", "S4036: When executing an OS command and unless you specify the full path to the executable, then the locations in your application's PATH environment va", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Security Hotspot", "S4507: Delivering code in production with debug features activated is security-sensitive. It has led in the past to the following vulnerabilities:", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Security Hotspot", "S4834: The access control of an application must be properly implemented in order to restrict access to resources to authorized entities otherwise this", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Security Hotspot", "S5122: Having a permissive Cross-Origin Resource Sharing policy is security-sensitive. It has led in the past to the following vulnerabilities:", Justification = "<Pending>")]
[assembly: SuppressMessage("Blocker Vulnerability", "S2115: When relying on the password authentication mode for the database connection, a secure password should be chosen.", Justification = "<Pending>")]
[assembly: SuppressMessage("Blocker Vulnerability", "S2278: According to the US National Institute of Standards and Technology (NIST), the Data Encryption Standard (DES) is no longer considered secure:", Justification = "<Pending>")]
[assembly: SuppressMessage("Blocker Vulnerability", "S2755: XML specification allows the use of entities that can be internal or external (file system / network access ...) which could lead to vulnerabilit", Justification = "<Pending>")]
[assembly: SuppressMessage("Blocker Vulnerability", "S3884: CoSetProxyBlanket and CoInitializeSecurity both work to set the permissions context in which the process invoked immediately after is executed. C", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Vulnerability", "S2053: In cryptography, a \"salt\" is an extra piece of data which is included when hashing a password. This makes rainbow-table attacks more difficult. U", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Vulnerability", "S2070: The MD5 algorithm and its successor, SHA-1, are no longer considered secure, because it is too easy to create hash collisions with them. That is,", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Vulnerability", "S3329: When encrypting data with the Cipher Block Chaining (CBC) mode an Initialization Vector (IV) is used to randomize the encryption, ie under a give", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Vulnerability", "S4423: Older versions of SSL/TLS protocol like \"SSLv3\" have been proven to be insecure.", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Vulnerability", "S4426: Most of cryptographic systems require a sufficient key size to be robust against brute-force attacks.", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Vulnerability", "S4432: Encryption algorithms can be used with various modes. Some combinations are not secured:", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Vulnerability", "S4433: An LDAP client authenticates to an LDAP server with a \"bind request\" which provides, among other, a simple authentication method.", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Vulnerability", "S4830: Validation of X.509 certificates is essential to create secure SSL/TLS sessions not vulnerable to man-in-the-middle attacks.", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Vulnerability", "S5445: Creating temporary files using insecure methods exposes the application to race conditions on filenames: a malicious user can try to create a fil", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Vulnerability", "S5542: Encryption operation mode and the padding scheme should be chosen appropriately to guarantee data confidentiality, integrity and authenticity:", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Vulnerability", "S5547: Strong cipher algorithms are cryptographic systems resistant to cryptanalysis, they are not vulnerable to well-known attacks like brute force att", Justification = "<Pending>")]
[assembly: SuppressMessage("Critical Vulnerability", "S5659: If a JSON Web Token (JWT) is not signed with a strong cipher algorithm (or not signed at all) an attacker can forge it and impersonate user ident", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Vulnerability", "S4211: Transparency attributes, SecurityCriticalAttribute and SecuritySafeCriticalAttribute are used to identify code that performs security-critical op", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Vulnerability", "S4212: Because serialization constructors allocate and initialize objects, security checks that are present on regular constructors must also be present", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Vulnerability", "S4564: ASP.Net has a feature to validate HTTP requests to prevent potentially dangerous content to perform a cross-site scripting (XSS) attack. There is", Justification = "<Pending>")]
[assembly: SuppressMessage("Major Vulnerability", "S5773: During the deserialization process, the state of an object will be reconstructed from the serialized data stream which can contain dangerous oper", Justification = "<Pending>")]
[assembly: SuppressMessage("Minor Vulnerability", "S2228: Debug statements are always useful during development. But include them in production code - particularly in code that runs client-side - and you", Justification = "<Pending>")]

```

----------------------------------------

## Generate the `GlobalSupressions.cs` file

I generated the code for the `GlobalSupressions.cs` file from the [SonarAnalyzer.CSharp resource strings on GitHub](https://github.com/SonarSource/sonar-dotnet/blob/master/analyzers/src/SonarAnalyzer.CSharp/RspecStrings.resx)...  
Only a crazy person would have done that manually!

```csharp
using System;
using System.IO;
using System.Linq;
using System.Text.RegularExpressions;
using System.Xml.Linq;

namespace TrashMe.MakeSionarAnalyserSuppressions
{
    static class Program
    {
        private const string SOLUTION_DIR = @"C:\Path\To\My\SonarSource\";
        static void Main(string[] args)
        {
            string xmlString = File.ReadAllText(SOLUTION_DIR + "RspecStrings.resx");
            XDocument XDoc = XDocument.Parse(xmlString);

            var descriptionNodes = (
                from xEl in XDoc.Descendants("data")
                where xEl.FirstAttribute.Value.EndsWith("_Description")
                select new
                {
                    CheckId = xEl.FirstAttribute.Value.Replace("_Description", string.Empty).Trim(),
                    Description = Regex.Replace(
                            xEl.Value.Substring(0, Math.Min(xEl.Value.Length, 150)), 
                            @"\p{C}+", 
                            string.Empty
                        ).Replace(@"""", @"\""")
                        .Trim(),
                    xEl
                }
                ).ToList();

            var categoryNodes = (
                from xEl in XDoc.Descendants("data")
                where xEl.FirstAttribute.Value.EndsWith("_Category")
                select new
                {
                    CheckId = xEl.FirstAttribute.Value.Replace("_Category", string.Empty).Trim(),
                    Category = xEl.Value.Trim(),
                    xEl
                }
                ).ToList();

            var typeNodes = (
                from xEl in XDoc.Descendants("data")
                where xEl.FirstAttribute.Value.EndsWith("_Type")
                select new
                {
                    CheckId = xEl.FirstAttribute.Value.Replace("_Type", string.Empty).Trim(),
                    Type = xEl.Value.Trim(),
                    xEl
                }
                ).ToList();

            var resultNodes =
                from d in descriptionNodes
                join c in categoryNodes
                    on d.CheckId equals c.CheckId
                join t in typeNodes
                    on d.CheckId equals t.CheckId
                orderby t.Type, c.Category, d.CheckId, d.Description
                select new
                {
                    c.Category,
                    d.CheckId,
                    d.Description,
                    Suppression = $"[assembly: SuppressMessage(\"{c.Category}\", \"{d.CheckId}: {d.Description}\", Justification = \"<Pending>\")]"
                };


            var result = string.Join(
                    Environment.NewLine,
                    resultNodes.Select(r => r.Suppression)
                    );

            Console.WriteLine(result);
        }
    }
}

```

----------------------------------------

## Credits ##

As I said, SonarQube, SonarAnalyser and SonarLint are brilliant. 

If you find this useful, go be nice to someone. Pay it forward.

_Cheers!_
