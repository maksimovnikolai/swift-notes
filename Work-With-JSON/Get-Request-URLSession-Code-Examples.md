## Get Request URLSession Code Examples

* **Model**

```swift
struct Person: Decodable { // Model
    let name: String
}

enum NetworkError: Error { // Custom error's
    case invalidURL
    case noData
    case decodingError
}
```

* **Function of requesting data from the network using "Result"**

```swift
func fetchData(from url: String, completion: @escaping(Result<Person, NetworkError>) -> Void) {
    guard let url = URL(string: url) else {
        completion(.failure(.invalidURL))
        return
    }
    
    let dataTask = URLSession.shared.dataTask(with: url) { data, _, error in
        guard let data = data else {
            completion(.failure(.noData))
            print(error?.localizedDescription ?? "no error description")
            return
        }
        
        do {
            let data = try JSONDecoder().decode(Person.self, from: data)
            DispatchQueue.main.async {
                completion(.success(data))
            }
        } catch {
            completion(.failure(.decodingError))
        }
    }
    dataTask.resume()
}
```
* **Function of requesting Image with "Data(contestOf: )**

```swift
func fetchData(from url: String?, completion: @escaping(Result<Data, NetworkError>) -> Void) {
    guard let url = URL(string: url ?? "") else {
        completion(.failure(.invalidURL))
        return
    }
    DispatchQueue.global().async {
        guard let imageData = try? Data(contentsOf: url) else {
            completion(.failure(.noData))
            return
        }
        DispatchQueue.main.async {
            completion(.success(imageData))
        }
    }
}

func fetchImage(from url: String?, with completion: @escaping(Data) -> Void) {
    guard let stringURL = url else { return }
    guard let imageURL = URL(string: stringURL) else { return }
    DispatchQueue.global().async { // - exit from the main thread
        //Data(contentsOf: imageURL) - loading any object with date type from the network
        guard let data = try? Data(contentsOf: imageURL) else { return }
        DispatchQueue.main.async {
            completion(data)
        }
    }
}
```

* **Function of requesting any data from the network using Generic**

```swift
func fetchData<T: Decodable>(dataType: T.Type, from url: String, completion: @escaping(Result<T, NetworkError>) -> Void) {
    guard let url = URL(string: url) else {
        completion(.failure(.invalidURL))
        return
    }
    
    let dataTask = URLSession.shared.dataTask(with: url) { data, _, error in
        guard let data = data else {
            completion(.failure(.noData))
            print(error?.localizedDescription ?? "no error description")
            return
        }
        
        do {
            let type = try JSONDecoder().decode(T.self, from: data)
            DispatchQueue.main.async {
                completion(.success(type))
            }
        } catch {
            completion(.failure(.decodingError))
        }
    }
    dataTask.resume()
}
```



```swift

```



```swift

```



```swift

```