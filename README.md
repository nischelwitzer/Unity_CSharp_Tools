# Unity_CSharp_Tools
Unity C# Tools

## State Engine

```
private enum GameStates { Start, Loop, Wind };
private GameStates myGameState;

myGameState = GameStates.Start;
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

