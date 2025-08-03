# Main OR Important Lead Interview Quetions and Answers

## 1. what is rebalancing in kafka how heart beat works in consumer ?
## 2. In springboot application is having application.properties and application.yml file which one will be consider ? and why ? and how to know which one os taken first ?
## 3. spring resilience vs circuit breaker in spring boot
## 4. how to create immutable class
## 5. purpose of functional interfaces in java and default methods in interfaces
## 6. What is the difference JPA vs Hibernates ?
## 7. Can you please give proper examples of Hibernate associations/mappings
## 8. Explain in detail Twelve-Factor Methodology in a Spring Boot Microservice
## 9. In Spring Application how shed lock works for cron jobs  can you explain in detail
10. How @Transactional works in Spring Data Explain in detail
11. 








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

 <img width="828" height="605" alt="image" src="https://github.com/user-attachments/assets/1fdd0695-072e-4772-8c64-1e683b01ccc9" />

 <img width="823" height="501" alt="image" src="https://github.com/user-attachments/assets/fcf0c964-6b33-4b99-9661-0f9b990fad6b" />

 <img width="838" height="260" alt="image" src="https://github.com/user-attachments/assets/5e55d158-3f3a-430e-9f2a-15c2a17d6c2b" />

 <img width="819" height="389" alt="image" src="https://github.com/user-attachments/assets/12cb6817-7c2e-412a-9035-01a470b6601f" />

 <img width="802" height="538" alt="image" src="https://github.com/user-attachments/assets/28c2fcde-53d9-4adc-bcf7-630b9a7c4dc7" />

 <img width="810" height="774" alt="image" src="https://github.com/user-attachments/assets/d6fada86-eb0f-4e75-9692-91a6a5ce0eac" />

 #### From G-Gemini Answer

 <img width="770" height="669" alt="image" src="https://github.com/user-attachments/assets/54b18c6f-4a41-41f4-beec-cdf6bc6f9539" />

 <img width="793" height="334" alt="image" src="https://github.com/user-attachments/assets/c794b2b6-af79-4568-acaa-b6edc87d6c6c" />

<img width="734" height="552" alt="image" src="https://github.com/user-attachments/assets/bb76a698-85e4-4541-97ef-391c78db991e" />

<img width="766" height="163" alt="image" src="https://github.com/user-attachments/assets/ba2eb6d6-4bea-46f8-9b16-c9826bcf2c86" />

<img width="741" height="616" alt="image" src="https://github.com/user-attachments/assets/6d5b723e-6918-4da1-bd41-ce9475b282da" />

<img width="732" height="53" alt="image" src="https://github.com/user-attachments/assets/8675f699-ddd5-4743-a185-af10fb62fdcc" />

```java
resilience4j.circuitbreaker:
  instances:
    serviceA:
      registerHealthIndicator: true
      slidingWindowSize: 10 # Number of calls to consider for failure rate
      minimumNumberOfCalls: 5 # Minimum calls before failure rate calculation starts
      failureRateThreshold: 50 # Percentage of failures to open the circuit
      waitDurationInOpenState: 5s # Time to wait in open state before half-open
      permittedNumberOfCallsInHalfOpenState: 3 # Number of calls allowed in half-open state
```
<img width="745" height="142" alt="image" src="https://github.com/user-attachments/assets/1f3480b4-0df8-4290-9462-d91e1d0524e8" />


#### 4) how to create immutable class

<img width="795" height="569" alt="image" src="https://github.com/user-attachments/assets/06ab7d51-b1aa-4ce7-8347-5a07834f5a13" />

```java
public final class Person {
    private final String name;
    private final int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    // Only getters, no setters
    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }
}

```
<img width="761" height="64" alt="image" src="https://github.com/user-attachments/assets/8fb7ac62-46b3-42f9-924f-d9b98b309a28" />

```java
import java.util.Date;

public final class Employee {
    private final String name;
    private final Date joiningDate; // Mutable

    public Employee(String name, Date joiningDate) {
        this.name = name;
        // Defensive copy
        this.joiningDate = new Date(joiningDate.getTime());
    }

    public String getName() {
        return name;
    }

    public Date getJoiningDate() {
        // Return defensive copy
        return new Date(joiningDate.getTime());
    }
}

```

<img width="827" height="334" alt="image" src="https://github.com/user-attachments/assets/69cbb6a1-a7e7-42d3-8fc4-c811198499db" />

<img width="852" height="604" alt="image" src="https://github.com/user-attachments/assets/f76b4cb1-c1ec-438b-b624-d717d253648d" />

<img width="847" height="460" alt="image" src="https://github.com/user-attachments/assets/2f1057e3-d85a-4ee3-b628-ea85e5121f19" />

