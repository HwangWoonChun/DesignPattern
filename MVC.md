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

MVP
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

## 2. Presenter
```swift
class Presenter {
    var model: Model?
    
    init(model: Model?) {
        if model?.text.count ?? 0 > 5 {
            self.model = model
        }
    }
    
}
```

## 3. View(ViewController)
```swift
class ViewController: UIViewController {
    
    @IBOutlet var label: UILabel!

    override func viewDidLoad() {
        super.viewDidLoad()
        
        let presenter = Presenter(model: Model(text: "Hel"))
        label.text = presenter.model?.text
        // Do any additional setup after loading the view.
    }

}
```
