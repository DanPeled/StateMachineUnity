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
## State Machine.cs

The `StateMachine` class is a generic class that represents a state machine. It has a type parameter `T` that specifies the type of the owner of the state machine. The state machine manages a stack of `State` objects, where the top of the stack represents the current state of the machine.

```csharp
using System.Linq;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

namespace Utils.StateMachine
{
    public class StateMachine<T>
    {
        // The current state of the state machine
        public State<T> CurrentState { get; private set; }

        // The stack of states managed by the state machine
        public Stack<State<T>> StateStack { get; private set; }

        // The owner of the state machine
        T owner;

        // Constructor for the state machine
        public StateMachine(T owner)
        {
            this.owner = owner;
            StateStack = new Stack<State<T>>();
        }

        // Executes the current state's action
        public void Execute()
        {
            CurrentState?.Execute();
        }

        // Pushes a new state onto the stack and enters it
        public void Push(State<T> newState)
        {
            StateStack.Push(newState);
            CurrentState = newState;
            CurrentState.Enter(this.owner);
        }

        // Pops the current state from the stack and exits it
        public void Pop()
        {
            StateStack.Pop();
            CurrentState.Exit();
            CurrentState = StateStack.Peek();
        }

        // Changes the current state to a new state
        public void ChangeState(State<T> newState)
        {
            if (CurrentState != null)
            {
                StateStack.Pop();
                CurrentState.Exit();
            }

            StateStack.Push(newState);
            CurrentState = newState;
            CurrentState.Enter(owner);
        }

        // Pushes a new state onto the stack and waits until the current state is restored
        public IEnumerator PushAndWait(State<T> newState)
        {
            var oldState = CurrentState;
            Push(newState);
            yield return new WaitUntil(() => CurrentState == oldState);
        }

        // Gets the previous state from the stack
        public State<T> GetPrevState()
        {
            return StateStack.ElementAt(1);
        }
    }
}
```
## State.cs
using UnityEngine;

namespace Utils.StateMachine
{
    public class State<T> : MonoBehaviour
    {
        // Method that is called when entering the state
        public virtual void Enter(T owner) { }

        // Method that is called every frame while the state is active
        public virtual void Execute() { }

        // Method that is called when exiting the state
        public virtual void Exit() { }
    }
}
