# Pub Sub Model:
1. What is the main benefit of the Pub-Sub model?
- **Answer:** It decouples the producer and consumer, enabling scalable and asynchronous communication.

2. Can a subscriber receive messages from multiple publishers?
- **Answer:** Yes, a subscriber can subscribe to multiple document types from different publishers.

3. How does webMethods ensure only relevant subscribers receive a message?
- **Answer:** The broker matches published documents to subscribers based on document types or filters.

4. What happens if a subscriber is offline when a message is published?
- **Answer:** Depending on the configuration, the broker can store the message and deliver it when the subscriber is back online.

5. What components in webMethods are used for implementing the Pub-Sub model?
- **Answer:** Publisher and subscriber services, broker (Universal Messaging), and Integration Server are key components.

