# Adding before / After hander to persistence service 

Adding a `before` handler to Persistence service.

````java

@Component
@ServiceName(value = "*", type = PersistenceService.class)
public class NGPManagedHander implements EventHandler {

@Before(event = { CqnService.EVENT_CREATE, CqnService.EVENT_UPDATE, CqnService.EVENT_UPSERT })
  @HandlerOrder(HandlerOrder.LATE)
  public void calculateManagedFields(EventContext context) {
  
       List<CdsElement> elements = context.getTarget()
        .elements()
        .collect(Collectors.toList());
    String userUUID = context.getUserInfo()
        .getName();
    List<Map<String, Object>> entries = getEntries(context);
    entries.forEach(entry -> {
      elements.forEach(element -> {
        /*
         * if createdBy or ModifiedBy is anonymous then it is a anonymization
         * flow, else it is a normal flow.
         */
        if ((entry.get(NGPManaged.CREATED_BY) != null && entry.get(NGPManaged.CREATED_BY)
            .toString()
            .equals(NonTranslationRelatedConstants.ANONYMOUS))
            || (entry.get(NGPManaged.MODIFIED_BY) != null && entry.get(NGPManaged.MODIFIED_BY)
                .toString()
                .equals(NonTranslationRelatedConstants.ANONYMOUS))) {
          processAnonymizationFlow(context, entry, element);
        } else {
          processStandardFlow(context, entry, element, userUUID);
        }
      });
    });
  
  
  }

}
````
