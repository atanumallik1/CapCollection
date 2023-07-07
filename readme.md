# General CAP Collection

Also refer to : https://github.com/atanumallik1/CAP-Learning
- Cap Recording
  - https://recap-conf.dev/   

- Application Lifecycle Service
  - https://cap.cloud.sap/docs/java/consumption-api#application-lifecycle-service
  - Handling error messages using application lifecycle handler https://cap.cloud.sap/docs/java/indicating-errors#errorhandler
- Persistance Service APIs
  - https://cap.cloud.sap/docs/java/persistence-services#default-persistence-service  
## Working with CDS mdoel 
- https://cap.cloud.sap/docs/java/reflection-api#get-all-elements-with-given-annotation
## API 
- Stop a Service from exposure 
   -  `@cds.serve.ignore`
- dont create DB
   - `	@cds.persistence.skip`
   - 
## REst Protocol
- https://cap.cloud.sap/docs/node.js/cds-serve#protocol
- https://cap.cloud.sap/docs/advanced/troubleshooting#how-can-i-expose-custom-rest-apis-with-cap
## Open Types 
- https://cap.cloud.sap/docs/releases/apr23#open-types-in-odata-v4

## DB Interaction 
- Bulk API
  - https://cap.cloud.sap/docs/java/query-api#bulk-insert
  - https://cap.cloud.sap/docs/java/query-api?q=bulk+update#bulk-update 
- Row Level locking 
  - Pessimistic locking : https://cap.cloud.sap/docs/guides/providing-services/#pessimistic-locking
  - ````cds
        Select<Cart_> select = Select.from(Cart_.class)
        .columns(r -> r._all(), r -> r.LineItems()
            .expand(i -> i._all(), i -> i.AccountAssignments()
                    .expand(),
                i -> i.Comments()
                    .expand()
                    .orderBy(c -> c.modifiedAt()
                        .asc())))
        .where(row -> row.get(Cart.CREATED_BY)
            .eq(eventContext.getUserInfo()
                .getName()));
      select = select.lock();
      CqnSelect selectQuery = select.asSelect();
    ````  
