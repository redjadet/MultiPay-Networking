# MultiPay-Networking

- Example GET request will be like this:

 NetworkClient.get("http://localhost:8080/api/") { response in
 
   switch response.result {
       case .success(let response):
         completion(.success(response))
       case .failure(let error):
         completion(.failure(error))
       }
 }

 - Example of POST request will be like this:

 let examplePostModel:Codable = ExampleModel()

 NetworkClient.post("http://localhost:8080/api/", method: .post, parameters: examplePostModel { response in
       switch response.result {
       case .success:
         completion(.success(()))
       case .failure(let error):
         completion(.failure(error))
       }
 }
 
 - Example of download request will be like this:
 
 func downloadSampleImage(progressCompletion: @escaping (_ percent: Float) -> Void, completion: @escaping (_ tags: [String]?) -> Void) {
    let imageURL = "https://upload.wikimedia.org/wikipedia/commons/thumb/b/b3/Wikipedia-logo-v2-en.svg/1200px-Wikipedia-logo-v2-en.svg.png"
    NetworkClient.download(imageURL).downloadProgress { progress in
      progressCompletion(Float(progress.fractionCompleted))
    }.responseData { response in
      switch response.result {
      case .failure(let error):
        print("Error while fetching the image: \(error)")
        completion(nil, nil)
      case .success(let photoData):
        guard let image = UIImage(data: photoData) else {
          print("Error while converting the image data to a UIImage")
          completion(nil, nil)
          return
        }
        self.upload(image: image, progressCompletion: progressCompletion, completion: completion)
								
      }
    }
  }
 
 
