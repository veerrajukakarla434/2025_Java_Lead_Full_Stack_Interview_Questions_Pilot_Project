# Main OR Important Lead Interview Quetions and Answers

### 1) what is rebalancing in kafka how heart beat works in consumer ?
### 2) In springboot application is having application.properties and application.yml file which one will be consider ? and why ? and how to know which one os taken first ?
### 3) spring resilience vs circuit breaker in spring boot





#### 1) what is rebalancing in kafka how heart beat works in consumer ?

<img width="805" height="280" alt="image" src="https://github.com/user-attachments/assets/47feacc3-01c8-4ca4-9ab7-21fda74a4a06" />

<img width="661" height="256" alt="image" src="https://github.com/user-attachments/assets/853e9f8f-b4aa-489f-9157-e613f042847c" />

<img width="817" height="531" alt="image" src="https://github.com/user-attachments/assets/50d91bfc-412c-4475-b61c-87ac6bff4e1c" />

<img width="773" height="328" alt="image" src="https://github.com/user-attachments/assets/dd6d93b6-729c-43c5-b2a5-36e1f1309e68" />

<img width="810" height="266" alt="image" src="https://github.com/user-attachments/assets/e35f97f3-9060-4e06-aa8a-72732f4abf9c" />

<img width="811" height="576" alt="image" src="https://github.com/user-attachments/assets/daea4340-b644-4213-a982-ff4423b3060b" />

<img width="812" height="494" alt="image" src="https://github.com/user-attachments/assets/47350257-176f-48ec-96fc-215550a1f4cf" />

<img width="838" height="341" alt="image" src="https://github.com/user-attachments/assets/40c05bdb-c86a-4bb2-a363-1d3af9ddf982" />

<img width="821" height="485" alt="image" src="https://github.com/user-attachments/assets/b9d5557f-3ee9-4d32-91c2-71593b475245" />

```java
import org.apache.kafka.clients.consumer.*;
import org.apache.kafka.common.TopicPartition;

import java.time.Duration;
import java.util.*;

public class RebalanceAwareConsumer {

    private static final String TOPIC = "my-topic";
    private static final String GROUP_ID = "my-group";
    private static final String BOOTSTRAP_SERVERS = "localhost:9092";

    private static final Map<TopicPartition, OffsetAndMetadata> currentOffsets = new HashMap<>();

    public static void main(String[] args) {
        Properties props = new Properties();
        props.put(ConsumerConfig.BOOTSTRAP_SERVERS_CONFIG, BOOTSTRAP_SERVERS);
        props.put(ConsumerConfig.GROUP_ID_CONFIG, GROUP_ID);
        props.put(ConsumerConfig.KEY_DESERIALIZER_CLASS_CONFIG,
                "org.apache.kafka.common.serialization.StringDeserializer");
        props.put(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG,
                "org.apache.kafka.common.serialization.StringDeserializer");

        // Heartbeat-related configs (optional tuning)
        props.put(ConsumerConfig.SESSION_TIMEOUT_MS_CONFIG, "10000");
        props.put(ConsumerConfig.HEARTBEAT_INTERVAL_MS_CONFIG, "3000");
        props.put(ConsumerConfig.MAX_POLL_INTERVAL_MS_CONFIG, "300000");

        // Disable auto-commit, we'll commit manually
        props.put(ConsumerConfig.ENABLE_AUTO_COMMIT_CONFIG, "false");

        try (KafkaConsumer<String, String> consumer = new KafkaConsumer<>(props)) {

            consumer.subscribe(Collections.singletonList(TOPIC), new ConsumerRebalanceListener() {

                @Override
                public void onPartitionsRevoked(Collection<TopicPartition> partitions) {
                    // Called BEFORE a rebalance starts (we're about to lose these partitions)
                    System.out.println("Partitions revoked: " + partitions);
                    consumer.commitSync(currentOffsets); // commit current offsets before losing them
                }

                @Override
                public void onPartitionsAssigned(Collection<TopicPartition> partitions) {
                    // Called AFTER a rebalance completes
                    System.out.println("Partitions assigned: " + partitions);
                }
            });

            while (true) {
                ConsumerRecords<String, String> records = consumer.poll(Duration.ofMillis(1000));

                for (ConsumerRecord<String, String> record : records) {
                    System.out.printf("Consumed: key=%s, value=%s, partition=%d, offset=%d%n",
                            record.key(), record.value(), record.partition(), record.offset());

                    // Track offsets for manual commit
                    TopicPartition partition = new TopicPartition(record.topic(), record.partition());
                    OffsetAndMetadata offset = new OffsetAndMetadata(record.offset() + 1);
                    currentOffsets.put(partition, offset);
                }

                // Commit after processing each batch
                if (!currentOffsets.isEmpty()) {
                    consumer.commitAsync(currentOffsets, (offsets, exception) -> {
                        if (exception != null) {
                            System.err.println("Commit failed: " + exception.getMessage());
                        }
                    });
                }
            }
        }
    }
}

```

