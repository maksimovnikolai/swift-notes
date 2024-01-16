## Parsing Multiple Keys

* **JSON data**

```swift
let json = """
{
  "Afpak": {
    "id": 1,
    "race": "hybrid",
    "flavors": [
      "Earthy",
      "Chemical",
      "Pine"
    ],
    "effects": {
      "positive": [
        "Relaxed",
        "Hungry",
        "Happy",
        "Sleepy"
      ],
      "negative": [
        "Dizzy"
      ],
      "medical": [
        "Depression",
        "Insomnia",
        "Pain",
        "Stress",
        "Lack of Appetite"
      ]
    }
  },
  "African": {
    "id": 2,
    "race": "sativa",
    "flavors": [
      "Spicy/Herbal",
      "Pungent",
      "Earthy"
    ],
    "effects": {
      "positive": [
        "Euphoric",
        "Happy",
        "Creative",
        "Energetic",
        "Talkative"
      ],
      "negative": [
        "Dry Mouth"
      ],
      "medical": [
        "Depression",
        "Pain",
        "Stress",
        "Lack of Appetite",
        "Nausea",
        "Headache"
      ]
    }
  },
  "Afternoon Delight": {
    "id": 3,
    "race": "hybrid",
    "flavors": [
      "Pepper",
      "Flowery",
      "Pine"
    ],
    "effects": {
      "positive": [
        "Relaxed",
        "Hungry",
        "Euphoric",
        "Uplifted",
        "Tingly"
      ],
      "negative": [
        "Dizzy",
        "Dry Mouth",
        "Paranoid"
      ],
      "medical": [
        "Depression",
        "Insomnia",
        "Pain",
        "Stress",
        "Cramps",
        "Headache"
      ]
    }
  }
}
""".data(using: .utf8)!
```

* **Create Model(s)**

```swift
struct Strain: Decodable {
    let id: Int
    let race: String
    let flavors: [String]
    let effects: [String: [String]]
}
```

* **decode JSON to Swift objects**

```swift
do {
    let dictionary = try JSONDecoder().decode([String: Strain].self, from: json)
    
    // use a for-loop to create [Strain] or use map {}
    
//    var strains = [Strain]()
//    for (_, value) in dictionary {
//        let strain = Strain(id: value.id,
//                            race: value.race,
//                            flavors: value.flavors,
//                            effects: value.effects)
//        strains.append(strain)
//    }
    
    let strains = dictionary.map { Strain(id: $0.value.id,
                                          race: $0.value.race,
                                          flavors: $0.value.flavors,
                                          effects: $0.value.effects)}
    dump(strains)
} catch {
    print("decoding error: \(error)")
}

/*
 ▿ 3 elements
   ▿ __lldb_expr_125.Strain
     - id: 1
     - race: "hybrid"
     ▿ flavors: 3 elements
       - "Earthy"
       - "Chemical"
       - "Pine"
     ▿ effects: 3 key/value pairs
       ▿ (2 elements)
         - key: "medical"
         ▿ value: 5 elements
           - "Depression"
           - "Insomnia"
           - "Pain"
           - "Stress"
           - "Lack of Appetite"
       ▿ (2 elements)
         - key: "negative"
         ▿ value: 1 element
           - "Dizzy"
       ▿ (2 elements)
         - key: "positive"
         ▿ value: 4 elements
           - "Relaxed"
           - "Hungry"
           - "Happy"
           - "Sleepy"
   ▿ __lldb_expr_125.Strain
     - id: 3
     - race: "hybrid"
     ▿ flavors: 3 elements
       - "Pepper"
       - "Flowery"
       - "Pine"
     ▿ effects: 3 key/value pairs
       ▿ (2 elements)
         - key: "negative"
         ▿ value: 3 elements
           - "Dizzy"
           - "Dry Mouth"
           - "Paranoid"
       ▿ (2 elements)
         - key: "medical"
         ▿ value: 6 elements
           - "Depression"
           - "Insomnia"
           - "Pain"
           - "Stress"
           - "Cramps"
           - "Headache"
       ▿ (2 elements)
         - key: "positive"
         ▿ value: 5 elements
           - "Relaxed"
           - "Hungry"
           - "Euphoric"
           - "Uplifted"
           - "Tingly"
   ▿ __lldb_expr_125.Strain
     - id: 2
     - race: "sativa"
     ▿ flavors: 3 elements
       - "Spicy/Herbal"
       - "Pungent"
       - "Earthy"
     ▿ effects: 3 key/value pairs
       ▿ (2 elements)
         - key: "negative"
         ▿ value: 1 element
           - "Dry Mouth"
       ▿ (2 elements)
         - key: "medical"
         ▿ value: 6 elements
           - "Depression"
           - "Pain"
           - "Stress"
           - "Lack of Appetite"
           - "Nausea"
           - "Headache"
       ▿ (2 elements)
         - key: "positive"
         ▿ value: 5 elements
           - "Euphoric"
           - "Happy"
           - "Creative"
           - "Energetic"
           - "Talkative"
 */
```