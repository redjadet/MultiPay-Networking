# MultiPay-Networking

Example GET request will be like this:

 NetworkClient.get("http://localhost:8080/api/") { response in
   switch response.result {
       case .success(let response):
         completion(.success(response))
       case .failure(let error):
         completion(.failure(error))
       }
 }

 Example of POST request will be like this:

 let examplePostModel:Codable = ExampleModel()

 NetworkClient.post("http://localhost:8080/api/", method: .post, parameters: examplePostModel { response in
       switch response.result {
       case .success:
         completion(.success(()))
       case .failure(let error):
         completion(.failure(error))
       }
 }
