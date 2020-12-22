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

## 3. ViewController + View
```swift
class ViewController: UIViewController {
    
    var model: Model!
    var label: UILabel?
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        self.model = Model(text: "hello world")
        label = CustomTextLabel(frame: CGRect(x: 100, y: 100, width: 100, height: 100))
        label?.text = model.text
        self.view.addSubview(self.customLabel!)
    }
}
```
