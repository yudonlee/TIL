# WeatherToday 앱개발

- [부스트코스 3번째 프로젝트](https://www.boostcourse.org/mo326 "WeatherTodayApplication")
  부스트코스 Project C WeatherToday앱개발 로드맵이다.
- 코드 원문을 올리는 것은 허용되지 않기 때문에, 구현에 필요한 아이디어를 기록하였다.
- 뷰컨트롤러간에 중복되는 구현 아이디어가 존재한다면, 먼저 나오는 뷰 컨트롤러에 적고 다음 뷰 컨트롤러에는 적지 않았다.

# 구현예시

<img src="https://github.com/yudonlee/TIL/blob/main/iOS/pictures/ViewController.png" width="300" height="300">

<img src="https://github.com/yudonlee/TIL/blob/main/iOS/pictures/CityListViewController.png" width="300" height="300">
<img src="https://github.com/yudonlee/TIL/blob/main/iOS/pictures/DetailedWeatherViewController.png" width="300" height="300">

# 전체적인 구현 모식도

[구현과정에 대한 전체적인 구조](https://github.com/yudonlee/TIL/blob/main/iOS/pictures/architecture.pdf "WeatherTodayApplicationArchitecture")

# 목차

1. ViewController
2. CityListViewController
3. DetailedWeatherViewController
4. 나만의 아이디어로 해결한 점
5. 구현중 발생했더 에러

# 1. ViewController

- UITableViewCell의 basic style에 값 넣기  
   본문에서는 한글로 된 나라이름을 요구하였기 때문에 JSON 파일에서 불러온 값중 한글이름을 textLabel에 넣어주었다.  
   이때, 좌측에 이미지가 필요하기 때문에 UITableViewCell.imageView를 이용해서 asset에 import된 이미지를 불러온다.

  ```swift
  //this is not formal form. just example
  UITableViewCell.imageView?.image = UIImage("ImageFileNameInAsset")
  ```

- ViewDidLoad에서 JSON 불러오기  
  JSON의 키들과, 받아오는 structure의 키의 값이 같아야 하는데, 그렇게 되면 swift naming convention인 camelCase를 지킬 수 없다.  
  그리하여 CodingKeys라는 protocol을 이용하여 해결한다.
- Navigator bar에 title 붙이기
  ```swift
  self.navigationItem.title = "someThing"
  ```
- 다음 뷰 컨트롤러에 Navigation bar 값 설정  
   prepare함수를 활용하여, 다음 뷰가 로드되기 전 필요한 값을 넘겨준다.  
   다음 뷰컨트롤러의 타이틀과, backbuttonTite은 이전 뷰에서 클릭된 셀의 값들이기 때문에 현재 터치된 셀의 정보를 갖고 있는 sender를 활용한다.
  ```swift
  // typecasting sender to UITableViewCell
  nextViewController.navigationItem.title = typecastedSender.textLabel?.text
  nextViewController.navigationController?.navigationBar.topItem?.backbuttonTitle = currentViewControllerTitle
  ```
- 다음 뷰 컨트롤러에 프로퍼티로 값 전달하기(cell 내부에 저장된 레이블을 통해서 Mapping으로 프로퍼티에 값을 전달하는 방식)
  딕셔너리를 사용하여, textLabel에서의 string을 가지고 필요한 string값을 불러와서 다음 뷰 컨트롤러에 프로퍼티에 저장했다.
  ```swift
  nextViewController.anyProperty = koreanToEnglish[UITableViewCell.textLabel?.text]
  ```

## 나라 값을 을 저장하는 structure

- CodingKey protocol을 활용하여 JSON 키값과 다르더라도 매칭  
   enum을 활용하여 swift namingConvention에 맞도록 한다.

## Country Json file 예시

```json
{ "korean_name": "한국", "asset_name": "kr" }
```

## 구현 중 발생했던 문제점

### 1. 다음 뷰 컨트롤러에 프로퍼티로 값 전달하기(cell 내부에 저장된 레이블을 통해서 Mapping으로 프로퍼티에 값을 전달하는 방식)

---

next 뷰 컨트롤러에 값을 전달할 떄 필요한 정보가 cell내부 label에 존재하지 않는다면, 값을 전달하기가 까다롭다.  
 위 구현사항에서 textLabel은 한국어였고, 다음 뷰 컨트롤러에 필요한 정보는 영어로 된 나라이름이였다.  
 처음에는 cell내부에서 사용하지 않는 UITableViewCell.detailedLabel에 저장하여 해결하고자 했다.  
 하지만 사용하지 않는 레이블은 값이 저장되지 않은채 nil값으로 전환되었고 그에따라서 에러가 발생하기도 하였다.  
 그래서 해결책은, dictionary를 사용하여 Dictionary[한글:영어]를 통해서 매핑하여 프로퍼티에 값을 전달했다.

# 2. CityListViewController

- 프로젝트 요구사항에서 세가지 텍스트 레이블과, imageView를 요구하기 때문에 TableView의 스타일을 style custom버전으로 해야한다.
- 커스텀 버전으로 구현되는 만큼, Label도 그에맞게 설정해주어야 하며, 이때 다음 뷰 컨트롤러에 필요한 프로퍼티를 별도로 저장할 공간이 필요한데, 이때 CustomViewCell.swift 파일에 저장하는 것이 가능하다
  ```swift
  //CustomTableViewCell.swift
  @IBOUTlet weak var firstTextLabel: UILabel!
  // other params..
  var anyProperty: Strig?
  ```
  ```swift
  // anyViewController.swift
  func TableView(...) {
      CustomTableViewCell.anyProperty = neededInformation
  }
  ```
- prepare() 함수를 통해 다음 뷰 컨트롤러에 필요한 값을 전달하기  
  다음 뷰 컨트롤러인 "DetailedWeatherViewController"는 현재 뷰 컨트롤러의 온도 정보를 담고있는 "temperatureLabel", 현재 날씨의 상태를 나타내는 사진을 담고있는 "imageView", 그리고 커스텀 셀 프로퍼티에 저장된 날씨(한국어)를 필요로 한다.  
  그렇기 때문에 prepare함수를 통해서 해당 값을 넣어준다.

- Double로 저장된 값을, string으로 전달할 때 소수 첫번째 자리까지 만 전달하기  
  String formatter를 활용하여 소수점을 원하는대로 truncate 할수 있다.
  ```swift
  // 결과 예시: "섭씨 12.6도 / 화씨 54.7도
  var temperature: String { let celsius: Int = 12.6 let fahrenheit: Int = 54.7 return "섭씨 \(String(format: "%.1f", celsius)) 도 / 화씨 \(String(format: "%.1f", fahrenheit))도" }
  ```

## City Json file 예시

```json
{
  "city_name": "오사카",
  "state": 10,
  "celsius": 25.7,
  "rainfall_probability": 80
}
```

# 3. DetailedWeatherViewController

- 이전 뷰 컨트롤러에서 넘어온 정보들을, label에 저장해준다.
  ```swift
  someTextLabel?.text = dataFromPreViewController
  someImageView.image = imageFromPreViewController
  ```

# 4. 구현중 생긴 문제

## 1. this class is not key value coding-compliant for the key temperature."

- 해당 익셉션으로 인하여, breakpoint도 주었지만 정상적으로 데이터들이 입력되는 상황에서 dequeueReusableCell method에서 원인을 찾을 수 없었다.
- 구글링 과정을 거치면서, 해당 문제는 흔히 outlet을 연결하지 않거나 잘못한 상황이라는 점을 알게 되었고 재지정하는 것을 통해서 해결했다.

### 출처

https://becodable.com/this-class-is-not-key-value-coding-compliant-for-the-key/
