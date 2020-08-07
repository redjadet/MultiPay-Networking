# Multipay_Networking

- Supports
 GET
 POST
 Download files
 Upload files
 Certificate Pinning
 Retry Request


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
 
 - Example of download request will be like this with func:
 
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
  
  - Example of upload request will be like this with func:
  
  func upload(image: UIImage,
              progressCompletion: @escaping (_ percent: Float) -> Void,
              completion: @escaping (_ tags: [String]?, _ colors: [PhotoColor]?) -> Void) {
    guard let imageData = image.jpegData(compressionQuality: 0.5) else {
      print("Could not get JPEG representation of UIImage")
      return
    }
    NetworkClient.upload(multipartFormData: { multipartFormData in
      multipartFormData.append(imageData, withName: "image", fileName: "image.jpg", mimeType: "image/jpeg")
    }, with: Router.upload)
      .uploadProgress { progress in
        progressCompletion(Float(progress.fractionCompleted))
      }.responseDecodable(of: UploadImageResponse.self) { response in
        switch response.result {
        case .failure(let error):
          print("Error uploading file: \(error)")
          completion(nil, nil)
        case .success(let uploadResponse):
          let resultID = uploadResponse.result.uploadID
          print("Content uploaded with ID: \(resultID)")
        }
    }
  }
 
 

