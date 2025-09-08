**Using Azure CLI:**

- **Create a resource group, an Event Hubs namespace, and an Event Hub**
- **Create a producer and a consumer using Python**



### Step-by-Step Azure CLI Commands

```bash
resourceGroup=eventhub-test
location=eastus
namespaceName=eventhubsns$RANDOM
```

```bash
# Create a resource group
az group create \
  --name test-event-hub \
  --location eastus

# Create an Event Hubs namespace
az eventhubs namespace create \
  --name event-hub-ns1 \
  --resource-group $namespaceName \
  --location eastus \
  --sku Standard

# Create an Event Hub inside the namespace with two partitions
az eventhubs eventhub create \
  --name my-event-hub \
  --partition-count 2 \
  --namespace-name event-hub-ns1 \
  --resource-group test-event-hub
```



Absolutely! Here's a simple Python example that sends events to **Azure Event Hub** using the `azure-eventhub` SDK. This is perfect for sending telemetry, logs, or any custom messages.

---

### Step-by-Step Python Code to Send Events to Azure Event Hub

#### Prerequisites
- Install the SDK:
  ```bash
  pip install azure-eventhub
  ```
- You’ll need:
  - **Connection string** for your Event Hub namespace
  - **Event Hub name**

---

### Python Code Example

```python
from azure.eventhub import EventHubProducerClient, EventData

# Replace with your Event Hub connection string and name
CONNECTION_STR = "Endpoint=sb://<your-namespace>.servicebus.windows.net/;SharedAccessKeyName=<key-name>;SharedAccessKey=<key>"
EVENT_HUB_NAME = "<your-event-hub-name>"

# Create a producer client
producer = EventHubProducerClient.from_connection_string(conn_str=CONNECTION_STR, eventhub_name=EVENT_HUB_NAME)

# Create a batch of events
event_data_batch = producer.create_batch()

# Add events to the batch
event_data_batch.add(EventData("First event"))
event_data_batch.add(EventData("Second event"))
event_data_batch.add(EventData("Third event"))

# Send the batch to the Event Hub
producer.send_batch(event_data_batch)

print("Events sent successfully!")

# Close the producer
producer.close()
```

---

### What This Does
- Connects to your Event Hub
- Creates a batch of events
- Sends them in one go
- Closes the connection


Perfect! Here's a simple Python script that acts as a **consumer** for Azure Event Hub. It listens for incoming events and prints them out in real time.

---

### Python Code: Event Hub Consumer

#### Prerequisites
- Install the SDK:
  ```bash
  pip install azure-eventhub
  ```
- You’ll need:
  - **Connection string** for the Event Hub **_consumer group_**
  - **Event Hub name**

---

### Consumer Code Example

```python
from azure.eventhub import EventHubConsumerClient

# Replace with your Event Hub connection string and name
CONNECTION_STR = "Endpoint=sb://<your-namespace>.servicebus.windows.net/;SharedAccessKeyName=<key-name>;SharedAccessKey=<key>;EntityPath=<event-hub-name>"
CONSUMER_GROUP = "$Default"  # or your custom consumer group

# Callback to process events
def on_event(partition_context, event):
    print(f"Received event from partition: {partition_context.partition_id}")
    print(f"Data: {event.body_as_str()}")
    print(f"Sequence number: {event.sequence_number}\n")

# Create the consumer client
client = EventHubConsumerClient.from_connection_string(
    conn_str=CONNECTION_STR,
    consumer_group=CONSUMER_GROUP
)

# Start receiving
try:
    print("Listening for events...\n")
    with client:
        client.receive(
            on_event=on_event,
            starting_position="-1",  # "-1" means from the beginning of the stream
        )
except KeyboardInterrupt:
    print("Stopped receiving.")
```

---

### 🧠 What This Does
- Connects to your Event Hub
- Listens for events from all partitions
- Prints each event’s content and metadata




### References

https://microsoftlearning.github.io/mslearn-azure-developer/instructions/azure-events-messages/02-event-hubs-send-receive.html