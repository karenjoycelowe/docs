---
title: "?? and ??= operators - null-coalescing operators"
description: "The `??` and `??=` operators are the C# null-coalescing operators. They return the value of the left-hand operand if it isn't null. Otherwise, they return the value of the right-hand operand"
ms.date: 11/28/2022
f1_keywords:
  - "??_CSharpKeyword"
  - "??=_CSharpKeyword"
helpviewer_keywords:
  - "null-coalescing operator [C#]"
  - "?? operator [C#]"
  - "null-coalescing assignment [C#]"
  - "??= operator [C#]"
ms.assetid: 088b1f0d-c1af-4fe1-b4b8-196fd5ea9132
---
# ?? and ??= operators - the null-coalescing operators

<!--
  Note: All the remaining Acrolinx issues in this article are because of the `??` and `??=` operator. Acrolinx believes it's a misspelling of the ? mark.
-->

Null handling is a common requirement in software development. C# offers powerful tools to manage nulls, including the null-coalescing operator (??) and the null-coalescing assignment operator (??=).

## Null-coalescing operator (??)

The null-coalescing operator `??` returns the value of its left-hand operand only if the left-hand operand is not `null`; otherwise, it evaluates and returns the right-hand operand. If the left-hand operand is not null, the `??` operator does not evaluate the right-hand operand.

Null-Coalescing Operator (??)
The null-coalescing operator ?? returns the value of its left-hand operand if it isn’t null; otherwise, it evaluates the right-hand operand and returns its result. The ?? operator doesn’t evaluate its right-hand operand if the left-hand operand evaluates to non-null.

  ```csharp
  List<int>? numbers = null;
  int? a = null;
  ```

Console.WriteLine((numbers is null)); // expected: true
// If numbers is null, initialize it. Then, add 5 to numbers
(numbers ??= new List<int>()).Add(5);
Console.WriteLine(string.Join(" ", numbers));  // output: 5
Console.WriteLine((numbers is null)); // expected: false        

Console.WriteLine((a is null)); // expected: true
Console.WriteLine((a ?? 3)); // expected: 3 since a is still null 
// If a is null, assign 0 to a and add a to the list
numbers.Add(a ??= 0);
Console.WriteLine((a is null)); // expected: false        
Console.WriteLine(string.Join(" ", numbers));  // output: 5 0
Console.WriteLine(a);  // output: 0

## Null-coalescing assignment operator (??=)

The null-coalescing assignment operator `??=` assigns the value of its right-hand operand to its left-hand operand only if the left-hand operand is `null`. If the left-hand operand is not null, the `??=` operator does not evaluate its right-hand operand.

  ```csharp
  int? b = null;
  b ??= 10;
  Console.WriteLine(b); // output: 10
  ```


[!code-csharp[null-coalescing assignment](snippets/shared/NullCoalescingOperator.cs#Assignment)]

## Important points
* The left-hand operand of the `??=` operator must be a variable, a [property](../../programming-guide/classes-and-structs/properties.md), or an [indexer](../../programming-guide/indexers/index.md) element.

* The left-hand operand of the `??` and `??=` operators must be a nullable type or a reference type; it cannot be a non-nullable value type. Notably, you can use the null-coalescing operators with unconstrained type parameters:

[!code-csharp[unconstrained type parameter](snippets/shared/NullCoalescingOperator.cs#UnconstrainedType)]

* The null-coalescing operators are right-associative. That is, expressions of the form

```csharp
a ?? b ?? c
d ??= e ??= f
```

are evaluated as

```csharp
a ?? (b ?? c)
d ??= (e ??= f)
```

## Common use cases

The `??` and `??=` operators can be useful in various scenarios:

### Using `??` with null-conditional operators
In expressions with the [null-conditional operators `?.` and `?[]`](member-access-operators.md#null-conditional-operators--and-), you can use the `??` operator to provide an alternative expression to evaluate in case the result of the expression with null-conditional operations is `null`:

  [!code-csharp-interactive[with null-conditional](snippets/shared/NullCoalescingOperator.cs#WithNullConditional)]

### Handling nullable value types
When you work with [nullable value types](../builtin-types/nullable-value-types.md) and need to provide a value of an underlying value type, use the `??` operator to specify the value to provide in case a nullable type value is `null`:

  [!code-csharp-interactive[with nullable types](snippets/shared/NullCoalescingOperator.cs#WithNullableTypes)]

Alternatively, use the <xref:System.Nullable%601.GetValueOrDefault?displayProperty=nameWithType> method if the value to be used when a nullable type value is `null` should be the default value of the underlying value type.

### Using ?? with `throw expressions
You can use a [`throw` expression](../statements/exception-handling-statements.md#the-throw-expression) as the right-hand operand of the `??` operator to make argument-checking code more concise:

  [!code-csharp[with throw expression](snippets/shared/NullCoalescingOperator.cs#WithThrowExpression)]

  The preceding example also demonstrates how to use [expression-bodied members](../../programming-guide/statements-expressions-operators/expression-bodied-members.md) to define a property.

### Simplifying null cChecks with ??=
You can use the `??=` operator to replace code of the form:

  ```csharp
  if (variable is null)
  {
      variable = expression;
  }
  ```

  with the following code:

  ```csharp
  variable ??= expression;
  ```

## Operator overloadability

The operators `??` and `??=` cannot be overloaded.

## C# language specification

For more information about the `??` operator, see [The null coalescing operator](~/_csharpstandard/standard/expressions.md#1215-the-null-coalescing-operator) section of the [C# language specification](~/_csharpstandard/standard/README.md).

For more information about the `??=` operator, see the [feature proposal note](~/_csharplang/proposals/csharp-8.0/null-coalescing-assignment.md).

## See also

- [Null check can be simplified (IDE0029, IDE0030, and IDE0270)](../../../fundamentals/code-analysis/style-rules/ide0029-ide0030-ide0270.md)
- [C# operators and expressions](index.md)
- [?. and ?[] operators](member-access-operators.md#null-conditional-operators--and-)
- [?: operator](conditional-operator.md)
