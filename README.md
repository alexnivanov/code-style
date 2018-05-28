# code-style
This is a common code style for high-level OO languages (Java/Scala/Kotlin/Swift/...) which is a result of many years of development. You can adopt it for your projects to make the code more clean, readable and sustainable.

This is basically an extension (it never breaks existing rules) to the default IntelliJ/Eclipse formatting.

## Lines spacing

### Classes body should have blank lines in the beginning and in the end
```scala
class MyActor(out: ActorRef) extends Actor {

  private val log = Logger(this.getClass)

  def receive: PartialFunction[Any, Unit] = {
    case msg: String =>
      out ! ("I received your message: " + msg)
  }

}
```

### Different classes in the same file should be separated by 2 lines
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

### Linked classes (class/companion objects) can be separated by 1 line:
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
