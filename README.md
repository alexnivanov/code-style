# code-style
This is a common code style for high-level OO languages (Java/Scala/Kotlin/Swift/...) which is a result of many years of development. You can adopt it for your projects to make the code more clean, readable and sustainable.

This is basically an extension (it never breaks existing rules) to the default IntelliJ/Eclipse formatting.

## Rule of thumb

The general rule to be followed and the ultimate reason behind all the subsequent rules:

**_EVERY_ (including whitespace) symbol in the code base should make sense and serve one or several purposes:**
1. _Required_ logic
1. Code maintainability
1. Code readability

Therefore we should strive to remove unused code including comments and whitespaces as much as possible.

**The secondary rule of thumb is that we should strive to have the minimum number of symbols in our codebase unless it damages maintainability and readability.**

## Lines spacing

### Classes body should have blank lines in the beginning and in the end:
```scala
class MyActor(out: ActorRef) extends Actor {

  private val log = Logger(this.getClass)

  def receive: PartialFunction[Any, Unit] = {
    case msg: String =>
      out ! ("I received your message: " + msg)
  }

}
```

### Different classes in the same file should be separated by 2 lines:
```scala
class MyWebSocket @Inject()(implicit system: ActorSystem, mat: Materializer) {

  def socket(barId: Int) = WebSocket.accept[String, String] { request =>
    ActorFlow.actorRef { out =>
      MyWSActor.props(out)
    }
  }

}


object MyWSActor {

  def props(out: ActorRef) = Props(new MyWSActor(out))

}
```

### Linked classes (eg. class/companion object) can be separated by 1 line:
```scala
object MyWSActor {

  def props(out: ActorRef) = Props(new MyWSActor(out))

}

class MyWSActor(out: ActorRef) extends Actor {

  private val log = Logger(this.getClass)

  def receive: PartialFunction[Any, Unit] = {
    case msg: String =>
      out ! ("I received your message: " + msg)
  }

}
```

### Case statements should be separated by blank line:
```scala
def active(actors: Map[Int, List[MyWSActor]]): Receive = {
    case AddMyActor(actor) =>
      actors.get(actor.id) match {
        case None =>
          context become active(actors + (actor.id -> List(actor)))

        case Some(list) =>
          context become active(actors + (actor.id -> (list :+ actor)))
      }

    case RemoveMyActor(actor) =>
      context become active(actors - actor.id)
  }
```
