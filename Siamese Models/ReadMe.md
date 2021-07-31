## **Working**

1. Contains a Siamese Architecture Implementation of the Application.

&nbsp;

2. Feature Extraction is performed as part of the data preprocessing pipeline using one of many pretrained models. End model used for training is merely a Multi-Layered Perceptron. This method is used so that the illusion of Siamese Architecture can be given without needing to run the data through the entire Deep Network every epoch.

&nbsp;

3. Architecture of MLP used is [ input_layer --> embed_layer --> output ]
    - input_layer&nbsp;&nbsp;&nbsp; - Size of the feature vector obtained from the pretrained network.
    - embed_layer - New size of the feature embeddings. Can be provided by user via the command line.
    - output_layer&nbsp; - Single neuron which after passing through a sigmoid gives a percent prediction / similarity score.

&nbsp;

4. Operations taking place during an application run:
    - Capture of an image from video a feed at the discretion of the user. An Object Detector is used to extract the approximate bounding box of the component within the frame. This bounding box is used in the Realtime Application to set a predefined area within the frame where the object can be placed. (Bounding Box Detection is not applicable for Real World Positive and Negative Base Samples and Dumb to Intelligent)
    - Creation of the Feature Vector Dataset.
        - Define a pretrained model to use and cut off the final classification layer.
        - Artificially increase dataset size by augmentation. 
        - Apply necessary transforms and pass images through the network to obtain features. Concatenate and Save.
    - Load features saved in Step 2 and pass it through a dataset creation pipeline making the data suitable for usage with a Siamese Network. Since pytorch is being used, also build the dataloaders.
    - Train the model. At present, only limited hyperparameters are present and can only be changed from the source. However, end user can set:
        - Number of Epochs (Default: 1000)
        - Number of Samples (Default: 1000)
        - Size of New Embeddings (Default: 2048)
        - Lower Confidence Bound (Default: 0.95)
        - Upper Confidence Bound (Default: 0.99)
        - Device ID of capture Device (Default: 0)
        - Early Stopping Epoch (Default: 50)

&nbsp;

5. Realtime Application of the designed network.
    - Extract features of every frame.
    - Pass these features into the trained MLP to obtain a prediction.
    - Based on a predefined confidence, classify the image as Match, Possible Match or Defective.
    - If it is a False Positive or False Negative, press the appropriate button to add image captured from feed into the correct directory.

&nbsp;

---

&nbsp;

To install the application's development version of Pytorch, use:

- `pip install torch==1.8.0+cu111 torchvision==0.9.0+cu111 torchaudio==0.8.0 -f https://download.pytorch.org/whl/torch_stable.html`

&nbsp;

---

&nbsp;

## **CLI Arguments**

<pre>
1. --num-samples - Number of Samples to be created for each class in the Dataset

2. --embed       - Size of the new Embeddings

3. --epochs      - Number opf training epochs

4. --id          - Device ID of the capture device

5. --lower       - Lower Confidence Bound of the System

6. --upper       - Lower Confidence Bound of the System

7. --early       - Number of epochs to wait after stagnated validation metrics before stopping the training
</pre>

&nbsp;

---