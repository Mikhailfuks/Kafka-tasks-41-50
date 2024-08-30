tasks 41 

if (Magic.validatingOnly) {
    
    Magic.reportProgress('Validating.');
}
else {

    Magic.reportProgress('Executing for real.')
}

return 'Done!';

tasks 42 


var schema = Magic.getAvroSchema('My cluster', 'some-avro-topic'); // reading existing schema

var newField = {
    name: 'MyNewField',
    type: 'int',
    doc: 'Some random number'
};
schema.fields.push(newField); // modifying schema by adding new field

var newSubject = 'new-topic'; // target topic

Magic.registerAvroSchema('My cluster', newSubject, schema); // registering schema for target topic

Magic.reportProgress('Registered new schema, trying to read it.');

var newSchema = Magic.getAvroSchema('My cluster', newSubject);

return newSchema; // displaying new schema in Results window


tasks 43 


@RestController
public class KafkaController {
    @Autowired
    private KafkaTemplate<String, String> kafkaTemplate;

    @PostMapping("/send")
    public void sendMessage(@RequestBody String message) {
        kafkaTemplate.send("my-topic", message);
    }
}

tasks 44 

producer.send(new ProducerRecord<>("my-topic", key, value), new Callback() {
    @Override
    public void onCompletion(RecordMetadata metadata, Exception exception) {
        if (exception != null) {
            // Обработка ошибок
            exception.printStackTrace();
        }
    }
});


tasks 45 

{
    "name": "jdbc-source",
    "config": {
        "connector.class": "io.confluent.connect.jdbc.JdbcSourceConnector",
        "tasks.max": "1",
        "connection.url": "jdbc:mysql://localhost:3306/mydb",
        "connection.user": "user",
        "connection.password": "password",
        "topic.prefix": "jdbc-",
        "mode": "incrementing",
        "incrementing.column.name": "id"
    }
}


tasks 46 

import org.apache.kafka.clients.consumer.ConsumerConfig;
import org.apache.kafka.clients.consumer.ConsumerRecord;
import org.apache.kafka.clients.consumer.KafkaConsumer;

import java.time.Duration;
import java.util.Collections;
import java.util.Properties;

public class SimpleConsumer {
    public static void main(String[] args) {
        Properties props = new Properties();
        props.put(ConsumerConfig.BOOTSTRAP_SERVERS_CONFIG, "localhost:9092");
        props.put(ConsumerConfig.GROUP_ID_CONFIG, "my-group");
        props.put(ConsumerConfig.KEY_DESERIALIZER_CLASS_CONFIG, "org.apache.kafka.common.serialization.StringDeserializer");
        props.put(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG, "org.apache.kafka.common.serialization.StringDeserializer");

        KafkaConsumer<String, String> consumer = new KafkaConsumer<>(props);
        consumer.subscribe(Collections.singletonList("my-topic"));

        while (true) {
            for (ConsumerRecord<String, String> record : consumer.poll(Duration.ofMillis(100))) {
                System.out.printf("Received message: key=%s value=%s%n", record.key(), record.value());
            }
        }
    }
}



tasks 47 


props.put("value.serializer", "io.confluent.kafka.serializers.KafkaAvroSerializer");
props.put("schema.registry.url", "http://localhost:8081");


tasks 48 


producer.send(new ProducerRecord<>("test-topic", key, value), (metadata, exception) -> {
    if (exception != null) {
        System.err.println("Error sending message: " + exception.getMessage());
    }
});

tasks 49 



public class MyCustomDeserializer implements Deserializer<MyObject> {
    @Override
    public MyObject deserialize(String topic, byte[] data) {
        // Deserialization logic
    }
}

tasks 50 

StreamsBuilder builder = new StreamsBuilder();
KStream<String, String> source = builder.stream("input-topic");
source.mapValues(value -> value.toUpperCase()).to("output-topic");
KafkaStreams streams = new KafkaStreams(builder.build(), props);
streams.start();
