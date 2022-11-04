[Back to Syllabus](./README.md#course-syllabus)

Lab 2 overview
You need data to build a machine learning model. In Lab 1, a Python-based signal processing application was used to take audio files of cats and dogs to generate a CSV file that can be used to generate a machine learning model.

In this lab, you use the CSV file from Lab 1 to build a machine learning model that can predict which animal is making the sounds.

Be sure you complete the previous lab and install all prerequisite software.


1. Create a machine learning project
In this section, you create a machine learning project and use it as the container for your data assets and the machine models that you generate.

To complete this lab you will need an instance of Watson Machine Learning that allows you to create and deploy machine learning models, and an instance of Watson Studio which provides a user interface and wizards that guide you through the process of creating machine learning models.

If you created Watson Studio projects before, you might see different images than what is shown here.

Go to the IBM Watson page and click Log In to log in to IBM Watson Studio with your IBM ID and password. Then, scroll down to the Watson Services section.



Click Add service and then Add to select Machine Learning.



Verify that you have Watson Studio properly configured by opening your Studio profile.



Click the Apps tab. Then, check whether you have a Watson Studio application listed. If this is your first machine learning project, you will not see Watson Studio listed. If you don't have an application in the list, click Add other apps to add Watson Studio.



Go back to the Studio home page and click New Project. Select Create an Empty Project and give the project a name and description.

Create a Project screenshot

Make sure that the Storage field is populated with cloud-object-storage credentials. You might need to create IBM Cloud Object Storage and set your region appropriately for the credentials to appear on the Watson Studio project creation page.

You must use IBM Cloud Object Storage for reading input (such as training data) and for storing results, such as log files.

If your Storage field is showing as unpopulated, click Cloud Object Storage.



Click Add to add Cloud Object Storage for Watson Studio.



You might need to go back to the previous step to associate the cloud object storage with your data science project.

When you're done creating the project, click Create.



Optional: Skip the tutorial selections after you create the project.


[Go to Next Module](./4_Lab_3_Create_predictions_in_a_Node_RED_application.md)