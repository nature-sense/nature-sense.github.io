

# Actor Framework

The Naturesense  **Actor Framework** is a simple C++ implementation of the  [**Actor Pattern** ](https://en.wikipedia.org/wiki/Actor_model) using [Boost ASIO](https://www.boost.org/doc/libs/latest/doc/html/boost_asio.html)  and [C++ (20) coroutines](https://en.cppreference.com/w/cpp/language/coroutines.html).

## Introduction

The fundamentai building block of the actor frameworks is an **Actor**. 

An **Actor** is a processing element that receives messages from a mailbox, perform some processing on the message, and optionally sends messages to one or more other actors via their mailboxes.  The actor processes one message at a time, and any state associated with the processing is purely local to the actor (there is no global state in an actor system).

![actos](images/actos.png)

An actor can handle different message types,  which are all sent through the same mailbos, and processed in the order they are received.

## Abstract Actor Class

The core functionality of all actors is defines in the abstract Actor class:.

This defines an Actor without its mailbox.

```c++
#include <toml++/toml.hpp>
#include <boost/asio.hpp>

namespace asio = boost::asio;

namespace io::naturesense {
	class Actor {
	public:
    	Actor(asio::io_context *ioc, toml::parse_result *config) : ioc(ioc), config(config){};
    	virtual ~Actor();
  
    	virtual void initialise();
    	virtual asio::awaitable<void> start();
    	virtual void stop();

	protected:

    	asio::io_context *ioc;
    	toml::parse_result *config;
	};
}
```



### Fields

| Field   | Type                 | Description                                                  |
| ------- | -------------------- | ------------------------------------------------------------ |
| *ioc    | asio::io_context     | Boost asio event context                                     |
| *config | `toml::parse_result` | The actor's configuration. Parse output from the `system.toml` configuration file |



Methods

| Method                                                       | Description                                                  |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| `Actor(asio::io_context *ioc, toml::parse_result *config) : ioc(ioc), config(config){};` | Constructor                                                  |
| `virtual void initialise()`                                  | Actor specific initialisation. Typically this will use the configuration to set up the actor. |
| `virtual asio::awaitable<void> start()`                      | Run the actor.  This is an asio asynchronous coroutine. which typically waits on the mailbox. |
| `virtual void stop()`                                        | Stop the actor, cleanup and exit. (Q shutdowm message?)      |
|                                                              |                                                              |



## Mailboxes and Messages

Each Actor has a mailbox through which it receives messages - which are essentially instructions for it to do something. Other actors can place messages into the mailbox. These actors need only to know the identity of the actor (i.e which mailbox). 

**This mechanism is typesafe, meaning that attempts to pass unsupported message types to a mailbox will be detected at compilation time.**

### Principles

**In the Actor Framework mailboxes are implemented using [Boost Asio concurrent channels](https://www.boost.org/doc/libs/latest/doc/html/boost_asio/reference/experimental__concurrent_channel.html)** . This is a "thread-safe channel that can be used to pass messages between different parts  of the same application". 

Unusually, Boost Asio concurrent channels are defined using callback signatures for each message-type the channel can handle. 

```c++
ex::concurrent_channel<void(RecycleReference), void(CaptureReference)> mailbox{ioc->get_executor(), 10};
```

So here the mailbox can pass RecycleReference and CaptureReference messages.

#### Receiving messages from other Actors

To receive messages from the mailbox the receive function is pased a lambda function to 

```c++
mailbox.async_receive(
    [](RecycleReference recycle_ref) {
        // Process message
    },
    [](CaptureReference capture_ref) {
        // Process message
    }
);
```



#### Sending Messages to other actors

To end a message to another actor, this actor must add it to the target's mailbox e.g.

```c++
co_await target_actor->mailbox->send_async(capture_ref);
```

To achieve this, every actor must be linked to its target actors at system initialisation time (see below).

### Message Definitions

Boost Asio Concurrent Channels support any c++ types including primitive types e,g. Int, float etc, and complex types e.g structure, class etc.

**By convention in the Actor Framework, messages are implemented as structs.** e.g.

```c++
namespace io::naturesense {
	struct RecycleReference {
    uint64_t id;
	};
}
```

To allow messages to be sent to other Actor an abstract class should be created which allows a recipiend actor to create and interface that a sending actor can use to send e.g for RecycleReference an abstract class. 

```c++
class RecycleReferenceActor {
public:
    virtual ~RecycleReferenceActor() = 0;
    virtual asio::awaitable<void> post_async(RecycleReference ref);
    virtual void post_sync(RecycleReference ref);
};
```

This should be used as a "mixin" to the Actor class allowing to to provide an interface to send RecycleReference messages to it e.g.

```c++
class MyActor : Actor, RecycleReferenceActor {
public:

		// implementation of Actor
		using Actor::Actor;
    ~Actor() override;
    void initialise() override;
    asio::awaitable<void> start() override;
    void stop() override;
    
    // implementation of RecycleReferenceActor
    asio::awaitable<void> post_async(RecycleReference ref) override;
    void post_sync(RecycleReference ref) override;
   
private:
  	// the mailbox
    ex::concurrent_channel<void(RecycleReference)> mailbox{ioc->get_executor(), 10};
}
```



Note : 

 A mailbox can support multiple message types and it must be declared in a single declaration containing all the ,essage types. 



#### Creating a Mailbox



