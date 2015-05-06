Start by downloading SWIG(http://www.swig.org) and copying the following files from the api examples over to a fresh directory: edk.h, edkErrorCode.h and EmoStateDLL.h. Copy edk.dll, edk\_utils.dll as well as edk.lib in a subdirectory called "lib".

In addition to the other header files I had to add stdbool.h which looks like this:

```
#ifndef _STDBOOL_H
#define _STDBOOL_H

/* C99 Boolean types for compilers without C99 support */
/*  */
#if !defined(__cplusplus)

#if !defined(__GNUC__)
/* _Bool builtin type is included in GCC */
typedef enum { _Bool_must_promote_to_int = -1, false = 0, true = 1 } _Bool;
#endif

#define bool _Bool
#define true 1
#define false 0
#define __bool_true_false_are_defined 1

#endif

#endif
```

Now include stdbool.h in the head of edk.h.

To get you started I'll supply you with the SWIG interface file that I created and have been using successfully: edkWrapper.i (should be contaned in the ressources directory in the repository)

Using SWIG, all the Java classes and the C-Wrapper code will be created automatically for you:
```
swig.exe -java edkWrapper.i
```

Now we'll have to compile the wrapper into a .dll. For this you need this definition file(more on this later): edkWrapper.def (should be contaned in the ressources directory in the repository)

With this you are now ready to compile using the Visual Studio compiler in the console from your directory wh ere you have all the files like so:
```
> C:\Programme\Microsoft Visual Studio 9.0\VC\bin\vsvars32.bat

> cl -I"C:\Programme\Java\jdk1.6.0_20\include" -I"C:\Programme\Java\jdk1.6.0_20\include\win32" -I"." -MT -LD edkWrapper_wrap.c 
-FeedkWrapper.dll /link /OPT:NOREF /LIBPATH:"./lib" /DEFAULTLIB:edk  /MAP /DEF:edkWrapper.def
```

Obviously, you'll have to edit the path to JDK to your needings.

If you've made it till here and have created your very own edkWrapper.dll you're pretty much done and can start playing around with it in Java.

One more advice though, whenever you add some custom wrapper function to the edkWrapper.i you'll have to add a mapping to the edkWrapper.def file. To do so compile as usual and then open the `*.map` file in which you should find the function written in a weird way. If you f.e. have added a function myFunction(int foo) it might show up in the `.map` as "`_Java_1myFunction@8`". In the .def file you'd then have to add "`Java_1myFunction=_Java_1myFunction@8`". In other words: remove the leading underscore and the stuff after and including the @-sign.

Now copy all the `*.java` files into your Java project and make the edkWrapper.dll accessible by it(f.e. by copying it into the system32 directory).

Here is a more or less exact replica of example 5 that comes with the API only in Java that goes to show how to read out EEG raw data:

```
public class RawEEGReader {

	public static void main(String[] args) {

		System.loadLibrary("edk");
		System.loadLibrary("edk_utils");
		System.loadLibrary("edkWrapper");
		
		EE_DataChannel_t channels[] = {
				EE_DataChannel_t.ED_COUNTER,
				EE_DataChannel_t.ED_AF3, 
				EE_DataChannel_t.ED_F7, 
				EE_DataChannel_t.ED_F3, 
				EE_DataChannel_t.ED_FC5, 
				EE_DataChannel_t.ED_T7, 
				EE_DataChannel_t.ED_P7, 
				EE_DataChannel_t.ED_O1, 
				EE_DataChannel_t.ED_O2, 
				EE_DataChannel_t.ED_P8, 
				EE_DataChannel_t.ED_T8, 
				EE_DataChannel_t.ED_FC6, 
				EE_DataChannel_t.ED_F4, 
				EE_DataChannel_t.ED_F8, 
				EE_DataChannel_t.ED_AF4, 
				EE_DataChannel_t.ED_GYROX, 
				EE_DataChannel_t.ED_GYROY, 
				EE_DataChannel_t.ED_TIMESTAMP, 
				EE_DataChannel_t.ED_FUNC_ID, 
				EE_DataChannel_t.ED_FUNC_VALUE, 
				EE_DataChannel_t.ED_MARKER, 
				EE_DataChannel_t.ED_SYNC_SIGNAL	
		};

		SWIGTYPE_p_void 			eEvent 			= edk.EE_EmoEngineEventCreate(); 
		SWIGTYPE_p_EmoStateHandle 	eState 			= edk.EE_EmoStateCreate();
		SWIGTYPE_p_unsigned_int 	nSamplesTaken 	= edk.createPUInt(0);		
		SWIGTYPE_p_unsigned_int 	pUser 			= edk.createPUInt(0L);
		
		System.out.println("===================================================================");
		System.out.println("Example to show how to log the EmoState from EmoEngine/EmoComposer.");
		System.out.println("===================================================================");
		System.out.println("Press '1' to start and connect to the EmoEngine                    ");
		System.out.println("Press '2' to connect to the EmoComposer                            ");
		System.out.print(">> ");

		int connectionStatus;
		int choice;
		try {
			choice = System.in.read();
		} catch (IOException e) {
			choice = 0;
		}
		
		if ( choice == 49 ) {	// Engine

			System.out.println("Connection to Emotiv Engine");
			connectionStatus = edk.EE_EngineConnect();
			
		} else if ( choice == 50) {
			System.out.println("Connection to Remote Emo Composer Engine");
			connectionStatus = edk.EE_EngineRemoteConnect("127.0.0.1", 1726);
		} else {
			System.out.println("Invalid Input");
			return;
		}
		
		System.out.println("Connection: "+connectionStatus);
		
		if ( connectionStatus == EdkErrorCodes.EDK_OK ) {
			
			boolean readyToCollect = false;
			
			SWIGTYPE_p_void hData = edk.EE_DataCreate();
			edk.EE_DataSetBufferSizeInSec(1.0f);
			
			int  state 		= EdkErrorCodes.EDK_OK;
			long user 		= 0;
			long startTime 	= System.currentTimeMillis();
			
			while ( ((System.currentTimeMillis() - startTime)/1000) < 30 ) {
				
				state = edk.EE_EngineGetNextEvent(eEvent);
				
					
				if ( state == EdkErrorCodes.EDK_OK ) {
					EE_Event_t eventType = edk.EE_EmoEngineEventGetType(eEvent);

					edk.EE_EmoEngineEventGetUserId(eEvent, pUser);
					
					if ( eventType == EE_Event_t.EE_UserAdded ) {

						System.out.println("User added");
						
						user = edk.pUIntToUInt(pUser);
						System.out.println("UserId: "+user);
						int code = edk.EE_DataAcquisitionEnable(user,true);
						System.out.println("DataAcquisitionState: "+(code==EdkErrorCodes.EDK_OK ? "OK" : code));
						
						readyToCollect = true;
					}
				}
			
			
				if ( readyToCollect ) {		
					
					edk.EE_DataUpdateHandle(user, hData);
					
					int code = edk.EE_DataGetNumberOfSample(hData, nSamplesTaken);
					
					long sampleCount = edk.pUIntToUInt(nSamplesTaken);
					
					if ( sampleCount > 0 ) {
						
						System.out.println("Samples Taken: "+sampleCount);
						
						double data[] = new double[channels.length];
						SWIGTYPE_p_double buffer = edk.createDataBuffer(nSamplesTaken);
						
						for ( int sample = 0; sample < sampleCount; sample++ ) {
							for (  int i = 0; i < channels.length; i++ ) {
								EE_DataChannel_t channel = channels[i];
								edk.EE_DataGet(hData, channel, buffer, sampleCount);
								data[i] = edk.readFromDataBuffer(buffer, sample);
							}
							
							System.out.println("Sample: " + Arrays.toString(data));
						}
						
						edk.freeDataBuffer(buffer);
					}
					
				}
				
				
				try {
					Thread.sl eep(100);
				} catch (InterruptedException e) {
					e.printStackTrace();
				}
			}
			
			edk.freePUInt(pUser);
			edk.EE_DataFree(hData);
		}
		
		edk.EE_EngineDisconnect();
		edk.EE_EmoStateFree(eState);
		edk.EE_EmoEngineEventFree(eEvent);
		edk.freePUInt(nSamplesTaken);
	}

}
```
Keep in mind that you can't read out raw data by connecting to the EmoComposer, even though it's portrayed differently in the example.