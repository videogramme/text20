# API Overview #

In general these function are provided by the PEEP device object:

```

// a boolean, true if the user is looking, false if not
device.isLooking;

// current fixation in window coordinates
device.x;
device.y;

// current raw gaze data in window coordinates
device.eyes.rawX;
device.eyes.rawY;

// current head position, ranges from 0.0 to 1.0
device.head.x;
device.head.y;
device.head.z;

// Prints the current status of the device.
device.debug()

// Obtains precision data (1.4.1).
device.precisionData()

// Gets last event (1.4.2, may be null).
device.lastEvent()
```



### Precision Data ###

The precision data object holds  the following elements:

```
// The time in ms when this event was recorded
data.rawTime;

// True if the raw data is valid 
data.rawValid;

// The raw x position in pixels
data.rawX;

// The raw y position in pixels
data.rawY;

// The duration of the last observed fixation
data.fixationTime;

// True if the last observed fixation is valid
data.fixationValid;

// The x position of the last observed fixation
data.fixationX;

// The y position of the last observed fixation
data.fixationY;
```

In general, the raw measurements are always newer than the last observed fixation.



### Last Event ###

The last event reflects the last observed eye tracking event. Note that the values are always presented in screen coordinates, not application coordinates!

```

// The center of gaze
last.getGazeCenter();
  
// The head position in (x,y,z); x and y are relative from 0 to 1, z in mm from the camera.
last.getHeadPosition();
  
// Distance of the left eye (mm)
last.getLeftEyeDistance();

// Distance of the left eye (mm)
last.getRightEyeDistance();
  
// Relative position (0 .. 1) of the left eye onto the screen
last.getLeftEyeGazePosition();
  
// Relative position (0 .. 1) of the right eye onto the screen
last.getRightEyeGazePosition();

// Position of the left eye in (x,y,z); x and y are relative from 0 to 1, z in mm from the camera.
last.getLeftEyePosition();
  
// Position of the right eye in (x,y,z); x and y are relative from 0 to 1, z in mm from the camera.
last.getRightEyePosition();

// Gaze position of the right eye onto the screen (pixels)
last.getRightEyeGazePoint();
  
// Gaze position of the left eye onto the screen (pixels)
last.getLeftEyeGazePoint();
  
// Pupil size of the left eye (mm)
last.getPupilSizeLeft();
  
// Pupil size of the left eye (mm)
last.getPupilSizeRight();
  
```