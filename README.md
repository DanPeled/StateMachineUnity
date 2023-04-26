# State Machine System - Unity
A state machine system for unity

# How to use : 
1. Add the files to your project
1. In a new script, add `using Utils.StateMachine` to gain access to the state machine scripts, and create a new state machine var. e.g : 
```cs
using UnityEngine;
using System.Collections;
using Utils.StateMachine;

public class MyCustomScript : MonoBehaviour {
  
  StateMachine stateMachine;
  // Use this for initialization
  void Start () {

  }

  // Update is called once per frame
  void Update () {

  }
  ```
1.To add create a state, make a new script, that will inherit the State class with the generic type of its owner, from the Utils.StateMachine namespace. e.g : 
```cs
using UnityEngine;
using Utils.StateMachine;
public class FreeRoamState : State<GameController>
{

}
  ```
  1.Continue your code.
