# Complex CQNs


## Common Aspects 
- https://cap.cloud.sap/docs/cds/common
- Curreny, Uom, Code List


## Build in type , Common Types
- https://cap.cloud.sap/docs/cds/types
- https://cap.cloud.sap/docs/cds/common
- Mixins 


### What is CQL 
- https://cap.cloud.sap/docs/cds/cql
- CDS Query Language (CQL) is based on standard SQL
- Query builder API allows us to build CQL (https://cap.cloud.sap/docs/java/query-api) , these APIs are like fluent API 
- These are easy APIs, and finally they generate CQN which is executed in Run method
````
CqnService service = ...

CqnSelect query = Select.from("bookshop.Books")
    .columns("title", "price");

Result result = service.run(query);
````

### What is CQN 
- https://cap.cloud.sap/docs/cds/cqn
- CQN is a canonical plain object representation of CDS queries. Such query objects can be obtained by parsing CQL, by using the query builder APIs, or by simply constructing respective objects directly in your code.
- a CQN is executed in the RUN method 
````
let query = {SELECT:{from:[{ref:['Foo']}]}}
cds.run (query)
````



## Expand
````
   Select<Cart_> select = Select.from(Cart_.class)
        .columns(r -> r._all(), r -> r.LineItems()
            .expand(i -> i._all(), i -> i.AccountAssignments()
                .expand()))
        .where(row -> row.get(Cart.CREATED_BY)
            .eq(createdBy));
````

````
Select<Requisitions_> requisitions = Select.from(Requisitions_.class)
        .columns(r -> r._all(), r -> r.LineItems()
            .expand(i -> i._all(), i -> i.AccountAssignments()
                    .expand(),
                i -> i.Comments()
                    .expand()
                    .orderBy(c -> c.modifiedAt()
                        .asc())))
        .where(row -> row.ReqId()
            .eq(reqId));
````
