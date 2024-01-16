## Parsing Dictionary

* **JSON**
```swift
let json = """
{
 "results": [
   {
     "firstName": "John",
     "lastName": "Appleseed"
   },
  {
    "firstName": "Alex",
    "lastName": "Paul"
  }
 ]
}
""".data(using: .utf8)!
```

* **Create Model(s)**

```swift
    // Codable: Decodable & Encodable
    // Decodable: converts json data
    // Encodable: converts to json data to json data to e.g POST to a Web API

// Top level JSON is a Dictionary
struct ResultsWrapper: Decodable {
    let results: [Contact]
}

struct Contact: Decodable {
    let firstName: String
    let lastName: String
}
```

* **decode the JSON data to our Swift model**
```swift


do {
    let dictionary = try JSONDecoder().decode(ResultsWrapper.self, from: json)
    let contacts = dictionary.results // [Contact]
    dump(contacts)
} catch {
    print("decoding error: \(error)")
}

/*
 ▿ 2 elements
   ▿ __lldb_expr_95.Contact
     - firstName: "John"
     - lastName: "Appleseed"
   ▿ __lldb_expr_95.Contact
     - firstName: "Alex"
     - lastName: "Paul"
 */
```