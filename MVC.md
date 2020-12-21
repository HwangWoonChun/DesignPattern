MVC
===========

## 1. Model
```swift
class Model {
    let text: String
    init(text: String) {
        self.text = text
    }
}
```

## 2. View
```swift
class CustomTextLabel: UILabel {
    override class func awakeFromNib() {
        super.awakeFromNib()
    }
}
```

## 3. ViewController
```swift
class ViewController: UIViewController, Bindable {
    
    var model: Model!
    var customLabel: CustomTextLabel = CustomTextLabel(frame: CGRect(x: 100, y: 100, width: 100, height: 100))
    
    override func viewDidLoad() {
        super.viewDidLoad()
    }
    
    func bindModel() {
        customLabel.text = model.text
        self.view.addSubview(customLabel)
    }
}
```