<img width="785" height="767" alt="image" src="https://github.com/user-attachments/assets/1a1ace7e-97ef-4aeb-92d6-e06925cc865a" />

#### 5) purpose of functional interfaces in java

<img width="829" height="495" alt="image" src="https://github.com/user-attachments/assets/2550597b-1433-4412-a7ef-6f9a9bf5deb1" />

<img width="838" height="398" alt="image" src="https://github.com/user-attachments/assets/1e1ca77a-afaf-4487-9636-8852a4c46508" />

<img width="809" height="708" alt="image" src="https://github.com/user-attachments/assets/86ad5eff-f056-4b7d-8125-109416ed8f1a" />

<img width="814" height="430" alt="image" src="https://github.com/user-attachments/assets/c98d1c12-e480-4799-b830-844ce9a8b133" />

<img width="830" height="298" alt="image" src="https://github.com/user-attachments/assets/da70c7e0-18c0-45ea-811d-45d43e8b93fa" />

<img width="869" height="556" alt="image" src="https://github.com/user-attachments/assets/3c06a011-d806-4fca-a26c-b8cba1278321" />

<img width="796" height="709" alt="image" src="https://github.com/user-attachments/assets/890b7125-081d-43f5-b456-c01fcfebd6c2" />

<img width="822" height="643" alt="image" src="https://github.com/user-attachments/assets/d0f349fe-612e-40ed-8228-9b35600621c4" />

<img width="824" height="757" alt="image" src="https://github.com/user-attachments/assets/9ce8f54b-8ba8-4ada-8554-7cce13e67809" />

#### 6. What is the difference JPA vs Hibernates ?

<img width="798" height="622" alt="image" src="https://github.com/user-attachments/assets/d5261bc3-8f0a-4505-b283-d25717c4e12c" />

<img width="849" height="528" alt="image" src="https://github.com/user-attachments/assets/d9260ced-e45c-4f44-8c49-3ab0cdf3bc57" />

<img width="819" height="388" alt="image" src="https://github.com/user-attachments/assets/9907fd02-468f-402a-95a3-f0d6e93f0851" />

<img width="835" height="695" alt="image" src="https://github.com/user-attachments/assets/92c54d2a-3b44-40dc-9236-cdad931501b8" />

<img width="883" height="595" alt="image" src="https://github.com/user-attachments/assets/fb36cc2f-1852-4bc1-bf48-4bd6fff4d39a" />

<img width="868" height="655" alt="image" src="https://github.com/user-attachments/assets/bdd3f089-d57d-41b3-8948-b6a3b8311ef3" />

<img width="830" height="383" alt="image" src="https://github.com/user-attachments/assets/6b475b14-17b3-47b7-9cc2-a979da10afa8" />

<img width="804" height="423" alt="image" src="https://github.com/user-attachments/assets/d47a253a-c081-4ee4-9670-5743ba0ed79f" />

<img width="802" height="484" alt="image" src="https://github.com/user-attachments/assets/45f65d26-14d9-4163-b5fc-d97067702010" />

<img width="802" height="346" alt="image" src="https://github.com/user-attachments/assets/5e6a66ee-2562-4c40-8911-c4ca36631521" />

<img width="845" height="746" alt="image" src="https://github.com/user-attachments/assets/083bd77a-0c24-42ab-9ab3-1c850d4dbc6e" />

<img width="807" height="661" alt="image" src="https://github.com/user-attachments/assets/e7a8f09b-a3ba-4be2-97cd-afb7e9d223dd" />

<img width="813" height="580" alt="image" src="https://github.com/user-attachments/assets/d6e81dc1-17e9-4961-860e-d2ccf70c9baf" />

<img width="839" height="509" alt="image" src="https://github.com/user-attachments/assets/8533fafd-e150-4227-8a98-3a3c0e430222" />

#### 7. Can you please give proper examples of Hibernate associations/mappings 

<img width="820" height="331" alt="image" src="https://github.com/user-attachments/assets/3c697e15-7bea-4b3f-8898-c5088dd87b3e" />

<img width="811" height="549" alt="image" src="https://github.com/user-attachments/assets/d7b36630-fd30-4c42-87f5-b854bf041daa" />

<img width="800" height="663" alt="image" src="https://github.com/user-attachments/assets/337c2543-9138-4792-928f-b7054cb36fdf" />

<img width="812" height="196" alt="image" src="https://github.com/user-attachments/assets/fbec4550-7f4b-4df6-8d17-16b7f867eb75" />

<img width="818" height="718" alt="image" src="https://github.com/user-attachments/assets/30cc530f-5df9-4b80-80e1-9bd51703b13f" />

