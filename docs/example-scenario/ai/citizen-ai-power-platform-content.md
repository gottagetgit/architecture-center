The following architecture extends the [analytics end-to-end with Azure Synapse Analytics](../dataplate2e/data-platform-end-to-end.yml) scenario. It allows for a custom machine learning (ML) model to be trained in Azure Machine Learning and implemented with a custom application built by using Microsoft Power Platform.

## Architecture

:::image type="content" source="media/citizen-ai-power-platform.png" alt-text="Diagram that shows the architecture for citizen AI with Microsoft Power Platform." lightbox="media/citizen-ai-power-platform.png" :::

Download a [Visio file](https://arch-center.azureedge.net/citizen-ai-power-platform.vsdx) of this architecture.

### Workflow

The workflow consists of the following steps:

- Ingest
- Store
- Train and deploy a model
- Consume

#### Ingest
Use [Azure Synapse Pipelines](/azure/data-factory/concepts-pipelines-activities) to pull batch data from various sources, both on-premises and in the cloud. This lambda architecture has two data ingestion flows: streaming and batch. They're described here:

- **Streaming**: In the upper half of the preceding architecture diagram are the streaming data flows (for example, big data streams and IoT devices).
  - You can use [Azure Event Hubs or Azure IoT Hub](/azure/iot-hub/iot-hub-compare-event-hubs) to ingest data streams generated by client applications or IoT devices. Event Hubs or IoT Hub ingest and store streaming data, preserving the sequence of events received. Consumers can connect to hub endpoints to retrieve messages for processing.
- **Batch**: In the lower half of the architecture diagram, data is ingested and processed in batches such as:
  - Unstructured data (for example, video, images, audio, and free text)
  - Semi-structured data (for example, JSON, XML, CSV, and logs)
  - Structured data (for example, relational databases and Azure Data Services)
  
    [Azure Synapse Link](/azure/cosmos-db/synapse-link) creates a tight seamless integration between Azure Cosmos DB and Azure Synapse Analytics.
    [Azure Synapse Pipelines](/azure/data-factory/concepts-pipelines-activities?tabs=data-factory) can be triggered based on a predefined schedule or in response to an event. They can also be invoked by calling REST APIs.

#### Store
Ingested data can land directly in raw format and then be transformed on the [Azure Data Lake](/azure/storage/blobs/data-lake-storage-introduction). Data once curated and transformed to relational structures can be presented for consumption in [Azure Synapse Analytics](https://azure.microsoft.com/services/synapse-analytics).

#### Train and deploy a model
[Machine Learning](https://azure.microsoft.com/services/machine-learning) provides an enterprise-grade ML service for building and deploying models faster. It provides users at all skill levels with a low-code designer, automated ML, and a hosted Jupyter notebook environment. Models can be deployed either as real-time endpoints on [Azure Kubernetes Service or as a Machine Learning managed endpoint](/azure/machine-learning/concept-endpoints). For batch inferencing of ML models, you can use [Machine Learning pipelines](/azure/machine-learning/concept-ml-pipelines).

#### Consume
A batch or real-time model published in Machine Learning can generate a REST endpoint that can be consumed in a [custom application built by using the low-code Power Apps platform](/connectors/custom-connectors/use-custom-connector-powerapps). You can also call a [real-time Machine Learning endpoint from a Power BI report](/power-bi/connect-data/service-aml-integrate) to present predictions in business reports.

> [!NOTE]
> Both Machine Learning and Power Platform stack have a range of built-in connectors to help ingest data directly. These connectors might be useful for a one-off minimum viable product (MVP). However, the "Ingest" and "Store" sections of the architecture advise on the role of standardized data pipelines for the sourcing and storage of data from different sources at scale. These patterns are typically implemented and maintained by the enterprise data platform teams.

### Components

You can use the following components.

#### Power Platform services

- [Power Platform](https://powerplatform.microsoft.com): A set of tools for analyzing data, building solutions, automating processes, and creating virtual agents. It includes Power Apps, Power Automate, Power BI, and Power Virtual Agents.
- [Power Apps](https://powerapps.microsoft.com): A suite of apps, services, connectors, and data platform. It provides a rapid application development environment to build custom apps for your business needs.
- [Power Automate](https://flow.microsoft.com): A service that helps you create automated workflows between your favorite apps and services. Use it to synchronize files, get notifications, collect data, and so on.
- [Power BI](https://powerbi.microsoft.com): A collection of software services, apps, and connectors that work together to turn your unrelated sources of data into coherent, visually immersive, and interactive insights.

#### Azure services

- [Machine Learning](https://azure.microsoft.com/services/machine-learning): An enterprise-grade ML service for building and deploying models quickly. It provides users at all skill levels with a low-code designer, automated ML, and a hosted Jupyter notebook environment to support your own preferred IDE of choice.
- [Machine Learning managed endpoints](/azure/machine-learning/how-to-deploy-managed-online-endpoints): Online endpoints that enable you to deploy your model without having to create and manage the underlying infrastructure.
- [Azure Kubernetes Service](/azure/machine-learning/how-to-create-attach-kubernetes?tabs=python): ML has varying support across different compute targets. Azure Kubernetes Service is one such target, which is a great fit for enterprise grade real-time model endpoints.
- [Azure Data Lake](/azure/machine-learning/concept-data): A Hadoop-compatible file system. It has an integrated hierarchical namespace and the massive scale and economy of Azure Blob Storage.
- [Azure Synapse Analytics](https://azure.microsoft.com/services/synapse-analytics): A limitless analytics service that brings together data integration, enterprise data warehousing, and big data analytics.
- [Event Hubs](https://azure.microsoft.com/services/event-hubs) and [IoT Hub](https://azure.microsoft.com/services/iot-hub): Both services ingest data streams generated by client applications or IoT devices. They then ingest and store streaming data, preserving the sequence of events received. Consumers can connect to the hub endpoints to retrieve messages for processing.

#### Platform services

To improve the quality of your Azure solutions, follow the recommendations and guidelines in the [Azure Well-Architected Framework](/azure/architecture/framework). The framework consists of five pillars of architectural excellence:

- Cost optimization
- Operational excellence
- Performance efficiency
- Reliability
- Security

To create a design that respects these recommendations, consider the following services:

- [Microsoft Entra ID](https://azure.microsoft.com/services/active-directory): Identity services, single sign-on, and multifactor authentication across Azure workloads.
- [Azure Cost Management and Billing](https://azure.microsoft.com/services/cost-management): Financial governance over your Azure workloads.
- [Azure Key Vault](https://azure.microsoft.com/services/key-vault): Secure credential and certificate management.
- [Azure Monitor](https://azure.microsoft.com/services/monitor): Collection, analysis, and display of telemetry from your Azure resources. Use Monitor to proactively identify problems to maximize performance and reliability.
- [Microsoft Defender for Cloud](https://azure.microsoft.com/services/security-center): Strengthen and monitor the security posture of your Azure workloads.
- [Azure DevOps](https://azure.microsoft.com/solutions/devops) & [GitHub](https://azure.microsoft.com/products/github): Implement DevOps practices to enforce automation and compliance of your workload development and deployment pipelines for Azure Synapse Analytics and Machine Learning.
- [Azure Policy](/azure/governance/policy): Implement organizational standards and governance for resource consistency, regulatory compliance, security, cost, and management.

### Alternatives

A machine learning MVP benefits from speed to outcome. In some cases, the needs of a custom model can be met by pretrained [Azure Cognitive Services](https://azure.microsoft.com/services/cognitive-services) or [Azure Applied AI Services](https://azure.microsoft.com/product-categories/applied-ai-services). In other cases, [Power Apps AI Builder](https://powerapps.microsoft.com/ai-builder) might provide a fit for a purpose model.

## Scenario details

A general technology trend is the growing popularity of citizen AI roles. Such roles are business practitioners looking to improve business processes through the application of ML and AI technologies. A significant contributor to this trend is the growing maturity and availability of low-code tools to develop ML models.

Because of a well-known high failure rate to such initiatives, the ability to rapidly prototype and validate an AI application in a real-world setting becomes a key enabler to a fail-fast approach. There are two key tools for developing models that modernize processes and drive transformative outcomes:

- **An ML toolkit for all skill levels**
  - Supports no-code to fully coded ML development
  - Has a flexible, low-code GUI
  - Enables users to rapidly source and prep data
  - Enables users to rapidly build and deploy models
  - Has advanced, automated ML capabilities for ML algorithm development
- **A low-code application development toolkit**
  - Enables users to build custom applications and automation workflows
  - Creates workflows so that consumers and business processes can interact with an ML model

[Machine Learning](https://azure.microsoft.com/services/machine-learning) fulfills the role of a low-code GUI for ML development. It has automated ML and deployment to batch or real-time endpoints. [Power Platform](https://powerplatform.microsoft.com), which includes [Power Apps](https://powerapps.microsoft.com) and [Power Automate](https://flow.microsoft.com), provides the toolkits to rapidly build a custom application and workflow that implements your ML algorithm. Business users can now build production-grade ML applications to transform legacy business processes.

### Potential use cases

These toolkits minimize the time and effort needed to prototype the benefits of an ML model on a business process. You can easily extend a prototype to a production-grade application. The uses for these techniques include:

- **Manufacturing Ops with legacy applications that use outdated deterministic predictions.** Such situations can benefit from the improved accuracy of an ML model. Proving improved accuracy requires both a model and development effort to integrate with legacy systems on-premises.
- **Call Center Ops with legacy applications that don't adjust when** [data drifts](/azure/machine-learning/how-to-monitor-datasets?tabs=python). Models that automatically retrain might provide a significant uplift in churn prediction or risk profiling accuracy. Validation requires integration with existing customer relationship management and ticket management systems. Integration could be expensive.

## Considerations

When you use these services to create a proof of concept or MVP, you're not finished. There's more work to make a production solution. Frameworks such as the [Well-Architected Framework](/azure/architecture/framework) provide reference guidance and best practices to apply to your architecture.

### Availability

Most of the components used in this example scenario are managed services that scale automatically. The [availability of the services](https://azure.microsoft.com/global-infrastructure/services/?products=machine-learning-service%2Cvirtual-machines&regions=all) used in this example varies by region.

Apps based on ML typically require one set of resources for training and another for serving. Resources required for training generally don't need high availability, as live production requests don't directly hit these resources. Resources required for serving requests need high availability.

### DevOps

DevOps practices are used to orchestrate the end-to-end approach used in this example. If your organization is new to DevOps, the [DevOps Checklist](/azure/architecture/checklist/dev-ops) can help you get started.

The [Machine Learning DevOps Guide](/azure/cloud-adoption-framework/ready/azure-best-practices/ai-machine-learning-mlops#machine-learning-devops-mlops-best-practices-with-azure-machine-learning) presents best practices and learnings on adopting ML operations (MLOps) in the enterprise with Machine Learning.

DevOps automation can be applied to the Power Platform solution provided in this example. For more information about Power Platform DevOps, see [Power Platform Build Tools for Azure DevOps: Power Platform](/power-platform/alm/devops-build-tools).

### Cost optimization

Cost optimization is about looking at ways to reduce unnecessary expenses and improve operational efficiencies. For more information, see [Overview of the cost-optimization pillar](/azure/architecture/framework/cost/overview).

**Azure pricing:** First-party infrastructure as a service (IaaS) and platform as a service (PaaS) services on Azure use a consumption-based pricing model. They don't require a license or subscription fee. In general, use the [Azure pricing calculator](https://azure.microsoft.com/pricing/calculator) to estimate costs. For other considerations, see [Cost optimization](/azure/architecture/framework/cost/index) in the Well-Architected Framework.

**Power Platform pricing:** [Power Apps](https://powerapps.microsoft.com/pricing), [Power Automate](https://flow.microsoft.com/pricing) and [Power BI](https://powerbi.microsoft.com/pricing) are SaaS applications and have their own pricing models, including per app plan and per user.

## Deploy this scenario

Consider this business scenario. A field agent uses an app that estimates a car's market price. You can use Machine Learning to quickly prototype an ML model of this app. You use a low-code designer and ML features to create the model, and then deploy it as a real-time REST endpoint.

The model might prove the concept, but a user has no easy way to consume a model implemented as a REST API. Power Platform can help close this last mile, as represented here.

:::image type="content" source="media/citizen-ai-power-platform-rest-ui.png" alt-text="Screenshot that shows an ML model that's created in Machine Learning. The model obtains car data from Azure Data Lake, and it provides inferences to an endpoint." lightbox="media/citizen-ai-power-platform-rest-ui.png" :::

Here's a user interface for the app, created in Power Apps by using the low-code interface that Power Apps provides.

:::image type="content" source="media/citizen-ai-power-platform-car-price.png" alt-text="Screenshot that shows buttons and dropdown lists for the user to enter car data. The app predicts a price and displays it when the user selects the Predict button." lightbox="media/citizen-ai-power-platform-car-price.png" :::

You can use Power Automate to build a low-code workflow to parse the user's input, pass it to the Machine Learning endpoint, and retrieve the prediction. You can also use [Power BI to interact with the Machine Learning model](/power-bi/connect-data/service-aml-integrate) and create custom business reports and dashboards.

:::image type="content" source="media/citizen-ai-power-platform-dashboard.png" alt-text="Diagram that shows architecture showing the schematic of the workflow." lightbox="media/citizen-ai-power-platform-dashboard.png" :::

To deploy this end-to-end example, follow [step by step instructions by using this sample Power App](https://github.com/Azure/carprice-aml-powerapp).

## Extended scenarios

Consider the following scenarios.

### Deploy to Teams

The sample app provided in the preceding example can also be deployed to Microsoft Teams. Teams provides a great distribution channel for your apps and provides your users with a collaborative app experience. For more information about how to deploy an app to Teams by using Power Apps, see [Publish your app by using Power Apps in Teams: Power Apps](/powerapps/teams/publish-and-share-apps).

### Consume the API from multiple apps and automations

In this example, we configure a Power Automate cloud flow to consume the REST endpoint as an HTTP action. We can instead set up a custom connector for the REST endpoint and consume it directly from Power Apps or from Power Automate. This approach is useful when we want multiple apps to consume the same endpoint. It also provides governance by using the connector data loss prevention (DLP) policy in the Power Platform admin center. To create a custom connector, see [Use a custom connector from a Power Apps app](/connectors/custom-connectors/use-custom-connector-powerapps). For more information on the Power Platform connector DLP, see [Data loss prevention policies: Power Platform](/power-platform/admin/wp-data-loss-prevention).

## Contributors

This article is maintained by Microsoft. It was originally written by:

* [Vyas Dev Venugopalan](https://au.linkedin.com/in/vyasvenugopalan) | Sr. Specialist - Azure Data & AI

## Next steps

- [How Machine Learning works: Architecture and concepts](/azure/machine-learning/concept-azure-machine-learning-architecture)
- [Build intelligent applications infused with world-class AI](https://mybuild.microsoft.com/sessions/2ba55238-d398-46f9-9ff2-eafcd9d69df3)

## Related resources

- [Analytics end-to-end with Azure Synapse Analytics](../dataplate2e/data-platform-end-to-end.yml)
- [End-to-end manufacturing using computer vision on the edge](../../reference-architectures/ai/end-to-end-smart-factory.yml)
- [Artificial intelligence (AI)](/azure/architecture/data-guide/big-data/ai-overview)
- [Compare the ML products and technologies from Microsoft](../../ai-ml/guide/data-science-and-machine-learning.md)
- [Machine learning operations (MLOps) framework to upscale ML lifecycle with Machine Learning](../mlops/mlops-technical-paper.yml)
