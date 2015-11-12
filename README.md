# Swift — Collection Types

## Objectives

1. Understand Swift collection types as generic collections.
2. Recognize some implications of the pass-by-value nature of Swift collections.
3. Create an explicitly-typed array.
4. Use subscripting to interact with an array.
5. Use a few array methods and properties to interact with an array.
6. Create an explicitly-typed dictionary.
7. Use subscripting to read a dictionary.
8. Use subscripting to overwrite or add values for a key in a dictionary.
9. Use the key-value Tuple returned by iterating over a dictionary with a `for-in` loop.

## Collections

Apple's summary of the conceptual differences between Swift's three collection types is quite perfect:

>Swift provides three primary *collection* types, known as arrays, sets, and dictionaries, for storing collections of values. Arrays are ordered collections of values. Sets are unordered collections of unique values. Dictionaries are unordered collections of key-value associations.  
>—From [*The Swift Programming Language, Collection Types*](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/CollectionTypes.html#//apple_ref/doc/uid/TP40014097-CH8-ID105), by Apple, Inc.

Arrays are ordered by index; sets hold unique values; dictionaries hold unique keys with values.

**Objective-C:** *Swift's* `Array`, `Set`, *and* `Dictionary` *classes are interoperable with* `NSArray`, `NSSet`, *and* `NSDictionary`.

### Generic Collections

Since Swift is a strongly-typed language, its collection types are also "typed", meaning that they are given knowledge regarding their contents. Apple notes in the [*Collection Types* chapter](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/CollectionTypes.html#//apple_ref/doc/uid/TP40014097-CH8-ID105) that its `Array`, `Set`, and `Dictionary` types "are implemented as *generic collections*."

Uses of the keywords `T`, `Element`, and `Item` denote a *generic*. There is no meaningful distinction between the terms and you will see all three of them in various documents, both from Apple and from answers and blogs written by outside developers.

We'll discuss generics in further detail in upcoming lessons. The effect of this is that the compiler gives individual instances of these collections knowledge of their membership types when they are declared, but otherwise the collection types themselves work exactly the same way whether they are told that they hold `Int`s, or `String`s, or anything else.

### `let` versus `var`

Collections declared using `let` are *immutable* and cannot be altered at all after they are initialized. If you wish to make a collection *mutable*, declare it with `var`. Apple recommends declaring collections with `let` as a default until you encounter the need to alter its contents, claiming that `let` allows the compiler to streamline performance if the collection never needs to be altered.

### Pass-by-value

Swift's collections types are passed by *value* and not by *reference*. When a collection instance is assigned to another collection instance, a copy of the instance is generated and assigned to the new instance. The effect of this is important to understand: **changes made to a collection after it is passed are not reflected in the state of the original collection.**

This is true both for local instances and instances passed into functions and methods via arguments. *If you wish to preserve the mutated state of an instance that is passed-by-value, you have to assign the mutated version back to the original instance.*

## `Array`

### Type Annotation

To explicitly declare the type of an empty array, wrap the type within square brackets `[` `]`. You can use either type annotation to set it equal to empty square brackets, or the initializer syntax that uses a parenthesis following the array type:

```swift
let array: [Element] = []      // type annotation
let array: array<Element> = [] // long form annotation
let array = [Element]()        // initializer
```

To explicitly declare an array with contents, the contents must match the annotated type.

```swift
var instructors: [String] = ["Joe", "Tim", "Jim", "Tom"]
let primes: [Int] = [1, 2, 3, 5, 7, 11]
let sqrts: [Double] = [1, 1.414, 1.732, 2.236, 2.646, 3.317]
```

### Subscripting

Arrays can be subscripted by passing an index integer into a pair of square brackets suffixed to the array's name:

```swift
// getting

let joe = instructors[0]
print(joe)
print(instructors[1])
```
This will print:

```
Joe
Tim
```

```swift
// setting, only for arrays declared with var

instructors[2] = "Jim Campagno"
primes[5] = 13   // error
```
### Methods

#### `.append()`

You can add to mutable array using the `.append()` method with the element to add placed inside the parenthesis. The element must match the array's type.

```swift
instructors.append("Mark")
instructors.append(42)   // error
primes.append(13)  // error
```

#### `.removeLast()`

The `.removeLast()` method will return the element at the highest index of array, however it will cause a crash if the array is empty. You must protect against calling this method on array when it is empty when you implement this method, otherwise use `.popLast()` which returns an optional.

#### `.popLast()`

The `.popLast()` method returns an optional of the array's content type, which will need to be unwrapped. This method will not cause a run time crash if it is called on an empty array since it will return an optional containing `nil` in such a case.

### `.reverse()`

The `.reverse()` method returns a copy of the array with the elements in reverse order. It does **not** implicitly mutate the array it is called on.

### Array Properties

#### `.first` & `.last`

The `.first` and `.last` are properties which return on optional containing the element at the array's lowest index (`.first`) or at its highest index (`.last`). If the array is empty, accessing these properties will return an optional containing `nil`.

#### `.count`

Arrays have a `count` property which returns an `Int` count of the number of elements currently in its membership.

### Arrays with Loops

To iterate over an array without needing the index number, use a `for` loop:

```swift
for Element in Array {
    // statements
}
```

An implementation of this might look like:

```swift
let instructors: [String] = ["Joe", "Tim", "Jim", "Tom", "Mark"]

for instructor in instructors {
    print("Good morning, \(instructor)!")
}
```
This will print:

```
Good morning, Joe!
Good morning, Tim!
Good morning, Jim!
Good morning, Tom!
Good morning, Mark!
```

The `instructor` instance declared in the `for` loop will be implicitly typed as `String`, since the `instructors` array is typed as a `String` array.

## `Set`

Sets have use cases that we're not going to discuss at this time. For now, just recognize that they are also a collection type.

## `Dictionary`

### Creating a Dictionary

To explicitly declare the types of an empty dictionary, wrap the types of the key and value in square brackets `[` `]` but separated by a colon (`:`). You can use either type annotation to set it equal to an empty bracket-and-colon set, or the initializer syntax that uses a parenthesis following the dictionary type:

```swift
let dictionary: [Key: Value] = [:]           // type annotation
let dictionary: Dictionary<Key, Value> = [:] // long form annotation
let dictionary = [Key: Value]()              // initializer
```

To explicitly declare a dictionary with contents, the contents must match the annotated key-value types.

```swift
let karmaByName: [String: Int] = ["Joe":53, "Tim":13, "Jim":9, "Tom":9, "Mark":8]
let primeSqrts: [Int: Double] = [1:1, 2:1.414, 3:1.732, 5:2.236, 7:2.646, 11:3.317]
```

To improve readability when declaring a large dictionary, you can write each key-value pair on its own line, indented like this:

```swift
let jenny: [String: String] = [
    "first name": "Jenny",
    "relationship": "Friend",
    "phone number": "(555) 867-5309",
    "email address": "jenny@email.com",
    "physical address": "123 Street Name",
    "city state": "Anywhere, USA",
    "zip code": "00409"
]
```

### Reading a Dictionary

Similar to arrays, dictionaries can be subscripted with a key. This syntax returns an *optional* that will contain `nil` if the dictionary holds no value for the requested key:

```swift
if let jennysPhoneNumber = jenny["phone number"] {
    print(jennysPhoneNumber)
}
```
This will print: `(555) 867-5309`.

```swift
if let jennysFavoriteShrimpDish = jenny["favorite shrimp dish"] {
    print(jennysFavoriteShrimpDish)
} else {
    print("Jenny doesn't like shrimp.")
}
```
This will print: `Jenny doesn't like shrimp.` because requesting a value for a non-existent key returns `nil`.

### Writing to a Dictionary

To alter a dictionary by adding a new key-value pair, or overwriting the value of an existing key, the dictionary must be *mutable*, meaning that it must have been created using `var`:

```swift
var jenny: [String: String] = [
    "first name": "Jenny",
    "relationship": "Friend",
    "phone number": "(555) 867-5309",
    "email address": "jenny@email.com",
    "physical address": "123 Street Name",
    "city state": "Anywhere, USA",
    "zip code": "00409"
]
```
We can then subscript the dictionary on the *left* side of an assignment operator in order to write to it:

```swift
jenny["relationship"] = "It's Complicated"     // overwrites an existing value

jenny["favorite shrimp dish"] = "Shrimp Gumbo" // creates a new key-value pair
```
Since keys must remain unique, writing to an existing key does not duplicate it, but rather replaces (or "overwrites") the previous value with a new value.

### Removing from a Dictionary

Swift dictionaries have a nice feature that if you set an existing key's value to `nil`, it will remove the key from the dictionary since it no longer has an associated value:

```swift
jenny["first name"] = nil
jenny["relationship"] = nil
jenny["phone number"] = nil
jenny["email address"] = nil
jenny["physical address"] = nil
jenny["city state"] = nil
jenny["zip code"] = nil
jenny["favorite shrimp dish"] = nil

print(jenny)
```
This will print: `[:]` meaning an empty dictionary.

### Looping Over Dictionaries

To iterate over every key-value pair in a dictionary, a `for-in` loop can be used with the following syntax:

```swift
for (key, value) in dictionary {
    // statements
}
```
The component that reads `(key, value)` is called a Tuple. In this case, the Tuple contains elements for the `key` and `value` of an associated pair in the dictionary. (**Advanced:** *This Tuple is actually a struct named* `DictionaryIndex`.)
However you name these elements in the `for-in` loop's declaration line is how the instances will be accessible within the loop's body.

So while `key` and `value` might be appropriate for the `jenny` dictionary:

```swift
for (key, value) in jenny {
    print("Jenny's \(key) is \(value).")
}
```
Which will print (assuming we haven't emptied the dictionary), in some order:

```
Jenny's favorite shrimp dish is Shrimp Gumbo.
Jenny's phone number is (555) 867-5309.
Jenny's first name is Jenny.
Jenny's physical address is 123 Street Name.
Jenny's city state is Anywhere, USA.
Jenny's zip code is 00409.
Jenny's email address is jenny@email.com.
Jenny's relationship is It's Complicated.
```
However, we can also choose to use more specific names for the elements to improve clarity:

```swift
let karmaByName: [String: Int] = ["Joe":53, "Tim":13, "Jim":9, "Tom":9, "Mark":8]

for (name, karma) in karmaByName {
    print("\(name) has \(karma) karma.")
}
```
Which will print, in some order:

```
Jim has 9 karma.
Tim has 13 karma.
Joe has 53 karma.
Tom has 9 karma.
Mark has 8 karma.
```
And also:

```swift
let primeSqrts: [Int: Double] = [1:1, 2:1.414, 3:1.732, 5:2.236, 7:2.646, 11:3.317]

for (prime, sqrt) in primeSqrts {
    print("The square root of the prime number \(prime) is approximately \(sqrt)")
}
```
Which will print, in some order:

```
The square root of the prime number 7 is approximately 2.646
The square root of the prime number 2 is approximately 1.414
The square root of the prime number 3 is approximately 1.732
The square root of the prime number 1 is approximately 1.0
The square root of the prime number 11 is approximately 3.317
```

### Useful Properties

Swift dictionaries have a couple intuitive properties:

#### `.count`

Holds an `Int` value of the number of key-value pairs currently held in the dictionary.

#### `.isEmpty`

Holds a `Bool` of whether or not the dictionary is empty; it returns `true` if the `count` is `0`("zero").
