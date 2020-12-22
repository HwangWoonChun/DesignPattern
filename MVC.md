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


Delegate
===========
## 1. ViewController
```swift
class ViewController: UIViewController {
    
    @IBOutlet var label: UILabel!
    @IBOutlet var tableView: UITableView!

    override func viewDidLoad() {
        super.viewDidLoad()
        let nib = UINib(nibName: "CustomCell", bundle: nil)
        tableView.register(nib, forCellReuseIdentifier: "CustomCell")
        // Do any additional setup after loading the view.
    }

}

extension ViewController: UITableViewDelegate, UITableViewDataSource {
    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return 5
    }
    func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        let cell = tableView.dequeueReusableCell(withIdentifier: "CustomCell") as! CustomCell
        cell.label.text = "\(indexPath.row)"
        cell.delegate = self
        return cell
    }
    func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {
        
    }
}

extension ViewController: CustomCellDelegate {
    func changeText(string: String) {
        self.label.text = string
    }
}

```
## 2. CustomCell
```swift
protocol CustomCellDelegate {
    func changeText(string: String)
}

class CustomCell : UITableViewCell {

    @IBOutlet weak var label: UILabel!
    var delegate: CustomCellDelegate?
    
    override func prepareForReuse() {
        super.prepareForReuse()
    }
    
    @IBAction func touchedButton(sender: Any) {
        delegate?.changeText(string: "changed Text")
    }

}

```


MVVM
===========
## 1. MODEL
```swift
class Model {
    var text: String
    init(text: String) {
        self.text = text
    }
}
```
## 2. MVVM
```swift
protocol ViewModelDelegate {
    func bindModel(to text: String?)
}

class ViewModel {
    var model: Model?
    var delegate: ViewModelDelegate?

    init(model: Model) {
        self.model = model
    }

    func changeModel(string: String) {
        self.model?.text = string
        self.delegate?.bindModel(to: string)
    }
}
```
## 3. ViewController
```swift
class ViewController: UIViewController {

    @IBOutlet var label: UILabel!
    var viewModel: ViewModel?

    override func viewDidLoad() {
        super.viewDidLoad()
        self.viewModel = ViewModel(model: Model(text: "hello world"))
        self.viewModel?.delegate = self
    }
    
    @IBAction func touchedButton1(sender: Any) {
        self.viewModel?.changeModel(string: "button1")
    }
    @IBAction func touchedButton2(sender: Any) {
        self.viewModel?.changeModel(string: "button2")
    }
}

extension ViewController: ViewModelDelegate {
    func bindModel(to text: String?) {
        self.label.text = text
    }
}
```
MVVM + RX
===========
## 1. MODEL
```swift
class Model {
    var text: String
    init(text: String) {
        self.text = text
    }
}
```
## 2. VIEW MODEL
```swift
import RxSwift

class ViewModel {
    var model: Model?
    let subject = PublishSubject<String>()

    init(model: Model) {
        self.model = model
    }

    func changeModel(string: String) {
        self.model?.text = string
        self.subject.onNext(string)
    }
}

```
## 3. ViewController 
```swift
class ViewController: UIViewController {

    @IBOutlet var label: UILabel!
    var viewModel: ViewModel?
    let disposeBag = DisposeBag()

    override func viewDidLoad() {
        super.viewDidLoad()
        self.viewModel = ViewModel(model: Model(text: "hello world"))
        
        self.viewModel?.subject.subscribe(onNext: { string in
            self.label.text = string
        }).disposed(by: disposeBag)
    }
    
    @IBAction func touchedButton1(sender: Any) {
        self.viewModel?.changeModel(string: "HI")
    }
}
```
