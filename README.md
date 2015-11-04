# Swift — Collection Types

## Objectives

## Collections

Apple's summary of the conceptual differences between Swift's three collection types is quite perfect:

>Swift provides three primary *collection* types, known as arrays, sets, and dictionaries, for storing collections of values. Arrays are ordered collections of values. Sets are unordered collections of unique values. Dictionaries are unordered collections of key-value associations.  
>—From [*The Swift Programming Language, Collection Types*](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/CollectionTypes.html#//apple_ref/doc/uid/TP40014097-CH8-ID105), by Apple, Inc.

Arrays are ordered by index; sets hold unique values; dictionaries hold unique keys with values.

**Objective-C:** *Swift's* `Array`, `Set`, *and* `Dictionary` *classes are interoperable with* `NSArray`, `NSSet`, *and* `NSDictionary`.

### Generic Collections

Since Swift is a strongly-typed language, its collection types are also "typed", meaning that they are given knowledge regarding their contents. Apple notes that its Array, Set, and Dictionary types "are implemented as *generic collections*."

References to the keyword `Element` (or the archaic `T` from Swift 1) denote a *generic*. We'll discuss generics in further detail in upcoming lessons. In a nutshell, a generic is a placeholder for **any** type. The effect of this is that the compiler gives individual instances of these collections knowledge of their membership types when they are declared, but otherwise the collection types themselves work exactly the same way whether they are told that they hold `Int`s, or `String`s, or anything else.

### `let` versus `var`

Collections declared using `let` are *immutable* and cannot be altered at all after they are initialized. If you wish to make a collection *mutable*, declare it with `var`. Apple recommends declaring collections with `let` as a default until you encounter the need to alter its contents, claiming that `let` allows the compiler to streamline performance if the collection never needs to be altered.

### Pass-by-value

Swift's collections types are passed by *value* and not by *reference*. When a collection instance is assigned to another collection instance, a copy of the instance is generated and assigned to the new instance. The effect of this is important to understand: **changes made to a collection after it passed are not reflected in the state of the original collection.**

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
    // statments
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

### Type Annotation

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


### Tuples