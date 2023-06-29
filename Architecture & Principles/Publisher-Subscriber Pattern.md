Used for [Event Grids](https://learn.microsoft.com/en-us/azure/event-grid/), [Event Hubs](https://learn.microsoft.com/en-us/azure/event-hubs/), and [Service Bus](https://learn.microsoft.com/en-us/azure/service-bus-messaging/).
Other technologies include:
- Redis
- RabbitMQ
- Apache Kafka

### Definition
Enable an application to announce events to multiple interested consumers asynchronously, without coupling the senders to the receivers. 

**Also called:** Pub/Sub messaging

# Context and problem
The cloud-based and distributed applications, components of the system often need to provide information to other components as events happen.

Asynchronous messaging is an effective way to decouple senders from consumers, and avoid blocking the sender to wait for a response. However, using a deidcated message queue for each sonumer does not effectively scale to man consumers. Also, some of the consumers might be interested in only a subset of the information. How can the sender annouce events to all interested consumers without knowing their identities.

# Solution

Introduce an asynchronous messaging subsystem that includes the following:
- A messaging channel used by the sender. The sender packages events into messages, using a known message format, and sends these messages via the channel. The sender in this pattern is also called the *publisher*.
	- Quick Definitions:
		- Message - packet of data
		- Event - a message that notifies components about a change or an action that has taken place.
- There is one output channel per consumer. The consumers are known as *subscribers*.
- 