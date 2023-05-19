# sagemaker-mlops-reference-architecture
## 1 Machine learning (ML) life cycle starts with Data, We create a separate account for all datastore. The data store can be S3, EFS, DynamoDB, RDS, Athena, Redshift. We use Glue / EMR for some data processing and store it back in a Data Account.

## 2 Security Account is mandatory for any enterprise scale usage, to Maintain User repository e.g) IAM Center (SSO), CloudTrail to log all the events and Alert if needed, Security Hub and Guarduty.
AWS Data lake for Unified data Governance. Data Lakes allow various roles in your organisation like data scientists, data developers, and business analysts to access data with their choice of analytic tools and frameworks. This includes open source frameworks such as Apache Hadoop, Presto, and Apache Spark, and commercial offerings from data warehouse and business intelligence vendors. Data Lakes allow you to run analytics without the need to move your data to a separate analytics system.
Shared Services Account to provisioned for all DevOps activities like provisioning the All the infrastructure shown in diagram using IaaC, Custom Image for Model. Codepipeline to deploy the Model to development and production environment using CICD.
Sagemaker Role Manager provides 3 preconfigured role personas and predefined permissions for 12 common ML activities.
Datascience Development account for Data scientists/ML Engineer - AWS Sagemaker Studio provides the Notebook for Developers using Python / RStudio to develop, test models locally. It is an integrated development environment (IDE) that provides a single web-based visual interface where you can access purpose-built tools to perform all machine learning (ML) development steps, from preparing data to building, training, and deploying your ML models, improving data science team productivity by up to 10x.
Once the Model is developed, Data Scientist / ML Engineer commit the Notebook / Python Code to Codecommit. You can store custom Artefacts in S3.
At the process of building models, Datascientist stores the features in Amazon SageMaker Feature Store, It is a fully managed, purpose-built repository to store, share, and manage features for machine learning (ML) models. Features are inputs to ML models used during training and inference. We can host this in a Machine learning Execution Account, where major ML compute operations run.
Once Model Commit to AWS Codecommit, We can invoke the Sagemaker Pipeline, Using Amazon SageMaker Pipelines, you can create ML workflows as DAG (Direct Acyclic Graph) with an easy-to-use Python SDK, and then visualise and manage your workflow using Amazon SageMaker Studio. You can be more efficient and scale faster by storing and reusing the workflow steps you create in SageMaker Pipelines. You can also get started quickly with built-in templates to build, test, register, and deploy models so you can get started with CI/CD in your ML environment quickly. During training in the pipeline, you can use pre pre-built container from ECR, or Custom Image stored in ECR in a Shared Services account.
Before Model Deployment, With the help of Amazon SageMaker Model Cards, you can create a single source of truth for model information by centralising and standardising documentation throughout the model lifecycle. You can record details such as purpose and performance goals while SageMaker Model Cards auto populates training details to accelerate the process. Once the models are deployed, Amazon SageMaker Model Dashboard gives you unified monitoring across all your models by providing deviations from expected behaviour, automated alerts, and troubleshooting to improve model performance.
We can store all the Model output in Sagemaker Registry. It helps to  catalogue models for production, Manage model versions, Associate metadata, such as training metrics, with a model, Manage the approval status of a model, Automate model deployment with CI/CD.
Using the Eventbridge, to trigger the Codepipeline to deploy the Model in Staging account.
Staging Account where we can host the staging environment of Amazon SageMaker Serverless Inference is a purpose-built inference option that makes it easy for you to deploy and scale ML models. Serverless Inference is ideal for workloads which have idle periods between traffic spurts and can tolerate cold starts. Serverless endpoints automatically launch compute resources and scale them in and out depending on traffic, eliminating the need to choose instance types or manage scaling policies. This takes away the undifferentiated heavy lifting of selecting and managing servers. Serverless Inference integrates with AWS Lambda to offer you high availability, built-in fault tolerance and automatic scaling. We received the payload from API Gateway. We also test Sagemaker Shadow (We see these details more in the Production environment).
Production Account has the same setup as Staging environment. Addition to that We use Sagemaker Shadow. Amazon SageMaker supports shadow testing to help you validate performance of new machine learning (ML) models by comparing them to production models. With shadow testing, you can spot potential configuration errors and performance issues before they impact end users. SageMaker eliminates weeks of time spent building infrastructure for shadow testing, so you can release models to production faster.
To improve the model quality, we store the inference data stored in S3 locally in Production account and replicate back to Data Account and also Machine learning Execution Account account.
Use Amazon SageMaker Model Monitor which allows you to create a set of baseline statistics and constraints using the data with which your model was trained, then set up a schedule to monitor the predictions made on your endpoint outputs.
We can initiate the retrain of Model by invoking the pipeline using Eventbridge based on Sagemaker Model output.
