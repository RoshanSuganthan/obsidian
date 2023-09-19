- ### Ensure The ML Model Is Necessary
- ### **Collect Data For The Chosen Objective**
- ### Develop Simple & Scalable Metrics
	- Technical and business metrics have to be developed based on the use cases.

## Infrastructure Best Practices
- The best practice is to create an **encapsulated** **ML** **model** that is self-sufficient. The infrastructure should not be dependent on the **ML** **model**. 
- This allows the building of multiple features later on. Testing and sanity checks on models are required before deployment.

- ### **Right Infrastructure Components**
- Multiple aspects such as containers, orchestration tools, hybrid environments, multi-cloud environments, and agile architecture must be implemented stepwise, allowing maximum scalability.
- ### **Cloud-based vs. On-premise Infrastructure**

- ### **Make The Infrastructure Scalable**
-  Infrastructure should support separate training models and serving models. This enables you to continue testing your model with advanced features without affecting the deployed serving model. Microservices architecture is instrumental in achieving encapsulated models.

## Data Best Practices
### **Understand Data Quantity Significance**
 - When the data availability is minimal, you can use transfer learning to gather as much data as possible. 
 - Once raw data is available, you must deploy feature engineering to pre-process the data. Collected data must undergo necessary transformations to be valuable as training data. Raw inputs converted into features will be helpful in the design phase of the **ML [data modeling](https://www.zucisystems.com/blog/what-is-data-modeling-and-why-is-it-important/).
 - ### **Data Processing Is Crucial**
 - 