# Complex CQNs

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
