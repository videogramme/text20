## Basic Steps ##

The Text 2.0 Framework bundle not only comes with an easy to use browser plugin to add eye tracking support right in your web page, it also comes with a full fledged library that enables your Java app to facilitate eye tracking data in the most simple way. It works like this:

  1. Add the file `text20.jar` to your Java project.
  1. Start the _tracking server_
  1. Use, for example, the code snippets below to access the raw eye tracking data:


<br><br>
<h2>How to get ...</h2>
<h3>... a) a tracking device</h3>

After you performed the steps in the introduction, use this code to get a tracking device and raw gaze data. Note, you may <b>not</b> use this code inside browser extensions, as when running an extensions, the PluginManager, TrackingDevice and others already exist!<br>
<br>
<pre><code>// Start the framework. Do this only *once* in your whole application. <br>
PluginManager pluginManager = PluginManagerFactory.createPluginManager();<br>
pluginManager.addPluginsFrom(new URI("classpath://*"));<br>
<br>
// Obtain an eye tracking device connection. In this case, the next best tracking server will be found. <br>
EyeTrackingDeviceProvider deviceProvider = pluginManager.getPlugin(EyeTrackingDeviceProvider.class, new OptionCapabilities("eyetrackingdevice:trackingserver"));<br>
EyeTrackingDevice device = deviceProvider.openDevice("discover://nearest");<br>
</code></pre>

<br>
<h3>... b) raw gaze data</h3>
Make sure you have a tracking device (see step 'a'). Then add a tracking listener:<br>
<br>
<pre><code>// Add a low-level listener to a eye tracking device. <br>
device.addTrackingListener(new EyeTrackingListener() {<br>
    public void newTrackingEvent(final EyeTrackingEvent event) {<br>
        System.out.println(event);<br>
    }<br>
});<br>
</code></pre>


<br>
<h3>... c) fixations</h3>

The main part of the evaluation is performed by a <code>GazeEvaluator</code>. It allows you to detect, for example, fixations. Make sure you have a tracking device (see step 'a').<br>
<br>
<pre><code>// Get a gaze evaluator for a given tracking device. <br>
GazeEvaluatorManager mgr = pluginManager.getPlugin(GazeEvaluatorManager.class);<br>
GazeEvaluator evaluator = mgr.createEvaluator(device);<br>
<br>
// Tell the evaluator to listen for fixations<br>
evaluator.addEvaluationListener(new FixationListener() {        <br>
    public void newEvaluationEvent(FixationEvent arg0) {<br>
        // Check type of event<br>
        if (arg0.getType() != FixationEventType.FIXATION_START) return;<br>
        ...<br>
    }<br>
});<br>
</code></pre>