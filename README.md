# State Machine System - Unity
A state machine system for unity

# How to use : 
1. Add the files to your project
2. In a new script, add `using Utils.StateMachine` to gain access to the state machine scripts, and create a new state machine var. e.g : 
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
3.To add create a state, make a new script, that will inherit the State class with the generic type of its owner, from the Utils.StateMachine namespace. e.g : 
```cs
using UnityEngine;
using Utils.StateMachine;
public class FreeRoamState : State<GameController>
{

}
  ```
4.Continue your code.
## Example : 
```csharp
using UnityEngine;
using Utils.StateMachine;

public class PlayerController : MonoBehaviour
{
    // A reference to the state machine
    private StateMachine<PlayerController> stateMachine;

    // Start is called before the first frame update
    void Start()
    {
        // Create a new state machine with this object as the owner
        stateMachine = new StateMachine<PlayerController>(this);

        // Push the initial state onto the state machine
        stateMachine.Push(new IdleState());
    }

    // Update is called once per frame
    void Update()
    {
        // Execute the current state's action
        stateMachine.Execute();
    }

    // Example method to transition to a new state
    public void TransitionToRunState()
    {
        stateMachine.ChangeState(new RunState());
    }

    // Example state classes
    private class IdleState : State<PlayerController>
    {
        public override void Enter(PlayerController owner)
        {
            Debug.Log("Entering IdleState");
        }

        public override void Execute()
        {
            Debug.Log("Executing IdleState");
        }

        public override void Exit()
        {
            Debug.Log("Exiting IdleState");
        }
    }

    private class RunState : State<PlayerController>
    {
        public override void Enter(PlayerController owner)
        {
            Debug.Log("Entering RunState");
        }

        public override void Execute()
        {
            Debug.Log("Executing RunState");
        }

        public override void Exit()
        {
            Debug.Log("Exiting RunState");
        }
    }
}
```
## [State Machine.cs](https://github.com/DanPeled/StateMachineUnity/blob/main/State.cs)

The `StateMachine` class is a generic class that represents a state machine. It has a type parameter `T` that specifies the type of the owner of the state machine. The state machine manages a stack of `State` objects, where the top of the stack represents the current state of the machine.
