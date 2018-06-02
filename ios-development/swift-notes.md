# Swift

18 years before today:

```swift
let date = Calendar.current.date(byAdding: .year, value: -18, to: Date())
```

## Enumerations

An enumeration defines a common type for a group of related values and enables you to work with those values in a type-safe way within your code.

If you are familiar with C, you will know that C enumerations assign related names to a set of **integer** values. Enumerations in Swift are much more flexible, and do not have to provide a value for each case of the enumeration. If a value \(known as a “**raw**” value\) is provided for each enumeration case, the value can be a string, a character, or a value of any integer or floating-point type.

**Associated value**: “Define an enumeration type called Barcode, which can take either a value of upc with an associated value of type \(Int, Int, Int, Int\), or a value of qrCode with an associated value of type String.”

```swift
enum Barcode {
    case upc(Int, Int, Int, Int)
    case qrCode(String)
}
```

