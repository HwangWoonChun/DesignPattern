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
class ViewController: UIViewController {
    
    var model: Model!
    var customLabel: CustomTextLabel?
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        self.model = Model(text: "hello world")
        customLabel = CustomTextLabel(frame: CGRect(x: 100, y: 100, width: 100, height: 100))
        customLabel?.text = model.text
        self.view.addSubview(self.customLabel!)
    }
}
```
