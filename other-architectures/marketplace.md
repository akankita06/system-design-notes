### Resources

- code karle video
- [shopify architecture video](https://www.youtube.com/watch?v=lEL4F_0J3l8)
- [amazon architecture](https://www.codekarle.com/system-design/Amazon-system-design.html)

&nbsp;

### requirements
    - **functional**
        - search
        - product display page
        - payments
        - orders
        - cart
    - **non functional**
        - low latency
        - high availability
        - high consistency - for some components
        &nbsp;
        
### data stores
    - items -  documentDB like mongoDB
        - why - each item has its own attribute + need to be stored in queryable format.
            - eg: shirt - has attributes color, size, length, material, etc.
            - all these attributes should be queryable so during search people can filter based on these attributes
        - kv store - not good, when queryable attributes. kv store works when values could be anything and need not be queryable.
        &nbsp;
        
![marketplace](https://github.com/akankita06/system-design-notes/blob/main/images/marketplace.png)
        
