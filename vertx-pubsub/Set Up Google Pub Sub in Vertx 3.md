# Set Up Google Pub Sub in Vert-x3

Here are the quick steps to set up Google Pub Sub in vert-x3

## Create Vertx Application

Download vertx starter template from [this](http://start.vertx.io/) link as shown in the image below

![](https://raw.githubusercontent.com/ethirajsrinivasan/blogs/master/vertx-pubsub/vert-x3%20starter%20template.png)

## Main Verticle

In the main verticle get an instance of router and create a new endpoint to be handled by a vertx handler eg MessageHandler.
To leverage the benefits of the vertx application code written has to be non blocking. Blocking or Time Consuming code can be executed with the help of worker verticle.So we will deploy the Google Pub Sub Message Processor as a worker verticle. Create new `DeploymentOptions` and set the worker as true. Configuration options can also be provided to the deploymentOptions. Configuration will be in the form of a json which can provided by local Json Object or the Json Object obtained from the application configuration. Configuration for Google Pub Sub may contain the topic that you are subscribing to , the path of the Google Pub Sub Credentials etc. Google credentails can be obtained by following the steps given [here](https://cloud.google.com/docs/authentication/getting-started). Place the google credentails file within the config folder in the src directory. Create a simple HTTPServer with router accepting the request.


```java
public class MainVerticle extends AbstractVerticle {

  private JsonObject appConfig;

  @Override
  public void start() throws Exception {

    // get application config
    appConfig = config();

    // get router and set the endpoints
    Router router = Router.router(vertx);
    router.get("/sendpubsubmessage").handler(new MessageHandler(vertx));

    // set config for worker verticle
    JsonObject pubsubConfig = new JsonObject();
    pubsubConfig.put("topic", "your_topic");
    pubsubConfig.put("credentialsPath","path/to/credentials.json");

    // deploy worker verticle
    DeploymentOptions pubsubOptions =  new DeploymentOptions().setWorker(true).setConfig(pubsubConfig);
      vertx.deployVerticle("com.example.demo.PubSubMessageProcessor", pubsubOptions);

    // start http server
    vertx.createHttpServer().requestHandler(req -> {
        try {
            router.accept(req);
        } catch(Throwable th) {
            req.response().setStatusCode(400).end(th.getMessage());
        }
    }).listen(8080);

  }
}
```

## Message Handler

create a simple message handler. Get the message to be sent to google pub sub from the parameter in the request and send it as json object in the eventbus. The `send` method takes the address and message as the arguments. Address can be simple string

```java
public class MessageHandler implements Handler<RoutingContext> {

  private Vertx vertx;
  
  // constructor
  public MessageHandler(Vertx vertx) {
    this.vertx = vertx;
  }

  // send message to verticle via eventbus
  @Override
  public void handle(RoutingContext event) {
    JsonObject message = new JsonObject();
    String value = event.request().params().get("message");
    message.put("key", value);
    vertx.eventBus().send("pubsub", message);
  }

}
```

## Add Dependency

Add the following maven dependency to the pom.xml file

```java
<dependency>
  <groupId>com.google.protobuf</groupId>
  <artifactId>protobuf-java</artifactId>
  <version>3.5.1</version>
</dependency>

<dependency>
  <groupId>com.google.cloud</groupId>
  <artifactId>google-cloud-pubsub</artifactId>
  <version>1.31.0</version>
</dependency>
```

## PubSub Message Processor

Configure the  pubslisher client for Google Pub Sub with the credentails and build it. Register the event bus with same address given in the message handler.

```java
public class PubSubMessageProcessor extends AbstractVerticle {


    private JsonObject pubsubConfig;
    private Publisher client;

    @Override
    public void start() throws Exception
    {
        pubsubConfig = config();
        client = createClient();
        
        // register handler
        EventBus eb = vertx.eventBus();
        eb.consumer("pubsub", jsonObjectMessage -> {
            sendMessageToPubSub(jsonObjectMessage);
        });
    }

    private void sendMessageToPubSub(Message<Object> jsonObjectMessage) {
        ByteString data = ByteString.copyFromUtf8(jsonObjectMessage.body().toString());
        PubsubMessage pubsubMessage = PubsubMessage.newBuilder().setData(data).build();
        client.publish(pubsubMessage);

    }

    // get publisher client
    private  Publisher createClient() throws Exception  {
        GoogleCredentials credentials  = GoogleCredentials.fromStream(new FileInputStream(pubsubConfig.getString("credentialsPath")));
        return Publisher.newBuilder(pubsubConfig.getString("topic")).setCredentialsProvider(FixedCredentialsProvider.create(credentials))
          .build();

    }

}
```

Google Pub Sub with Vertx Application is now configured

## Compilation and Start Server

```sh
mvn clean compile package
java -jar target/demo-1.0.0-SNAPSHOT-fat.jar com.example.demo.MainVerticle
```

Full Code can be found [here](https://github.com/ethirajsrinivasan/vertx-pubsub)
