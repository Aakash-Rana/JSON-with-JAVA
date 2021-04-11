### JSon Parser
```
package packt.javaee.json;

import javax.json.*;
import javax.json.stream.JsonGenerator;
import javax.json.stream.JsonGeneratorFactory;
import javax.json.stream.JsonParser;
import javax.json.stream.JsonParserFactory;
import java.io.StringReader;
import java.io.StringWriter;
import java.util.Scanner;

/**
 * JSON with Java EE: Hands on Training.
 */
public
 class Main {

    public static void main(String[] args) {
        new Main().start();
    }

    private void start() {
        while (true) {
            printOptions();

            Scanner scanner = new Scanner(System.in);
            switch (scanner.nextLine()) {
                case "q":
                case "Q":
                    return;
                case "1":
                    jsonParserScenario();
                    break;
                case "2":
                    jsonGeneratorScenario();
                    break;
                case "3":
                    objectModelScenario();
            }
        }
    }

    private void printOptions() {
        System.out.println();
        System.out.println("----------------------- JSON with Java EE: Hand on Training -----------------------");
        System.out.println();
        System.out.println("Choose a scenario to run or press 'Q' to exit:");
        System.out.println();
        System.out.println("1. JSON parser");
        System.out.println("2. JSON generator");
        System.out.println("3. Create object model from file");

        System.out.println();
        System.out.println("-----------------------------------------------------------------------------------");
    }

    /**
     * Scenario 1: Using JsonParser (Video 2.3).
     *
     * Challenges:
     *
     * 1. Parse document and list all JSON-P events
     * 2. List all keys corresponding to events
     * 3. List all values
     */
    private void jsonParserScenario() {
        JsonParser parser = Json.createParser(Main.class.getResourceAsStream("/jasons.json"));

        //JsonParserFactory jsonParserFactory = Json.createParserFactory(null);
        //JsonParser parser = jsonParserFactory.createParser(Main.class.getResourceAsStream("/jasons.json"));

        while (parser.hasNext()) {
            JsonParser.Event event = parser.next();
            System.out.print(event.toString());  // KEY_NAME

            switch (event) {
                case KEY_NAME:
                case VALUE_STRING:
                case VALUE_NUMBER:
                    System.out.print(": " + parser.getString());
                    break;
                case VALUE_NULL:
                    System.out.print(": null");
                    break;
                case VALUE_TRUE:
                    System.out.print(": true");
                    break;
                case VALUE_FALSE:
                    System.out.print(": false");
            }

            System.out.println();
        }
    }

    /**
     * Scenario 2: Using JsonGenerator (Video 2.4).
     *
     * Challenge:
     *
     * Write to System.out a JSON document below using JsonGenerator
     *
     *   {
     *     "name": "Jason Bourne",
     *     "profession": "Super agent",
     *     "bad-guy": false,
     *     "kills": 1000,
     *     "phoneNumbers": [
     *       {
     *         "type": "home",
     *         "number": "123-456-789"
     *       }
     *     ]
     *   }
     */
    private void jsonGeneratorScenario() {
        //JsonGenerator generator = Json.createGenerator(System.out);

        JsonGeneratorFactory jsonGeneratorFactory = Json.createGeneratorFactory(null);
        JsonGenerator generator = jsonGeneratorFactory.createGenerator(System.out);

        generator.writeStartObject()
            .write("name", "Jason Bourne")
            .write("profession", "Super Agent")
            .write("bad-guy", false)
            .write("kills", 1000)
            .writeStartArray("phoneNumbers")
                .writeStartObject()
                    .write("type", "home")
                    .write("number", "123-456-789")
                .writeEnd()
            .writeEnd()
        .writeEnd()
        .flush();
    }

    /**
     * Scenario 3: Using Object Model API (Video 2.5).
     *
     * Challenges:
     *
     * 1. Create object model representing the following JSON document:
     *
     *   {
     *     "name": "Jason Bourne",
     *     "profession": "Super agent",
     *     "bad-guy": false,
     *     "kills": 1000,
     *     "phoneNumbers": [
     *       {
     *         "type": "home",
     *         "number": "123-456-789"
     *       }
     *     ]
     *   }
     *
     * 2. Write it to a stream.
     * 3. Create JsonReader and read JSON object model from a stream.
     * 4. Traverse the model and print all keys, key types and values
     */
    private void objectModelScenario() {
        // Create a object model
        JsonObject data = Json.createObjectBuilder()
                .add("name", "Jason Bourne")
                .add("profession", "Super Agent")
                .add("bad-guy", false)
                .add("kills", 1000)
                .add("phoneNumbers", Json.createArrayBuilder()
                    .add(Json.createObjectBuilder()
                        .add("type", "home")
                        .add("number", "123-456-789")))
                .build();

        // Write model to a stream
        StringWriter stringWriter = new StringWriter();
        JsonWriter jsonWriter = Json.createWriter(stringWriter);

        //JsonWriterFactory jsonWriterFactory = Json.createWriterFactory(null);
        //JsonWriter jsonWriter = jsonWriterFactory.createWriter(stringWriter);

        jsonWriter.writeObject(data);
        jsonWriter.close();

        String str = stringWriter.toString();
        System.out.println(str);

        // Create object model from a stream
        StringReader stringReader = new StringReader(str);
        JsonReader jsonReader = Json.createReader(stringReader);

        //JsonReaderFactory jsonReaderFactory = Json.createReaderFactory(null);
        //JsonReader jsonReader = jsonReaderFactory.createReader(stringReader);

        JsonObject obj = jsonReader.readObject();

        // Traverse model
        traverseModel(obj, null);
    }

    private void traverseModel(JsonValue value, String key) {
        System.out.print("Key: " + key + "; ");

        switch (value.getValueType()) {
            case ARRAY:
                System.out.println("Type: ARRAY");
                JsonArray array = (JsonArray) value;
                for (JsonValue item: array) {
                    traverseModel(item, null);
                }
                break;
            case OBJECT:
                System.out.println("Type: OBJECT");
                JsonObject obj = (JsonObject) value;
                for (String objKey: obj.keySet()) {
                    traverseModel(obj.get(objKey), objKey);
                }
                break;
            case STRING:
                JsonString jsonString = (JsonString) value;
                System.out.println("Type: STRING; Value: " + jsonString.getString());
                break;
            case NUMBER:
                JsonNumber jsonNumber = (JsonNumber) value;
                System.out.println("Type: NUMBER; Value: " + jsonNumber.toString());
                break;
            case TRUE:
            case FALSE:
            case NULL:
                System.out.println("Value: " + value.getValueType());
        }
    }
}