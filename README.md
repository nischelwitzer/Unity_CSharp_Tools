# Unity_CSharp_Tools
Unity C# Tools

## State Engine - GameStates

```
private enum GameStates { Start, Loop, Wind };
private GameStates myGameState;

myGameState = GameStates.Start; // Init

String infoString = "STATE: [" + (int)myGameState + " <> " + myGameState.ToString() + "] ";
```

use with

```
switch (myGameState)
    {
        case (GameStates.Start):
            break;
        default:
            break;
    }
```

## TimeOut Timer

```
// Timer Tools - for timeout
private float timeStart;
private float timeUser;
public int timeLoop = 999; // loop
public int timeOut = 999; // time outTime - if no interaction

timeStart = Time.time;
timeUser = 0.0f;

InvokeRepeating("UpdateTimer", 5, 1);
```

### Methods

```
void UpdateTimer()
{
    if (timerText != null)
    {
        int mySec = (int)getTimeUser;
        timerText.text = mySec.ToString();

        if (mySec > timeOut)  // TimeOut no User interaction -> go to Start
        {
            myGameState = GameStates.Start;
            TimeUserInteract(); // simulate interaction, set UserTime back to 0
        }
    }
    showStateInfo();
}

private void TimeUserInteract()
{ // for timeout, interaction happened
    timeUser = Time.time;
}

private float getTimeUser
{
    get { return Time.time - timeUser; }
}
```

## Key Simulation for Arduino/Uduino

* neuer InputManager
* PlayerInput

```
void OnKey1_Simulator()  // S1 Simulation from Arduino - see PlayerInput Script
{
    OnDataReceived("S1"); // Simulate Arduino S1 Serial Command
}
 
void OnDataReceived(string data, UduinoDevice deviceName = null) // Arduino
{
    Debug.Log("UDUINO Got: " + data + " from " + (deviceName == null ? "Simulator" : deviceName.name) + " >> " + counter);
    TimeUserInteract(); // Interaction > reset user timer

    // if (GameInfoAction != null) GameInfoAction(data);
    GameInfoAction?.Invoke(data);

    if (data == "S1") // wind is made
    {
        if (myGameState == GameStates.Loop) // only now react on wind
        {
            mySec.Enqueue(Time.time); // at click to frequency calculator 
            curFrequency = getClickFreq;
            Debug.Log("CLICK: Frquency > "+curFrequency+" [" + mySec.Count + "] Threshold:"+config.iActionFrequency);

            if (curFrequency > config.iActionFrequency) // check if wind is fast enough
            {
                myGameState = GameStates.Wind;
                StartVideo(1, false); // videoplayer 1, video 1, no looping
            }
        }
    }

    if (data == "B11") myGameState = GameStates.Loop; 

    if (data == "D1") myGameLanguage = GameLanguage.Deutsch;
    if (data == "E1") myGameLanguage = GameLanguage.Englisch;

    showStateInfo();
}
```

## Time Tools & Reset

```
// ##########################################################################
// ##### Time Tools & Reset
// ##########################################################################

private void TimeUserInteract()
{ // for timeout, interaction happened
    timeUser = Time.time;
}

private float getTimeUser
{
    get { return Time.time - timeUser; }
}

private float getTimeTotal
{
    get { return Time.time - timeStart; }
}

private float getClickFreq
{
    get
    {
        float splitTime = (Time.time - clickWindowSec);

        while ((mySec.Count > 0) && (mySec.Peek() < splitTime))
            mySec.Dequeue();

        // foreach (float val in mySec)
        //    if (val < splitTime) mySec.Dequeue();

        return ((float)mySec.Count / clickWindowSec * 60.0f);
    }
}
```


## Info and Debugging Tools

```
// ##########################################################################
// ##### Info and Debugging Tools
// ##########################################################################

private String showStateInfo(String moreInfo = "")
{
    String infoString = "STATE: [" + (int)myGameState + " <> " + myGameState.ToString() + "] ";
    byte currentVPlayer = (byte)(videoPlayerToggle ? 1 : 0);

    infoString += "Video: " + currentVPlayer;

    curFrequency = getClickFreq; // calc new frequency
    infoString += " Clicks/min> " + curFrequency + " cpm [#" + mySec.Count + "] Thres:" + config.iActionFrequency + "\n" +
        myGameLanguage.ToString();

    if (statusText != null) statusText.text = infoString;

    // infoString += "  Time:" + getTimeUser.ToString("00.00");
    // infoString += "  TotalTime:" + getTimeTotal.ToString("00.00");

    return infoString;
}
```

Toggle Info Canvas

```
void OnTab_ToggleInfoCanvas()
{
    bool stateInfoCanvas = myInfoCanvas.activeSelf;
    Debug.Log("InfoCanvas State old:" + stateInfoCanvas + " to " + !stateInfoCanvas);
    myInfoCanvas.SetActive(!stateInfoCanvas);
}
```

    
