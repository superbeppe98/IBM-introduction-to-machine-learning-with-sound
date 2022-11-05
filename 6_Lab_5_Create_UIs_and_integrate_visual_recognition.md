[Back to Syllabus](./README.md#course-syllabus)

- Lab 5 overview
    - By now, you should have created a machine learning model for the dog and cat audio files and several models for the birdsong audio files. You also should have created two Node-RED flows to be able to pass in audio files and make  predictions
    - In this lab, you will create two simple HTML-based user interfaces:
        - One for the birdsong machine learning model
        - One for the dogs and cats machine learning model
    - The dogs and cats page will also include the Watson Visual Recognition service, which allows you to upload an image of a dog or cat and make a prediction. The IBM Watson™ Visual Recognition service uses deep learning algorithms to analyze images for scenes, objects, faces, and other content. The response includes keywords that provide information about the content
    - Complete all previous labs. You should already have an instance of Node-RED running with the two machine learning models created

1. Create the interface for the dog and cat audio
   - In this section, you create a simple HTML web page where you can select an audio file from your system and make a prediction
   - Open the Node-RED instance that you created in Lab 3 of this course. The flow should look like this:
   - Add an http input node, an http output node, and three template nodes. Connect them so that they look like this:
   - This part of the flow creates an HTTP GET request
   - Double-click the http input node and add a suitable URL ending in the URL field. This creates the web page URL
   - For example, adding a URL ending of ```/dogs_cats``` creates a full URL of ```https://< myNodeREDinstance>.au-syd.mybluemix.net/dogs_cats```
   - Click Done
   - For the third template node, make these changes. When you're finished, click Done
   - Set the Name field to HTML
   - Set Syntax Highlight to HTML. The Syntax Highlight box helps to color code the tags and other items for ease of use
   - Go to the animal-sounds repository and copy the HTML code for Section 1 into the Template field
   - This code creates a simple HTML page with a title, subtitle, an input field to select a file from your system, and a button to process the audio file. There is also space for the prediction result to show on the page
   - The HTML page includes placeholders to the CSS and JavaScript code that you will create in the next few steps
   - CSS placeholder:
       - ```{{{payload.css}}}```
   - JavaScript placeholder:
       - ```{{{payload.script}}}```
   - The node settings should look like this:
   - For the second template node, make these changes. When you're finished, click Done
   - Set the name to JavaScript
   - Set the property to msg.payload.script. This setting places the template contents on msg.payload.script that enables the HTML template reference to find and inject it at its placeholder. If you look at the HTML that you just pasted, at the bottom of the file is the following line:
       - ```{<script>{{{payload.script}}}</script>```
   - This is how the HTML file gets the JavaScript
   - Set the Syntax Highlight field to JavaScript
   - Copy and paste the contents of the Section_1_JavaScript.js file from GitHub in the Template field
   - This code checks that the file that is uploaded by the user is an audio file, passes it to an API called performAudioReco, which you will create later in this lab, and returns the result by creating a new ```<div>``` element and inserting a table with the classification and score
   - For the first template node, make these changes. When you're finished, click Done
   - Set the name to CSS
   - Set Syntax Highlight field to CSS
   - Set the property to msg.payload.css
   - Delete the text in the Template field
   - The user interface used in this course is simple. Leave this file empty for now, but If you want to add your own styling to the interface, you can insert your CSS in this template node
   - You should now have a small flow that looks like this:
   - Deploy the application and navigate to the URL that you provided. The last part of the URL is what you specified as the URL in the http input node, for example:
       - ```{https://catsdogs.au-syd.mybluemix.net/dogs_cats```{
   - The web page should look like this:
   - At the moment, the Process Audio button won’t make a prediction because you haven’t created the HTTP request endpoint, which is highlighted in the JavaScript code description where the code calls performAudioReco
   - Add another http input node, an http output node, and a function node and wire them together. Place the new nodes between the nodes that you just added and the microphone and base64 nodes
   - Edit the http input node and change the method to POST, and in the URL field, enter /performAudioReco. Select the Accept File Uploads checkbox and then click Done
   - Edit the function node and name it Locate Audio Buffer. Change the number of outputs to 2 at the bottom and paste in the following code. When you're finished, click Done

       - ```{if (msg.req && msg.req.files && ```
            ```Array.isArray(msg.req.files) && ```
            ```msg.req.files[0].buffer) {```
            ```msg.payload = msg.req.files[0].buffer;```
            ```return [null, msg];```
            ```} else {```
            ```  msg.payload = {'error' : 'No File Received'};```
            ``` return [msg, null];  ```
        ```}```

    - This code checks that a file has been passed in from the web page. If no file has been passed, it will throw an error
    - The first output node should go to the http node that you just added
    - Join the second output node to the base64 node
    - This part of the flow passes the audio file that was uploaded in the interface through the flow that you created in Lab 3 to run the machine learning model
    - Add a Play Audio node after the Locate Audio Buffer node so that the sound plays after the user uploads it
    - If you don’t have this node already, click Options > Manage Palette > Install and search for node-red-contrib-play-audio
    - Wire the output node from the Just the results node to the http out node. You can also do this by using link nodes
    - Deploy the application and switch back to your web page
    - You should now be able to select an audio file from your file system and run a prediction by clicking Process Audio. The audio file should play, and the prediction will be returned in a table

2. Add the Visual Recognition service for the dog and cat interface
   - In this section, you will add the Watson Visual Recognition service to your instance of Node-RED to do image and audio analysis from the same web page
   - Select all of the nodes below the user interface line
   - Use the down arrow to create a gap between the nodes.
   - The Visual Recognition component of the application you are building will be another REST API, so it needs a http input node and an http output node adding to the canvas.
   - Between the two http nodes, add a change node (shown as set msg.payload), a visual recognition node, and a function node. Connect these nodes.
   - Configure the http input node by setting the method to POST, the URL to /performVisualReco, and the name to Visual Recognition.
   - Configure the change node to set msg.payload to the input URL. The POST request will send a URL as an HTML POST parameter named imageurl. The visual recognition node is looking for the URL on msg.payload.
   - The next node to configure is the visual recognition node. You need an API key. If you already have an API key, go to your IBM Cloud account, open the Visual Recognition service, and copy the API key and skip to the next step
   - If you do not have an API key, follow these steps:
   - Log in to IBM Cloud and click Catalog. Then, search for and select the Visual Recognition service
   - Select the region that you want to deploy the service in, the pricing plan that you want, and then click Create
   - Wait for the service to be created. Then, copy your credentials. If they are hidden, click Show Credentials
   - If you connect the Visual Recognition service to your Node-RED instance on IBM Cloud and restart Node-RED, the key is automatically detected by Node-RED
   - Open the visual recognition node and paste the API key and service endpoint into the corresponding field. Make sure the Detect field is set to Classify an image. When you're finished, click Done
   - Configure the function node by naming it Parse Response. Then, paste in the following code:

    ```if (msg.result && ```
        ```msg.result.images &&``` 
        ```Array.isArray(msg.result.images) &&```
        ```0 < msg.result.images.length) {```
        ```msg.payload = msg.result.images[0];```            
        ```} else {```
        ```msg.payload = {'error' : 'Error Processing Image'};```
    ```}```
    ```return msg;```

   - This code verifies that there is a valid result for the first image. This node sets the result to msg.payload. If the result is invalid, it will send an error code to the calling web page
   - Now that you've added all the new flows, you must update the HTML and JavaScript nodes to allow for the Visual Recognition service to work through the user interface
   - Edit the HTML node and delete all the previous code. Go to the animal-sounds repository and copy the HTML code for Section 2 into the HTML field. There are comments in the code to highlight the new bits for the Visual Recognition service to work
   - The new code consists of two rows of cat and dog images with a button to analyze the image
   - Edit the JavaScript node by deleting all the previous code and pasting in the new code from the Section_2_JavaScript.js file
   - The new code adds to the code added in the first section of this lab. It adds the API information for the Visual Recognition service /performVisualReco, which you created earlier, and adds an image picker so that you can select and image and pass it to the API
   - Configure the CSS node by pasting in the new code from the Section_2_CSS.css file
   - This code adds styling to the image picker elements so that you know which image is selected
   - Deploy the application and go to your web page. You should now have a page where you can process both images and audio files
   - If you select an image and click Analyze Animal Image, you see the result in the table
   - The final flow looks like this. Some comment nodes were added to describe this flow
   - Optional: Combine the audio result with the image result and see what happens if both the audio and image are a dog. See what happens to the result if one is a dog and one is a cat
   - You've now created a simple web page that can show the results of your machine learning model and the results of running the Watson Visual Recognition service

3. Create the interface for the birdsong audio
   - In this section, you will create an interface for the birdsong prediction
   - From your cats and dogs flow, copy all of the nodes
   - Create a tab and call it Birdsong UI and paste the nodes onto the canvas
   - Delete the following nodes because they are not required for this part:
   - The Birdsong interface will not be using the Visual Recognition service, so there is no need for that flow. The flow should now look like this:
   - Edit the http input node by changing the URL endpoint to something like /birdsong
   - Delete the contents of the CSS node. You can add your own styling here
   - Update the JavaScript node with code from the Section_3_JavaScript.js file
   - The code is similar to that from the cats and dogs audio flow where it checks the audio file, calls the prediction API, and displays the results on the page. In addition, the JavaScript expects predictions to be delivered through WebSockets, which you will be creating shortly
   - Edit the HTML node. Delete the contents and replace them with the code from Section_3_HTML.html:
   - This code is similar to the cats and dogs HTML before the Visual Recognition service was added
   - Deploy the application and navigate to the URL that you provided in the http input node, for example, ```https://catsdogs.au-syd.mybluemix.net/birdsong```
   - You should see a basic web page
   - Change the API that is called for the audio buffer. In the cats and dogs flow, the API is called /performAudioReco. This cannot be the same value for a second API call
   - Edit the node and change the name to /performBirdAudioReco. Do not change the Locate Audio Buffer node
   - Between the Locate Audio Buffer node and the http node, add two function nodes, one for each output
   - Edit the function node from the first output. Name the node Error Response and paste in the following code:
       - ```msg.payload = {'error' : 'No File Received'};```
       - ```return msg;```
   - Now, if you try to run a prediction without passing in an audio file, the application will handle the error and return it as a message
   - In the second function node, name it Good Response and paste in the following code:

msg.payload = {'response' : 'received'};
return msg;
If an audio file is successfully passed through the API, it returns the message received.

The flow should look like this:

Because the birdsong data set has up to nine models, you need nine machine learning nodes. Therefore, in the next few steps, you will clean up the flows and create space for the additional nodes.

Delete the links surrounding the Run Prediction node.

Move the Run Prediction node to the bottom of all the nodes. Then, add a link output node and join it to the Setup for Prediction node.

Add a link input and a link output node around the Run Prediction node.

Add a link input node before the Just the Results node. Give the link nodes appropriate names so that it's easier to identify them.

The flow should now look like this:

Wire the link nodes as shown in the following image:

Add eight more machine learning nodes below the Run Prediction node and link them all to the link input and link output nodes:

Configure each machine learning model to the models and deployments that you created in Lab 4.

For example, as shown in the following image, you see five models for this Watson Machine Learning instance; therefore, assign each node to a different model.

In the Just the Results function node, paste in the following code:

msg.result = msg.payload.values[0].splice(152);
msg.resultColumns = msg.payload.fields.splice(152);

msg.payload = {
    'result' : msg.result,
    'columns' : msg.resultColumns
};

return msg;
This code makes the result easier to read.

After the Just the Results node, delete the link to the http response node and add a websocket output node.

A WebSocket is a one-way communication link. Your web page is expecting responses through this WebSocket link. Every time the Machine Learning service makes a prediction, it updates the web page.

Now, the websocket node needs to be configured to publish updates on the address that the browser JavaScript is listening on. The JavaScript code provided to you includes the following lines where the WebSocket URI is defined:

  // This needs to point to the web socket in the Node-RED flow
  // ... in this case it's ws/birdsong
  wsUri += "//" + loc.host + "/ws/birdsong";
Configure your websocket node path to /ws/birdsong.

Now, if you go to the birdsong web page, you should be able to select an audio file and get a result against each machine learning model.

The following example shows the Common Swift being processed in the results table. You can see from the results that the Common Swift is the highest probability score with 0.38. There are repeated bird names in the table because some birds have been incorporated into more than one machine learning model. 


You've now created a second simple web page that can show the results of your machine learning models.