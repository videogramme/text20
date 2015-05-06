**>>> This page is currently being reworked <<<**




# Overview #

  * TODO: Give overview what the text is and what it is capable of, also add screenshots



<br />
# Quick Start Guide #

The quick start guide explains how to take off rapidly, and describes the necessary steps you need to take in order to get the framework running.

  * TODO: Add screenshots, links to browsers, and more information in general for each chapter. Also, explain why things are being done


## General ##
  1. Install the latest Java (v6) and a modern browser (Chrome >9, Firefox 4, Safari 5).
  1. Start the **Tracking Server**. This step is optional, but if you skip it, it will take the browser plugin a few seconds search before it gives up and uses the mouse as a default fallback. If you want to use eye tracking, the tracking server has to be running.
  1. Start the **Diagnosis**. This step is optional, but recommended to calibrate the eye tracking device and check data arrives. The proper sequence to calibrate is 1) perform _hardware calibration_ and 2) perform _local recalibration_ with transparency enabled.


## Running a Web Application ##
  1. Browser Calibration
  1. Open the **Web-App**. After a few moments (100ms up to 5 seconds, depending on whether a tracking server is running) a speech output should be generated saying either "using tracker" or "using mouse". Please note that "using tracker" only indicates that the plugin uses the tracking server and does not state anything about the type of adapter used within the server, i.e., if the adapter is mouse-bound, it will say _tracker_ anyway.

## Developing a Web Application ##

  * TODO: Describe first steps required to create a web application

Using the framework is straightforward. First, include all required libs (`jquery`, `jquery.timers` and `text20` in our case), then execute the `core.init()` function and start using gaze active handlers like `onFixation`, `onGazeOver` and so forth:

```
<script type="text/javascript" src="jquery.js"></script>
<script type="text/javascript" src="jquery.timers.js"></script>
<script type="text/javascript" src="text20.js"></script>

<script type="text/javascript">
    text20.core.init()
</script>

<body><div onFixation="alert('Hello again')">Hello World</div></body>
```



## Developing a Java Application ##

  * TODO: ...

```
// 1. Start the framework and load all plugins we have.
PluginManagerUtil pm = PluginManagerFactory.createPluginManagerX().addPluginsFrom(ClassURI.CLASSPATH);

// 2. Get a device provider, and open the nearest device
EyeTrackingDevice device = pm.getPlugin(EyeTrackingDeviceProvider.class, "eyetrackingdevice:trackingserver").openDevice("discover://nearest");

// 3. Add an eye tracking listener and have fun :-)
device.addTrackingListener(listener);
```




<br />
# Architecture Overview #

  * TODO: Put global image of architecture, including eye tracking, tracking server, diagnosis, browser, browser-plugin,  java application (framework API), ...



## The HTML and JavaScript Layer ##

  * TODO: Explain, in general, how eye tracking is integrated into a web page

### New DOM Attributes ###


Here is a brief list what handlers we support and how they work. Inside the attribute you may use `this` to access the annotated element.

```
<!-- This handler is called every time ... a fixation occurs on the annotated element -->
<div onFixation="alert('Fixation on ' + this)" ></div>

<!-- ... the user looks on the element  -->
<div onGazeOver="..." ></div>

<!-- ... the user away from the element  -->
<div onGazeOut="..." ></div>

<!-- ... continuous reading behavior is detected  -->
<div onPerusal="..." ></div>
```


## JavaScript Interface ##

The JavaScript interface allows you to register handlers on a site-wide level. These are called every time the corresponding event occurs inside the document. The only parameter `obj` is a map containing extended attributes for the event.

```
// Register a listener that is called ... as soon as the plugin is up and running
text20.connector.listener("INITIALIZED", function(obj) {})

// ... a fixation is detected anywhere on the document
// obj.x = document x position
// obj.y = document y position
// obj.type = "START" || "END"
// obj.duration = time in ms the fixation lasted
// obj.meanderivation = how coherent the fixation has been (pixels to center)
text20.connector.listener("fixation", function(obj) {})

// ... reading behavior is detected anywhere in the document
// <Handler is currently being reworked>
text20.connector.listener("perusal", function(obj) {})
```




### JavaScript Plugin Configuration ###

Here we list a set of advanced configuration options for the low-level connector. These have to be set before `core.init()` is being called.


```
// Specify where the plugin-jar is kept
text20.connector.config.archive = "somedir/text20.jar"

// Which eye tracking device to use. Possible choices:
// eyetrackingdevice:auto
// eyetrackingdevice:mouse
// eyetrackingdevice:trackingserver
text20.connector.config.trackingDevice = "eyetrackingdevice:auto"

// URL of the tracking server. 'discover://nearest' 
// will do a local search and should work fine in most 
// cases.
text20.connector.config.trackingURL = "discover://nearest"

// Enable brain tracking using the Emotiv headset?
text20.connector.config.enableBrainTracker = false

// URL of the brain tracking device 
text20.connector.config.brainTrackingURL = "discover://nearest"

// A list of extensions to load (relative path) 
text20.connector.config.extensions = ["speechio.jar"]

// If set to true, the plugins will record the 
// interaction for later replay and analysis
text20.connector.config.recordingEnabled = false

// If set to true, the plugins will record the 
// internal program flow for debugging. Disable to get slight speed ups. 
text20.connector.config.diagnosis = true

// If set to true, the plugin will check for updates and report about new versions 
// on the console. Also sends an anonymous ID for statistical purposes, but no 
// personal information whatsoever.
text20.connector.config.updateCheck =  true

// Where to store our output
text20.connector.config.sessionPath = "/tmp/sessions"


// Minimum duration in ms for coherent eye tracking data to be considered
// as a fixation
text20.core.config.fixation.minimumDuration = 100

// Maximal radius in pixel for eye tracking data to be considered as 
// coherent enough for a fixation 
text20.core.config.fixation.maxFixationRadius = 25
```


## The Tracking Server ##

  * TODO: Explain why we need a tracking server and what it does, and how


## The Diagnosis ##

  * TODO: Explain why we need a diagnosis and what it does, and how


## The Java Interface ##

  * TODO: Give overview on general Java / code layout of the plugin

### Using the Java API in Your Own Application ###

The Text 2.0 Framework was written entirely in Java and JavaScript. While the rest of this guide focus on the JavaScript and architectural side, it is quite simple to use the Java interface as well.

  * TODO: Describe general Java plugin architecture

See more infos on how to [use eye tracking in Java](UsingJavaInterface.md).


### Writing an Extension ###

  1. Create a new Eclipse project based on the `Extension Development` example contained in v1.4.2.
  1. Add the `text20.jar` and `jcores.script-*.jar` to its dependencies
  1. Write your extension (implementing `Extension` and adding @PluginImplementation)
  1. Add the following code to your extension class:
```
/** Execute this code to create you extension.jar */
public static void main(String[] args) {

    // Blacklist the text20.jar (add other libraries if needed)
    final Blacklist blacklist = new Blacklist();
    blacklist.file("text20.jar");

    // Create the extension
    JCoresLibrary.LIBRARY("MyExtensionName").blacklist(blacklist).pack(Debug.DO);
}
```
  1. Execute your extension in Eclipse as a Java application, which should produce a JAR that you can add as an dynamic extension to your HTML Text 2.0 / eye tracking application