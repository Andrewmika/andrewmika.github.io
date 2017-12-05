---
layout: post
title: Cordova官方文档整理
date: '2015-08-07 08:47:45'
tags:
- ios
- cordova
---

### 一、install Cordova
1. install Node.js
2. install cordova:
`$ sudo npm install -g cordova`

### 二、Create the App
1. create:`$ cordova create hello(directory) com.example.hello HelloWorld(projectName)`
2. add platforms:`$ cd hello `
` $ cordova platform add ios`
3. check platforms:`$ cordova platforms ls`
4. remove platforms:`$ cordova platform remove/rm blackberry10`

### 三、run the App
1. build:`$ cordova build or  $ cordova build/prepare/compile ios`
2. test:`$ cordova emulate ios`

### 四、add plugins
1. search:`$ cordova plugin search ***`
2. add:`$ cordova plugin add cordova-plugin-device`
3. check:`cordova plugin ls`

 #### advanced 
 
	1. version:`$ cordova plugin add cordova-plugin-console@latest(0.2.1)`
	2. URL:`$ cordova plugin add https://github.com/apache/cordova-plugin-console.git`
	3. tag or branch:`$ cordova plugin add https://github.com/apache/cordova-plugin-console.git#r0.2.0`
	4. subdirectory:`$ cordova plugin add https://github.com/someone/aplugin.git#:/my/sub/dir`
	5. local path:`$ cordova plugin add ../my_plugin_dir`
	
### 五、updating Cordova and project
1. updating Cordova:` $ sudo npm update -g cordova` or `$ sudo npm install -g cordova@3.1.0-0.2.0`
2. listing the info:` $ npm info cordova`
3. updating the project:`$ cordova platform update ios`

### 六、Adding Cleaver to the Xcode Project (CordovaLib Sub-Project)
1. Quit Xcode if it is running.

2. Open a terminal and navigate to the source directory for Cordova iOS.

3. Copy the **config.xml** file described above into the project directory.

4. Open Xcode and use the Finder to copy the **config.xml** file into its Project Navigator window.

5. Choose Create groups for any added folders and press Finish.

6. Use the Finder to copy the **CordovaLib/CordovaLib.xcodeproj** file into Xcode's Project Navigator

7. Select **CordovaLib.xcodeproj** within the Project Navigator.

9. Type the **Option-Command-1** key combination to show the File Inspector.

10. Choose Relative to Group in the File Inspector for the drop-down menu for Location.

11. Select the project icon in the Project Navigator, select the Target, then select the Build Settings tab.

12. Add **-force_load** and **-Obj-C** for the **Other Linker Flags** value.

13. Click on the project icon in the Project Navigator, select the Target, then select the Build Phases tab.
14. Expand Link Binaries with Libraries.

15. Select the **+** button, and add the following frameworks. Optionally within the Project Navigator, move them under the Frameworks group:

 ```
 AssetsLibrary.framework 
 CoreLocation.framework 
 CoreGraphics.framework 
 MobileCoreServices.framework
 ```
 
16. Expand Target Dependencies, the top box with that label if there's more than one box.

17. Select the **+** button, and add the CordovaLib build product.

18. Expand Link Binaries with Libraries, the top box with that label if there's more than one box.

19. Select the **+** button, and add **libCordova.a**.

20. Set the Xcode **Preferences → Locations → Derived Data → Advanced...** to **Unique**.

21. Select the project icon in the Project Navigator, select your Target, then select the Build Settings tab.

22. Search for **Header Search Paths**. For that setting, add these three values below, including the quotes:

```
 "$(TARGET_BUILD_DIR)/usr/local/lib/include"        
 "$(OBJROOT)/UninstalledProducts/include"
 "$(BUILT_PRODUCTS_DIR)"
```

### 七、using CDVViewController
two properties:**wwwFolderName** and **startPage**

### 八、about Plugin
*An iOS plugin is implemented as an Objective-C class that extends the CDVPlugin class. For JavaScript's exec method's service parameter to map to an Objective-C class, each plugin class **must be registered** as a **< feature>** tag in the named application directory's **config.xml** file.*

1. **exec**:cordova.exec
		`exec(<successFunction>, <failFunction>, <service>, <action>, [<args>]);`

This marshals a request from the UIWebView to the iOS native side, effectively calling the action method on the service class, with the arguments passed in the args array.

2. **plugin method**:

```
		- (void)myMethod:(CDVInvokedUrlCommand*)command
	    {
	        CDVPluginResult* pluginResult = nil;
	        NSString* myarg = [command.arguments objectAtIndex:0];
	        if (myarg != nil) {
	            pluginResult = [CDVPluginResult resultWithStatus:CDVCommandStatus_OK];
	        } else {
	            pluginResult = [CDVPluginResult resultWithStatus:CDVCommandStatus_ERROR messageAsString:@"Arg was null"];
	        }
	        [self.commandDelegate sendPluginResult:pluginResult callbackId:command.callbackId];
	    }
``` 

