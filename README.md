# CMTAutoBind
简单的实现数据与视图的绑定。当数据改变后，会主动更新与数据绑定的视图。
```
step1: 创建数据 
        let observer = CMTObserver<String>.init("-1")
        
step2: 创建视图 
        let label = CMTLabel.init(frame: CGRect.init(x: 100, y: 100, width: 100, height: 100))
        label.bind.text = observer
        
        
step3: 在任何地方修改数据，label的text属性值会自动修改
        observer.value = "333"
``` 

[![CI Status](https://img.shields.io/travis/506227061@qq.com/CMTAutoBind.svg?style=flat)](https://travis-ci.org/506227061@qq.com/CMTAutoBind)
[![Version](https://img.shields.io/cocoapods/v/CMTAutoBind.svg?style=flat)](https://cocoapods.org/pods/CMTAutoBind)
[![License](https://img.shields.io/cocoapods/l/CMTAutoBind.svg?style=flat)](https://cocoapods.org/pods/CMTAutoBind)
[![Platform](https://img.shields.io/cocoapods/p/CMTAutoBind.svg?style=flat)](https://cocoapods.org/pods/CMTAutoBind)

## Example

// 使用方式
```
class ViewController: UIViewController {
    
    var observer: CMTObserver<String>? = CMTObserver<String>.init(nil)
    let observer2 = CMTObserver<String>.init("-1")
    let observerBackGroundColor = CMTObserver<UIColor>.init(UIColor.orange)
    var count: Int = 0
    var observerModel3: CMTObserver<TestModel>? = CMTObserver<TestModel>.init(nil)

    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view.
        
        let label = CMTLabel.init(frame: CGRect.init(x: 100, y: 100, width: 100, height: 100))
        label.bind.text = observer
        label.bind.backgroundColor = observerBackGroundColor
        self.view.addSubview(label)
        
        let button = CMTButton.init(type: .system)
        button.frame = CGRect.init(x: 100, y: 200, width: 100, height: 100)
        button.bind.normalTitle = observer2
        button.addTarget(self, action: #selector(btnAction(_:)), for: .touchUpInside)
        self.view.addSubview(button)
        
        let label22 = CMTLabel.init(frame: CGRect.init(x: 100, y: 300, width: 100, height: 100))
        label22.bind.text = observer?.addListener({ v1, v2 in
            print("label22 v1=\(String(describing: v1)), v2=\(String(describing: v2))")
            return true
        })
        label22.bind.backgroundColor = observerBackGroundColor
        self.view.addSubview(label22)
        
        let label33 = CMTLabel.init(frame: CGRect.init(x: 100, y: 500, width: 100, height: 100))
        label33.bind.text = observerModel3?.ts({ model in
            return model?.name
        })
        label33.bind.backgroundColor = observerModel3?.ts({ model in
            if model?.age ?? 0 < 10 {
                return UIColor.red
            }
            return UIColor.green
        })
        self.view.addSubview(label33)
        
        let _ = CMTButton.init(frame: CGRect.init(x: 100, y: 600, width: 100, height: 100))
        
       
    }

    @objc func btnAction(_ sender: UIButton) {
        count += 1
        observer?.value = "\(count)"
        
        if count == 10 {
//            observer = nil
            observerBackGroundColor.value = UIColor.green
        }
        if count == 12 {
            observer = nil
            observer2.value = "-2"
            observerBackGroundColor.value = UIColor.yellow
        }
        
        if (count == 10) {
            let model = TestModel(name: "nnnnn", age: 10, detail: DetailModel(remark: "rrrrr", context: "ccccc"))
            observerModel3?.value = model
        }
        
        if (count == 20) {
//            observerModel3 = nil
        }
    }
}
```

## Requirements

## Installation

CMTAutoBind is available through [CocoaPods](https://cocoapods.org). To install
it, simply add the following line to your Podfile:

```ruby
pod 'CMTAutoBind'
or
pod 'CMTAutoBind', :git => "https://github.com/iJianbao/CMTAutoBind.git"
```

## Author

506227061@qq.com, jianbao_hundun@163.com

## License

CMTAutoBind is available under the MIT license. See the LICENSE file for more info.