<img width="805" height="649" alt="image" src="https://github.com/user-attachments/assets/00ab5ce2-cbbc-4d71-928c-bd33d2313952" />

<img width="795" height="422" alt="image" src="https://github.com/user-attachments/assets/62b04c1c-f071-40eb-9e59-e77d30dd45b9" />

<img width="845" height="355" alt="image" src="https://github.com/user-attachments/assets/cbcc1069-eb74-47cb-b54d-4724ac572e4c" />

<img width="831" height="413" alt="image" src="https://github.com/user-attachments/assets/a5b7e5ce-9f11-4d9d-a5f2-2f0cfa393cdd" />

<img width="808" height="552" alt="image" src="https://github.com/user-attachments/assets/2a2a012b-1867-4f00-8439-727ad0051228" />

<img width="842" height="553" alt="image" src="https://github.com/user-attachments/assets/b685d6e7-7012-4b57-b9ce-48e9e0e7ce16" />

<img width="815" height="635" alt="image" src="https://github.com/user-attachments/assets/fd34d75e-15b4-40fc-9967-cb974f70523a" />

#### 2) In springboot application is having application.properties and application.yml file which one will be consider ? and why ?

<img width="797" height="526" alt="image" src="https://github.com/user-attachments/assets/633106d4-4b50-4213-9b16-356d3d5b6e44" />

<img width="806" height="781" alt="image" src="https://github.com/user-attachments/assets/febd93a0-ac4d-4410-a071-5baeed057800" />

<img width="815" height="717" alt="image" src="https://github.com/user-attachments/assets/57be930e-ad02-471a-93f7-7466df7d73b6" />

<img width="799" height="566" alt="image" src="https://github.com/user-attachments/assets/c262cbf9-4966-428a-a081-657723ddf335" />

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.core.env.*;
import org.springframework.boot.CommandLineRunner;
import org.springframework.beans.factory.annotation.Autowired;

@SpringBootApplication
public class DemoApplication implements CommandLineRunner {

    @Autowired
    private ConfigurableEnvironment environment;

    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }

    @Override
    public void run(String... args) {
        printPropertySource("server.port");
        printPropertySource("custom.name");
    }

    private void printPropertySource(String key) {
        for (PropertySource<?> propertySource : environment.getPropertySources()) {
            if (propertySource.containsProperty(key)) {
                Object value = propertySource.getProperty(key);
                System.out.printf("🔍 Key: %-20s | Value: %-15s | Source: %s%n",
                        key, value, propertySource.getName());
                return;
            }
        }
        System.out.println("❌ Property not found: " + key);
    }
}

```
<img width="814" height="388" alt="image" src="https://github.com/user-attachments/assets/2a7fc961-412e-415a-bd5e-cd942051d4fa" />

<img width="802" height="462" alt="image" src="https://github.com/user-attachments/assets/6ec63e0e-f1f5-46f6-9584-485ca6e87316" />

#### 3) spring resilience vs circuit breaker in spring boot