3. **The JavaScript Interface**

    note:The JavaScript provides the front-facing interface, making it perhaps the most important part of the plugin. You can structure your plugin's JavaScript however you like, but you need to call cordova.exec to communicate with the native platform, using the following syntax:
	
```
	cordova.exec(function(winParam) {},
                 function(error) {},
                 "service",
                 "action",
                 ["firstArgument", "secondArgument", 42, false]);
```
                 
*parameters*:

- function(winParam) {}: A success callback function. Assuming your exec call completes successfully, this function executes along with any parameters you pass to it.
	
- function(error) {}: An error callback function. If the operation does not complete successfully, this function executes with an optional error parameter.
	
- "service": The service name to call on the native side. This corresponds to a native class, for which more information is available in the native guides listed below.
	
- "action": The action name to call on the native side. This generally corresponds to the native class method. See the native guides listed below.
	
- [/* arguments */]: An array of arguments to pass into the native environment.

*sample Java Script*

js interface:
	
```
	 window.echo = function(str, callback) {
        cordova.exec(callback, function(err) {
            callback('Nothing to echo.');
        }, "Echo", "echo", [str]);
    };
```
	 
### 九、config.xml
1. **Global Preferences**
	*Fullscreen* allows you to hide the status bar, default is false.

2. **Multi-Platform Preferences**
 * *DisallowOverscroll* set to true to forbid the interface to display any feedback
 * *HideKeyboardFormAccessoryBar* defaults to false,set to true to hide the additional toolbar 
 * *Orientation* to lock orientation and prevent the interface from rotating in response to changed in orientation.
    
### 十、Events
1. **deviceready**:when Cordova fully loaded.
	`document.addEventListener("deviceready", yourCallbackFunction, false);`

	note:
	The deviceready event fires once Cordova has fully loaded. Once the event fires, you can safely make calls to Cordova APIs. Applications typically attach an event listener with document.addEventListener once the HTML document's DOM has loaded.
	
	The deviceready event behaves somewhat differently from others. Any event handler registered after the deviceready event fires has its callback function called immediately.
	
	*example*
	
```
	<!DOCTYPE html>
	<html>
	  <head>
	    <title>Device Ready Example</title>
	
	    <script type="text/javascript" charset="utf-8" src="cordova.js"></script>
	    <script type="text/javascript" charset="utf-8">
	
	    // Wait for device API libraries to load
	    //
	    function onLoad() {
	        document.addEventListener("deviceready", onDeviceReady, false);
	    }
	
	    // device APIs are available
	    //
	    function onDeviceReady() {
	        // Now safe to use device APIs
	    }
	
	    </script>
	  </head>
	  <body onload="onLoad()">
	  </body>
	</html>
```
2. **pause**:fires when application is put into the background
	`document.addEventListener("pause", yourCallbackFunction, false);`
	
	*example*:
	
```
	<!DOCTYPE html>
	<html>
	  <head>
	    <title>Pause Example</title>
	    <script type="text/javascript" charset="utf-8" src="cordova.js"></script>
	    <script type="text/javascript" charset="utf-8">
	    // Wait for device API libraries to load
	    //
	    function onLoad() {
	        document.addEventListener("deviceready", onDeviceReady, false);
	    }
	    // device APIs are available
	    //
	    function onDeviceReady() {
	        document.addEventListener("pause", onPause, false);
	    }
	
	    // Handle the pause event
	    //
	    function onPause() {
	    }
	
	    </script>
	  </head>
	  <body onload="onLoad()">
	  </body>
	</html>

```
 note:
	In the pause handler, any calls to the Cordova API or to native plugins that go through Objective-C do not work, along with any interactive calls, such as alerts or console.log(). They are only processed when the app resumes, on the next run loop.
	
3. **resume**:when an application is retrieved from the background
	`document.addEventListener("resume", yourCallbackFunction, false);`
	
	*example*:
	
```
 <!DOCTYPE html>
	<html>
	  <head>
	    <title>Resume Example</title>
	
	    <script type="text/javascript" charset="utf-8" src="cordova.js"></script>
	    <script type="text/javascript" charset="utf-8">
	
	    // Wait for device API libraries to load
	    //
	    function onLoad() {
	        document.addEventListener("deviceready", onDeviceReady, false);
	    }
	
	    // device APIs are available
	    //
	    function onDeviceReady() {
	        document.addEventListener("resume", onResume, false);
	    }
	
	    // Handle the resume event
	    //
	    function onResume() {
	    }
	
	    </script>
	  </head>
	  <body onload="onLoad()">
	  </body>
	</html>

```
 note:
	Any interactive functions called from a pause event handler execute later when the app resumes, as signaled by the resume event. These include alerts, console.log(), and any calls from plugins or the Cordova API, which go through Objective-C.
