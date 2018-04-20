# XDMicroJSBridge

[![CI Status](http://img.shields.io/travis/458770054@qq.com/XDMicroJSBridge.svg?style=flat)](https://travis-ci.org/458770054@qq.com/XDMicroJSBridge)
[![Version](https://img.shields.io/cocoapods/v/XDMicroJSBridge.svg?style=flat)](http://cocoapods.org/pods/XDMicroJSBridge)
[![License](https://img.shields.io/cocoapods/l/XDMicroJSBridge.svg?style=flat)](http://cocoapods.org/pods/XDMicroJSBridge)
[![Platform](https://img.shields.io/cocoapods/p/XDMicroJSBridge.svg?style=flat)](http://cocoapods.org/pods/XDMicroJSBridge)                        
A most simple iOS bridge for communication between Obj-C and JavaScript in UIWebViews
## Example

To run the example project, clone the repo, and run `pod install` from the Example directory first.

## Installation

XDMicroJSBridge is available through [CocoaPods](http://cocoapods.org). To install
it, simply add the following line to your Podfile:

```ruby
pod 'XDMicroJSBridge'
```

## Usage
### Init bridge
```objC
#import "XDMicroJSBridge.h"
@property (nonatomic, strong) UIWebView *webview;
@property (nonatomic, strong) XDMicroJSBridge *bridge;
self.bridge = [XDMicroJSBridge bridgeForWebView:_webview];
```
### Register methods
```objC
__weak typeof(self) weakself = self;
    [_bridge registerAction:@"camerapicker" handler:^(NSArray *params, XDMCJSBCallback callback) {
        dispatch_async(dispatch_get_main_queue(), ^{
            if (callback) {
                weakself.callback = callback;
            }
            UIImagePickerController *cameraVC = [[UIImagePickerController alloc] init];
            cameraVC.delegate = weakself;
            cameraVC.sourceType = UIImagePickerControllerSourceTypeCamera;
            [weakself presentViewController:cameraVC animated:YES completion:nil];
        });
    }];
```
### Call methods in JavaScript
```javaScript
<script>
// 调起相机
    function clickcamera() {
        XDMCBridge.camerapicker(function (response) {
            //response['photos']：回调回来的照片数组
            var photos = response['photos'];
            var insert = document.getElementById('insert');
            for(var i = 0; i < photos.length; i++) {
                var img = new Image(100,100);
                img.src = photos[i];
                insert.appendChild(img);
            }
        });
    }
</script>
```
You can see the details in demo project.
## Author

caixindong

## License

XDMicroJSBridge is available under the MIT license. See the LICENSE file for more info.
