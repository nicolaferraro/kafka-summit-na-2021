# Apache Camel K at Kafka Summit NA 2021

The demo creates 3 bindings:
- A Telegram Chat is bound to a Kafka topic
- The Kafka topic is bound to a log sink to display messages on the console
- The Kafka topic is also bound to a AWS S33 sink using streaming upload, to progressively create files that contain batches of messages

Writing at least 5 messages on the Telegram chat will trigger the creation of a file on S3. Every 5 messages will trigger a creation of a new file. All data is printed on the logs by the `kafka-to-log` binding.

## Requirements

- A Kubernets or OpenShift cluster with the Apache Camel K 1.6.0 operator installed
- A Kafka cluster created on cloud.redhat.com with a topic named `chatbot-x` and a service account to access it
- A Telegram bot created using the [@botfather](https://t.me/botfather) with related authorization token
- An AWS account, a S3 bucket and a service account to access it

## Running it

1. First sync the Kamelet (to ensure you're using the right version of them):

```
kubectl apply -f kamelets/
```

2. Edit the `telegram-to-kafka.yaml` file to add your Telegram and Kafka credentials.

3. Run `telegram-to-kafka`:

```
kubectl apply -f telegram-to-kafka.yaml
```

4. Edit the `kafka-to-log.yaml` file to add your Kafka credentials.

5. Run `kafka-to-log`:

```
kubectl apply -f kafka-to-log.yaml
```

Write a *message on the chat with your Telegram bot*, then watch the logs of `kafka-to-log` to see the messages:

```
kubectl logs -f -l camel.apache.org/integration=kafka-to-log
```


6. Edit the `kafka-to-s3.yaml` file to add your Kafka and S3 credentials.

7. Run `kafka-to-s3`:

```
kubectl apply -f kafka-to-s3.yaml
```

Write *at least 5 messages on the chat* with your Telegram bot, then watch the files on the S3 bucket.

Enjoy!
