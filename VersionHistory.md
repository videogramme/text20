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


This release has been successfully tested with Chrome 14 and Firefox 6.0 on Windows and Mac,
Java 7.0, Processing 1.2.1. Higher versions usually also work.



<br />
### Version 1.3.0 (released 20.9.2010) ###

  * Browser calibration. Yay! (No more fiddling with offsets)
  * DHTML elements can now be deregistered
  * Improved diagnosis visualisations
  * Fixed recording compatibility
  * Updated examples
  * Many bug fixes

<br />
### Version 1.2.0 (released 13.7.2010) ###

  * Joint release of PEEP and The Framework
  * Unified infrastructure for the tracking server, diagnosis, plugin and Processing
  * Many bug fixes
  * Now supports TET API v5 (enabled by default, switch to v2 in config.properties)
  * Improved debugging and error logging for tracking server
  * PEEP startup is now way faster

<br />
### Version 1.0.1 (released 24.6.2010) ###


  * Improved startup speed of tracking server
  * Fixed diagnosis loading problems on Mac OS
  * Fixed configuration loading bug
  * Fixed plugin loading bug

<br />
### Version 1.0 (released 14.6.2010) ###

  * Infinite number of bug fixes
  * Brain tracking support
  * Reworked JavaScript layer (v4)
  * Added head position listener
  * Works with Google Chrome 5, Safari 5, Firefox 3.5
  * Extensions support
  * XStream (XML based) session recording
  * Improved discovery: eye tracking connections should be very reliable now
