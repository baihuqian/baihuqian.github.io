---
layout: post
title: Akka Notes
tags: Scala Akka
---


## General
Actor Model: OO implementation of large scale concurrent application. It can be hierarchical.

Actor: Objects with encapsulated state and behavior, and communication happens exclusively by exchanging messages.

ActorRef: It represents a single actor during communication.

## Actor
Create Actors: extend Actor and implement the receive method. The receive method is a set of case statements for all messages the actor receives and their corresponding behaviors.

Props is a configuration class to specify options for the creation of actors. An instance of it is passed to the `actorOf` factory method of `ActorSystem` and `ActorContext` to create actors.

Actor API:

* `self`: an `ActorRef` of the actor
* `sender`: reference sender Actor of the message
* `context`
* Overridable life-cycle hooks: `preStart`; `postStop`; `preRestart`; `postRestart`, all return `Unit`.

Message passing:

* The `!` operator (or tell method) to send a one-way asynchronous message (`actor ! Message()`) and returns `Unit` immediately.
* The `?` operator (or ask method) sends a message and returns a `Future` of possible reply. Usually the Future is `pipeTo` an actor to register `onComplete` handler when the `Future` returns. Note `Future` is a generic type.
* Message can be forwarded and sender reference is maintained: `<target> forward <message>`

Messages can be defined in sender or receiver. All messages belong to an Actor share a common empty sealed trait, and the messages are declared as case classes so that the fields for message content is automatically generated:

```scala
sealed trait All<Actor_name><Description>Message
case class <description>Message(content1: type1, content2: type2, …) extends All<Actor_name><Description>Message
```

## FSM

State * Event -> Action + State (next)

States are defined using the similar manner as messages, but use case object instead:

```scala	
sealed trait State
case object <description>State extends State
```

Use startWith(<initial_state>, <initial_data>) to define start state. Use initialize to start the FSM
Usually there is one when(<state>) { … } declaration per state. Inside when block, each Event(<message>, <data>) is matched with case statement. Inside each case statement, use goto(<next_state>) or stay to define next state, and use using modifier to define data used in next state:

```scala
when(<current_state>) {
 	case Event(<message_received>, <data>) => {
      	// do something here
      	stay using <new_data>
      	// or
      	// goto(<new_state>) using <new_data>
 	}
}
```

`whenUnhandled { … }` statement handles unhandled massages.

```scala
onTransition {
 	case <old_state> -> <new_state> => {
      	// do something here
 	}
}
```
handles operations when state changes. 

Stop a FSM by the result state as `stop()`. It takes an optional parameter reason, which is one of `Normal` (default), `Shutdown`, or `Failure`. You can use `onTermination` handler to execute cleanup:

```scala
onTermination {
 	case StopEvent(FSM.Normal, state, data) => { … }
 	case StopEvent(FSM.Shutdown, state, data) => { … }
 	case StopEvent(FSM.Failure(cause), state, data) => { … }
}
```
