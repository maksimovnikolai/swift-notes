## POST Request URLSession Model

* **Cсылка для тестирования POST запросов**

```swift
let postRequestURL = "https://jsonplaceholder.typicode.com/posts"
```

* **POST Request на основе Модели**
```swift
struct Person: Codable { // Модель
    let name: String
    let lastName: String
    let age: Int
}

func postRequest(with data: Person, to url: String, completion: @escaping(Result<Any, NetworkError>) -> Void) {
    
    guard let url = URL(string: url) else {
        completion(.failure(.invalidURL))
        return
    }
    
    // Создание JSON на основе модели используется JSONEncoder().encode
    guard let courseData = try? JSONEncoder().encode(data) else {
        completion(.failure(.encodingError))
        return
    }
    
    var request = URLRequest(url: url) // запрос по определенному URL адресу
    request.httpMethod = "POST" // метод по которому будет выполнен запрос
    request.addValue("application/json", forHTTPHeaderField: "Content-Type")
    request.httpBody = courseData // установка в тело запроса объекта с типом "Data"
    
    let dataTast = URLSession.shared.dataTask(with: request) { data, _, error in
        guard let data = data else {
            completion(.failure(.noData))
            print(error?.localizedDescription ?? "no error description")
            return
        }
        
        do {
            let person = try JSONDecoder().decode(Person.self, from: data)
            completion(.success(person))
        } catch {
            completion(.failure(.decodingError))
        }
    }
    dataTast.resume()
}
```

* **POST Request**

```swift
// Инициализация модели данных для отправки на сервер
let person = Person(name: "Sam", lastName: "Smit", age: 33)


postRequest(with: person, to: postRequestURL) { result in
    switch result {
    case .failure(let error):
        print(error)
    case .success(let person):
        print(person)
    }
}

// ОТВЕТ
/*
 ▿ __lldb_expr_23.Person
 - name: "Sam"
 - lastName: "Smit"
 - age: 33
 */

```