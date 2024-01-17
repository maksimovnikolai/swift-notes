## POST Request URLSession Code Examples

*  **Cсылка для тестирования POST запросов**

```swift
let postRequestURL = "https://jsonplaceholder.typicode.com/posts"
```

* **Custom error's**

```swift
enum NetworkError: Error { 
    case invalidURL
    case noData
    case decodingError
}
```

* **метод для POST запроса который будет отправлять на сервер JSON созданный на основе словаря**
* **JSON - представляет из себя словарь - [String: Any], и этот тип никогда не меняется**

```swift
func postRequest(with data: [String: Any], to url: String, completion: @escaping(Result<Any, NetworkError>) -> Void) {
    // 1. Создание URL адреса по которому будет создаваться запрос
    guard let url = URL(string: url) else {
        completion(.failure(.invalidURL))
        return
    }
    
    // 2. Создание Тела для запроса
    // JSONSerialization - позволяет создать JSON на основе словаря, преобразую в тип "Data"
    let courseData = try? JSONSerialization.data(withJSONObject: data)
    
    var request = URLRequest(url: url) // запрос по определенному URL адресу
    request.httpMethod = "POST" // метод по которому будет выполнен запрос
    request.addValue("application/json", forHTTPHeaderField: "Content-Type") // формирует отправленные данные в JSON
    request.httpBody = courseData // установка в тело запроса объекта с типом "Data"
    
    // 3.
    let dataTast = URLSession.shared.dataTask(with: request) { data, response, error in
        guard let data = data, let response = response else {
            completion(.failure(.noData))
            print(error?.localizedDescription ?? "no error description")
            return
        }
        print(response)
        do {
            // JSONSerialization.jsonObject - декодирует объект типа "Data" в JSON-файл
            let jsonData = try JSONSerialization.jsonObject(with: data)
            completion(.success(jsonData))
        } catch {
            completion(.failure(.decodingError))
        }
    }
    dataTast.resume()
}

// ОПИСАНИЕ:
/*
 1. Создание URL адреса по которому будет создаваться запрос
 2. Создание Тела для запроса
 3. Если запрос пройдет успешно, сервер получит отправленные ему данные,
    распакует их, сохранит у себя, потом снова запакует в тип "Data" и
    отправит обратно (нам). А также "response".
    Также "error" в случае какой либо ошибки.
 */
```

* **ТЕСТ**
```swift
let person = [
    "name": "Sam",
    "lastName": "Smith",
    "age": "33"
]

postRequest(with: person, to: postRequestURL) { result in
    switch result {
    case .failure(let error):
        print(error)
    case .success(let json):
        print(json)
    }
}

// Ответ от сервера
/*
 <NSHTTPURLResponse: 0x6000027be0e0> { URL: https://jsonplaceholder.typicode.com/posts } { Status Code: 201, Headers {
     "Alt-Svc" =     (
         "h3=\":443\"; ma=86400"
     );
     "Cache-Control" =     (
         "no-cache"
     );
     "Content-Length" =     (
         70
     );
     "Content-Type" =     (
         "application/json; charset=utf-8"
     );
     Date =     (
         "Mon, 25 Sep 2023 20:37:19 GMT"
     );
     Etag =     (
         "W/\"46-w5v6Ij3U7u1JECygI0V455bfcFs\""
     );
     Expires =     (
         "-1"
     );
     Location =     (
         "http://jsonplaceholder.typicode.com/posts/101"
     );
     Pragma =     (
         "no-cache"
     );
     Server =     (
         cloudflare
     );
     Vary =     (
         "Origin, X-HTTP-Method-Override, Accept-Encoding"
     );
     Via =     (
         "1.1 vegur"
     );
     "access-control-allow-credentials" =     (
         true
     );
     "access-control-expose-headers" =     (
         Location
     );
     "cf-cache-status" =     (
         DYNAMIC
     );
     "cf-ray" =     (
         "80c6107b2b940056-DME"
     );
     nel =     (
         "{\"success_fraction\":0,\"report_to\":\"cf-nel\",\"max_age\":604800}"
     );
     "report-to" =     (
         "{\"endpoints\":[{\"url\":\"https:\\/\\/a.nel.cloudflare.com\\/report\\/v3?s=cXcXLYyVJfIciPb%2Bi443CpJyRACGwgJADk1q7bwo4d0Ecg0NYtwv7DsCFAxap3QzKYfdTeZo9RpzDZQi62VZUFO9yc8%2F0uTDLfaKZKnrgPdUMPtDs1M9CePAkv524fcrm3riRYA%2BGQtoc0yffa79\"}],\"group\":\"cf-nel\",\"max_age\":604800}"
     );
     "x-content-type-options" =     (
         nosniff
     );
     "x-powered-by" =     (
         Express
     );
     "x-ratelimit-limit" =     (
         1000
     );
     "x-ratelimit-remaining" =     (
         999
     );
     "x-ratelimit-reset" =     (
         1695674291
     );
 } }
 {
     age = 33;
     id = 101;
     lastName = Smith;
     name = Sam;
 }
 */
```