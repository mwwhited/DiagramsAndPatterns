# Data Access Patterns

## Command Query Responsibility Segregation (CQRS)

```plantuml
@startuml

title: Data Access Pattern - Command Query Responsibility Segregation (CQRS) 

()User
cloud "Front End"{
    [Read API] as Read
    [Write API] as Write
}
frame Services {
    database "Data Store" as Storage
    queue "Event Store" as Queue
}

User --> Read : Query
User --> Write : Command

Read --> Storage : Data Query
Write --> Queue : Store Command
Queue -> Storage : Handle Command

@enduml
```

## Create, Read, Update, Delete (CRUD)

```plantuml
@startuml

title: Data Access Pattern - Create, Read, Update, Delete (CRUD)

()User

[Create]
[Read]
[Update]
[Delete]

database "Data Store" as Storage

User --> Read : Get
User --> Create : Post
User --> Update : Put
User --> Delete : Delete

Read   --> Storage : Select
Create --> Storage : Insert
Update --> Storage : Update
Delete --> Storage : Delete

@enduml
```

## Event Sourcing

```plantuml
@startuml

title: Data Access Pattern - Event Sourcing

class AggregateRoot {
  +apply(event: Event): void
  +getState(): State
  +replayEvents(events: List<Event>): void
}

class EventStore {
  +save(event: Event): void
  +getEvents(aggregateId: int): List<Event>
}

class Event {
  +id: int
  +timestamp: Date
  +data: string
}

class State {
  +data: string
}

class Command {
  +execute(): void
}

class EventPublisher {
  +publish(event: Event): void
}

AggregateRoot --> EventStore : uses
AggregateRoot --> State : has
AggregateRoot --> Command : processes
AggregateRoot --> EventPublisher : publishes
EventStore --> Event : stores
EventPublisher --> Event : publishes

@enduml
```
