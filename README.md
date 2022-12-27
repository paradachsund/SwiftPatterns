# SwiftPatterns
https://docs.swift.org/swift-book/ReferenceManual/Patterns.html

import UIKit

//https://docs.swift.org/swift-book/ReferenceManual/Patterns.html

//Swift has 2 kinds of patterns -
//1. matches any kind of value at runtime - used for destructing values in simple variables, constants, or optional bindings - include wildcard, identifier, and any value binding or tuple patterns
//2. fails to match a specified value at runtime - enumeration case patterns, optional patterns, expression patterns, type-casting patterns


// Type 1 Patterns
//1a. Wildcard pattern -- wildcard-pattern -> _
for _ in 1...3 { //disregard the current value of the range iteration
    //something in triplicate
}

//1b. Identifier pattern -- identifier-pattern -> identifier
//matches a value and binds it to a variable or constant
let someImmutableValue = 24

//1c. Value-Binding pattern -- valueBinding pattern -> var pattern | let pattern
//binds matched values to variable or constant names
let point = (2,1)
switch point {
case let (x,y): //decompose tuple elements and bind to each element in corresponding identifier pattern
    print("xy = (\(x)\(y))")
}

//1d. Tuple pattern -- Tuple pattern -> (x,y)
//comma-separated list of zero or more patterns enclosed by parenthesis corresponding to type annotations. When used as pattern in a for-in statement or var constant declaration, can only contain wildcard, identifier, optional, or other tuple patterns and no type II patterns

let points = [(3,2), (2,1)]
//for (x, 0) in points { //invalid since 0 in tuple pattern  (x, 0) is an expression pattern
    //
//}

// Type 2 Patterns
//2a. Enumeration Case pattern -> typeIdentifier (optional) * enum-case-name tuple-pattern
//matches case of extant enum type, appear in switch statement case labels and in case conditions of if, while, guard, and for-in statements
//also matches values of that case wrapped in optional - to omit optional pattern.

enum Turns { case left, right }
let x: Turns? = .left
switch x {
case .left:
    print("left")
case .right:
    print("right")
case nil:
    print("straight")
}

//2b. Optional pattern -> identifier-pattern ?
//matches values wrapped in a some(Wrapped) case of an Optional<Wrapped> enumeration, optional patterns consist of an identifier pattern followed by a question mark and appear in the same places as enum case patterns

//because optional patterns are syntactic sugar for Optional enum case patterns, the following are equivalent

let optionalValue: Int? = 24
if case .some(let x) = optionalValue { //enumeration case pattern
    print("x = \(x)")
}

if case let x? = optionalValue {
    print("x = \(x)") //optional pattern
}

//convenient way to fast iteration over array of optional values
let optionalsArray = [nil, 1, 2, 3, nil, 5]
//match only non-nil values
for case let number? in optionalsArray {
    print("number is \(number)")
}

//2c. Type-Casting patterns
//2 kinds of type-casting, 'is' and 'as'
// is - matches if type is same or subclass of that of value at runtime
var something: Int? = 0
print(something is Int)
// as - matches if type of value at runtime is the saem as the type specified to the right
var things:[Any] = [0, 3.14, "hello"]
    for thing in things {
    switch thing {
        case 0 as Int:
            print("zero int")
        case 0 as Double:
            print("zero double")
        case let someInt as Int:
        print("integer value of \(someInt)")
    default:
        break
    }
}

//2d. Expression pattern -> expression
//represents value of expression, only appears in switch statement case labels. Expression representing by expressio pattern is compared with value of input expression using ~= operator, which compares to values of the same type y default using ==, but can also match ranges

let point0 = (1,2)
switch point0 {
case (0,0):
    print("at origin")
case (-2...2, -2...2):
    print("within +/- 2 of origin")
default:
    print("point at \(point0.0) \(point0.1)")
}

//overload ~= operation to provide custom expression matching behavior
func ~= (pattern: String, value:Int) -> Bool {
    return pattern == "\(value)"
}
let point1 = (0,0)
switch point1 {
case ("0", "0"):
    print("at origin")
default:
    print("point at \(point1.0) \(point1.1)")
}
