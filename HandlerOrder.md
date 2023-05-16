# CAP Handler Order
We can specify some high level ordering for Handlers.

Generic handlers typically are executed by the framework before HandlerOrder.EARLY and after HandlerOrder.LATE:

- Generic framework handlers
- Custom handlers, annotated with HandlerOrder.EARLY
- Custom handlers for phases @Before, @On, and @After
- Custom handlers, annotated with HandlerOrder.LATE
- Generic framework handlers

````java
@After(event = CqnService.EVENT_READ, entity = Books_.CDS_NAME)
@HandlerOrder(HandlerOrder.EARLY)
public void firstHandler(EventContext context) {
    // This handler is executed first
}
````
## Reference
https://cap.cloud.sap/docs/java/query-api#bulk-insert
