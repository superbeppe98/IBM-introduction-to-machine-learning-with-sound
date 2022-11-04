[Back to Syllabus](./README.md#course-syllabus)

- Lab 2 overview
  - You need data to build a machine learning model. In Lab 1, a Python-based signal processing application was used to take audio files of cats and dogs to generate a CSV file that can be used to generate a machine learning model
  - In this lab, you use the CSV file from Lab 1 to build a machine learning model that can predict which animal is making the sounds
  - Be sure you complete the previous lab and install all prerequisite software

1. Create a machine learning project
  - In this section, you create a machine learning project and use it as the container for your data assets and the machine models that you generate
  - To complete this lab you will need an instance of Watson Machine Learning that allows you to create and deploy machine learning models, and an instance of Watson Studio which provides a user interface and wizards that guide you through the process of creating machine learning models
  - If you created Watson Studio projects before, you might see different images than what is shown here
  - Go to the IBM Watson page and click Log In to log in to IBM Watson Studio with your IBM ID and password. Then, scroll down to the Watson Services section
  - Click Add service and then Add to select Machine Learning
  - Verify that you have Watson Studio properly configured by opening your Studio profile
  - Click the Apps tab. Then, check whether you have a Watson Studio application listed. If this is your first machine learning project, you will not see Watson Studio listed. If you don't have an application in the list, click Add other apps to add Watson Studio
  - Go back to the Studio home page and click New Project. Select Create an Empty Project and give the project a name and description
  - Make sure that the Storage field is populated with cloud-object-storage credentials. You might need to create IBM Cloud Object Storage and set your region appropriately for the credentials to appear on the Watson Studio project creation page
  - You must use IBM Cloud Object Storage for reading input (such as training data) and for storing results, such as log files
  - If your Storage field is showing as unpopulated, click Cloud Object Storage
  - Click Add to add Cloud Object Storage for Watson Studio
  - You might need to go back to the previous step to associate the cloud object storage with your data science project
  - When you're done creating the project, click Create
  - Optional: Skip the tutorial selections after you create the project.

2. Create a machine learning model
   - In this section, you create a machine learning model. Models are created in the machine learning service and link back to data in the machine learning project
   - If you're not already logged in, log in to the IBM Watson home page. Then, scroll down to Recently updated projects and select the project that you created. Then, select Assets
   - Click Add to project and select the AutoAI Experiment asset type.
   - Name the experiment and optionally provide a description. Make sure the Experiment Type is set to From Blank
   - On the right-hand side, click Associate a Machine Learning service instance
   - You can now create a new service or use an existing one
   - If you do not have a service, select the appropriate plan and click Create. If you already have a service, click Existing and select it from the list
   - On the Watson Studio page, click Reload and leave the compute configuration to default
   - The service should then get populated. Click Create
   - You are then asked to add a data source. Click Browse and find the cats and dogs CSV file from the birdsongs directly in the Git Repository you cloned in Lab 1. If you are using MacOS, you will need the soundmac.csv file, for Windows you need the sound.csv file.
   - You are asked which columns you want to predict. If you open the spreadsheet, notice that the columns are all numbered COLUMN1, COLUMN2 and so on. COLUMN1 specifies whether the audio file was a cat or a dog, which is what you want to predict in the model
   - Select COLUMN1 from the Column value to predict menu. 
   - The prediction type is automatically set as Binary Classification. This is correct as the model being build returns cat or dog only
   - At this point, the configuration is complete. To view the experiment parameters, click Experiment Settings. On this page you configure settings such as the test, train, split percentage as well as selecting the algorithms the experiment uses. For the purposes of this lab, click Cancel as the default settings meet the requirements
   - Explanation of terminology
        - Training Data Split: where a set amount of data is set aside to validate the experiment e.g. 60% 20% 20% where 60% is used to train the model and 20% is used to test the performance of the model and 20% as a holdout check of the better performing models to detect overfitting
        - Overfitting: where a model performs very well on its training and test data but performs poorly on holdout data
    - Click Run Experiment. The models will now be trained. This could take a considerable amount of time
    - This part of the screen shows the progress of the training
    - This section of the page shows the models trained in order of the best performing model first. In this example the Gradient Boosting Classifier with 3 enhancements was the best model.
    - Hover the mouse over the best performing model until it turns blue and click Save As and Model
    - Now you have a model that will predict a value of cat or dog from an audio file. To be able to use this in a Node-RED application (or as an API for a different application), you must deploy it

3. Deploy the machine learning model
   - In this section, you deploy the new machine learning model so that applications can use it to make predictions
   - Go back to your project overview by clicking the project name in the breadcrumb. Scroll down to the Models section
   - Click on the burger icon under Actions and select Deploy
   - On the right side of the page, click Add Deployment
   - Give the deployment a name (and optional description), and then click Save
   - Wait for the model to deploy. The status will show a success status when it is complete. If it fails to deploy, try refreshing the browser after a few minutes. Alternatively, delete that deployment and follow these steps again
   - Click the name of the deployment. On this page, note the deployment name, which you will need later to call the predictor in your Node-RED application
   - The Implementation tab shows you the scoring endpoint and the sample source code
   - Next, you need to find and record your machine learning credentials in IBM Cloud
   - Click the navigation menu icon and select Services > Watson Service
   - In the list of services, click your machine learning service and then click Service credentials
   - Make a note of the service credentials because you will need them in the next lab

[Go to Next Module](./5_Lab_4_Create_multiclass_classification_models.md)