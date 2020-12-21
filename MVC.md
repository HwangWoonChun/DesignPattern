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

## 4. helper
```swift
protocol Bindable {
    associatedtype ModelType
    
    var model: ModelType! { get set }
    func bindModel()
}

extension Bindable where Self: UIViewController {
    mutating func bind(to model: Self.ModelType) {
        self.model = model
        bindModel()
    }
}
```

## 5. SceneDelegate 
```swift
//
//  SceneDelegate.swift
//  test
//
//  Created by mmxsound on 2020/12/21.
//

import UIKit

class SceneDelegate: UIResponder, UIWindowSceneDelegate {

    var window: UIWindow?


    func scene(_ scene: UIScene, willConnectTo session: UISceneSession, options connectionOptions: UIScene.ConnectionOptions) {
        // Use this method to optionally configure and attach the UIWindow `window` to the provided UIWindowScene `scene`.
        // If using a storyboard, the `window` property will automatically be initialized and attached to the scene.
        // This delegate does not imply the connecting scene or session are new (see `application:configurationForConnectingSceneSession` instead).
        guard let windowScene = (scene as? UIWindowScene) else { return }
        
        window = UIWindow(windowScene: windowScene)
        
        let data = Model(text: "hello world")
        let navigation = UINavigationController()
        
        let storyboard = UIStoryboard(name: "Main", bundle: nil)
        if var vc = storyboard.instantiateViewController(withIdentifier: "ViewController") as? ViewController {
            vc.bind(to: data)
            navigation.viewControllers = [vc]
            self.window?.rootViewController = navigation
        }
        
        self.window?.makeKeyAndVisible()
    }


    func sceneDidDisconnect(_ scene: UIScene) {
        // Called as the scene is being released by the system.
        // This occurs shortly after the scene enters the background, or when its session is discarded.
        // Release any resources associated with this scene that can be re-created the next time the scene connects.
        // The scene may re-connect later, as its session was not necessarily discarded (see `application:didDiscardSceneSessions` instead).
    }

    func sceneDidBecomeActive(_ scene: UIScene) {
        // Called when the scene has moved from an inactive state to an active state.
        // Use this method to restart any tasks that were paused (or not yet started) when the scene was inactive.
    }

    func sceneWillResignActive(_ scene: UIScene) {
        // Called when the scene will move from an active state to an inactive state.
        // This may occur due to temporary interruptions (ex. an incoming phone call).
    }

    func sceneWillEnterForeground(_ scene: UIScene) {
        // Called as the scene transitions from the background to the foreground.
        // Use this method to undo the changes made on entering the background.
    }

    func sceneDidEnterBackground(_ scene: UIScene) {
        // Called as the scene transitions from the foreground to the background.
        // Use this method to save data, release shared resources, and store enough scene-specific state information
        // to restore the scene back to its current state.
    }


}
```