<img width="768" height="530" alt="image" src="https://github.com/user-attachments/assets/5b1baf9b-1413-4c73-91a1-4e5de677c818" />

<img width="818" height="582" alt="image" src="https://github.com/user-attachments/assets/c91f3ab6-e8a9-44aa-bfe4-d89c9066183b" />

<img width="804" height="406" alt="image" src="https://github.com/user-attachments/assets/34cb9a85-40c0-4a4e-81e5-9aa162b3c27c" />

#### 8. Explain in detail Twelve-Factor Methodology in a Spring Boot Microservice

<img width="844" height="444" alt="image" src="https://github.com/user-attachments/assets/5be1454a-6667-4c6a-b4f8-8c0150f43996" />

<img width="836" height="549" alt="image" src="https://github.com/user-attachments/assets/e4acce6c-ef0b-45c4-9db5-412d15b50f31" />

<img width="710" height="543" alt="image" src="https://github.com/user-attachments/assets/4f4809e0-0bc9-4749-8d61-8542c36c573b" />

<img width="830" height="385" alt="image" src="https://github.com/user-attachments/assets/6c76b6af-f792-4f2d-b73d-6a63a53d6752" />

<img width="826" height="464" alt="image" src="https://github.com/user-attachments/assets/76901d77-8307-440b-8647-05a2f2e969ac" />

<img width="824" height="390" alt="image" src="https://github.com/user-attachments/assets/98e21917-ab71-413e-a80c-ed6270d77840" />

<img width="883" height="581" alt="image" src="https://github.com/user-attachments/assets/bb59cd71-51f3-48ee-9d34-e91e2c37a586" />

<img width="926" height="719" alt="image" src="https://github.com/user-attachments/assets/25120ecc-18cb-46f7-87c7-45ec9e1d685d" />

<img width="803" height="349" alt="image" src="https://github.com/user-attachments/assets/9bbd0eb6-9fc1-472a-b7dd-f25a023e9ae7" />

<img width="844" height="540" alt="image" src="https://github.com/user-attachments/assets/e56cf0d3-1f8a-4576-9bef-5dc8fec7dc77" />

<img width="820" height="499" alt="image" src="https://github.com/user-attachments/assets/c3597c4d-7214-4c2b-a2f8-c5f83612b63a" />

<img width="816" height="639" alt="image" src="https://github.com/user-attachments/assets/7941bc2e-ec20-4020-99f1-73d76dac18f7" />

#### 9. In Spring Application how shed lock works for cron jobs  can you explain in detail

<img width="826" height="352" alt="image" src="https://github.com/user-attachments/assets/96c353cd-6249-4dbc-8eef-2d582011200e" />

<img width="802" height="566" alt="image" src="https://github.com/user-attachments/assets/bf5bbc88-f0b3-4256-8435-d17f4ed26375" />

<img width="789" height="457" alt="image" src="https://github.com/user-attachments/assets/c161600f-78d3-4dc3-9ec0-5f71277327e8" />

<img width="795" height="725" alt="image" src="https://github.com/user-attachments/assets/0345804a-7123-49ad-9ed4-7d573fd66664" />

<img width="806" height="602" alt="image" src="https://github.com/user-attachments/assets/97a2e205-878f-4a75-a0ca-ba1db4632e81" />

<img width="798" height="615" alt="image" src="https://github.com/user-attachments/assets/57e59fb3-c171-42cc-8385-bbc90d42f54d" />

#### 10. How @Transactional works in Spring Data Explain in detail

<img width="819" height="372" alt="image" src="https://github.com/user-attachments/assets/070f27fd-16b8-494d-8ea5-935dfd598850" />

<img width="796" height="668" alt="image" src="https://github.com/user-attachments/assets/a9db03cd-716f-4720-bee5-728e5fd9bd27" />

<img width="794" height="730" alt="image" src="https://github.com/user-attachments/assets/1ecf1a21-e2af-43e9-9e58-b4973ee9c5bc" />

<img width="801" height="775" alt="image" src="https://github.com/user-attachments/assets/5f7081ad-c377-4ef4-ab0a-ed561a1304e6" />

<img width="817" height="688" alt="image" src="https://github.com/user-attachments/assets/4efd8841-d0dc-454f-a0b9-2535a9198e46" />

<img width="811" height="467" alt="image" src="https://github.com/user-attachments/assets/1ad422ca-21b1-4b39-8194-0ce7aaeb1856" />

<img width="826" height="418" alt="image" src="https://github.com/user-attachments/assets/d92a6b22-bd7b-4695-a5c3-05a3dc64dbd3" />





