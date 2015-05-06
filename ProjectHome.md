## Old Page Starts Here ##

This is the development portal of the [Text 2.0 project](http://text20.net), part of [DFKI](http://dfki.de). In here you will find the browser plugin required to create eye tracking apps using HTML, CSS and JavaScript, as well as some diagnosis and related tools. The main features are

  * Eye tracking right in your browser, combined seamlessly with CSS and JavaScript.
  * Supports (almost) arbitrary HTML - gaze active image tags rotated using 3D CSS transforms? No problem.
  * Runs on Mac OS, Windows and (in theory) on Linux
  * Google Chrome, Safari and Firefox supported
  * Create extensions, write and add your favorite algorithms dynamically.
  * Record interaction sessions as  XML streams and replay them (w. limitations)
  * Supports all Tobii trackers using the TET API (e.g., 1750, T60, X120, TX300 ...)
  * Mouse fallback and a simulator with configurable accuracy included
  * Brain tracking using the Emotiv headset (experimental)
  * Also provides a [Java API](http://code.google.com/p/text20/wiki/UsingJavaInterface) to access eye tracking data
  * Open source (LGPL)


If you're looking for more multimedia interaction and less document interaction, have a look at [PEEP, the Processing Easy Eyetracker Plugin](http://peep.text20.net). For the latest news, also have a look at the [Text 2.0 Twitter Channel](http://twitter.com/text20/).



<br />
## Showcase ##

Below you will find a number of videos and applications that were created with the help of the Text 2.0 Framework and PEEP.

<a href='http://www.youtube.com/watch?feature=player_embedded&v=8QocWsWd7fc' target='_blank'><img src='http://img.youtube.com/vi/8QocWsWd7fc/0.jpg' width='425' height=344 /></a><a href='http://www.youtube.com/watch?feature=player_embedded&v=vwCerXyvVMk' target='_blank'><img src='http://img.youtube.com/vi/vwCerXyvVMk/0.jpg' width='425' height=344 /></a>
<a href='http://www.youtube.com/watch?feature=player_embedded&v=RmBI7WYveYo' target='_blank'><img src='http://img.youtube.com/vi/RmBI7WYveYo/0.jpg' width='425' height=344 /></a><a href='http://www.youtube.com/watch?feature=player_embedded&v=7UZhB2ad8GE' target='_blank'><img src='http://img.youtube.com/vi/7UZhB2ad8GE/0.jpg' width='425' height=344 /></a>

More showcases and explanations can be found in the [show case section](ShowCase.md).


<br />
### Version 1.4.1 (released 6.9.2011) ###



This release contains mainly stability and usability enhancements. Especially the
tracking server and the browser plugin are way more robust now and contain many
little features that help you to prevent and detect problems. The changes in detail:


**General**
  * Added "Lightning", an intelligent text cursor placement tool
  * JSPF Log Converter added (allows you to convert debug logs into text files to  trace errors)
  * New network protocol which is 80% faster
  * General stability improvements
  * Code cleanups


**Browser Plugin**
  * Image URLs are now recorded for the replay
  * Debugging-overlay extension added, can be triggered from JavaScript
  * JavaScript debugging added for browsers which support it
  * Fixation listener now provides more information
  * Chrome 10 and Firefox 4 (and later) should be able to reload the page when using the object tag
  * Simplified extension development, you can now tag methods with @ExtensionMethod, the rest is done automatically. The old interface was moved to DynamicExtension
  * Changed extension internals (e.g., moved singleton services to the InformationBroker)
  * Various bug fixes


**Diagnosis**
  * Starts up faster now in most situations
  * Splash screen added
  * Various bug fixes


**PEEP**
  * Added precision-API for more accurate timing information


**Tracking Server**
  * The server is much more robust now (it even survives reconnecting the tracker's LAN  cable and still continues to track afterwards)
  * Lots of debugging information added, the most common problems are now detected automatically
  * Default config changed for improved startup speed of the clients
  * The server is now able to check for framework-updates and download them for you
  * Tray icon added


This release has been successfully tested with Chrome 13 and Firefox 6.0 on Windows and Mac,
Java 7.0, Processing 1.2.1. Higher versions usually also work.




More news can be found in the [version history](VersionHistory.md).



<br />
## Usage ##

Start the **tracking server**, start the **diagnosis** to check and calibrate your gaze data and finally **run your web app**.

If you have any problems, please use the [discussion group](http://groups.google.com/group/text20). Also one of our users, Amit, has set up _#text2.0_ on _irc.freenode.net_ for real time discussions.

See the [Usage Guide](UsageGuide.md) and the [FAQ](FAQ.md) for more information.



<br />
## Coding ##

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

Also see the [Usage Guide](UsageGuide.md) for more information.


<br />
## Requirements ##

The project has been tested on Mac OS (simulator only) and Windows. Linux support untested, but it could work. Results anyone?

  * [Java 6 & 7](http://java.com/)
  * [Google Chrome 13](http://www.google.de/chrome)(`*`), [Safari 5](http://www.apple.com/safari/) or [Firefox 6](http://www.mozilla-europe.org/)
  * [Tobii Eye Tracker](http://www.tobii.com/) (optional, requires installed Tobii 2.x SDK; a simulator with configurable accuracy is included)

(`*`) **Note**: Due to its unstable Java-bridge Google Chrome is known to have problems in some versions. In case you encounter ClassNotFound exceptions please try another version / browser (Chrome 15 has problems, 16-beta at time of writing doesn't).

<br />
## Citing ##

For any web related project, please link to [text20.net](http://text20.net). If you drop us a message and send us screenshots, we can also put you in the showcase.  If you are using our framework for scientific work please cite either:

  * [The Text 2.0 Framework - Writing Web-Based Gaze-Controlled Realtime Applications Quickly and Easily](http://data.text20.net/documentation/paper.text20framework.pdf), Ralf Biedert, Georg Buscher, Sven Schwarz, Manuel Möller, Andreas Dengel, Thomas Lottermann. In proceedings of the International Workshop on Eye Gaze in Intelligent Human Machine Interaction (EGIHMI) held in conjunction with IUI 2010. _(For references to the technical framework)._

  * [Text 2.0](http://data.text20.net/documentation/paper.text20.pdf), Ralf Biedert, Georg Buscher, Sven Schwarz, Jörn Hees, Andreas Dengel. In CHI '10 extended abstracts on human factors in computing systems. _(To refer to the idea)._




<br />
## Credits & Copyright ##

The Text 2.0 Framework has been created in innumerable hours of work and was redesigned a number of times until we finally considered it to be _production-ready_. It is built on top of the EyeTracker2Java library created by [Georg Buscher](http://gbuscher.com) and Florian Mittag. Georg was also the person that had the most significant influence on the framework's development through his ideas, suggestions and participation, let alone his influence on the whole Text 2.0 project.

The framework also includes the hard work of Arman Vartan, Thomas Lottermann, Eugen Massini, Andreas Buhl, as well as the suggestions, tips and feedback of many more. Thank you very much!

Kaiserslautern, June 2010
- [Ralf Biedert](http://dfki.de/~biedert), [German Research Center for Artificial Intelligence](http://dfki.de)

