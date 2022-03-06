# Yolov4_Road_Damage_Detection
A Repository to Train a Custom Yolov4 based object detector for road damage detection using the RDD2020 dataset.

# Table of Contents
1. [ Dataset ](#data)
2. [ Model ](#model)
3. [ Configurations ](#Configurations)
4. [ Model Training ](#Training) 
5. [ Results ](#Results) 
6. [ Testing ](#Testing) 
7. [ Additional Information ](#info)


<a name="data"></a>
# Section 1: Dataset

1. Dataset is taken from RDD2020: https://data.mendeley.com/datasets/5ty2wb6gvg/1

    Deeksha Arya, Hiroya Maeda, Sanjay Kumar Ghosh, Durga Toshniwal, Hiroshi Omata, Takehiro Kashiyama, Toshikazu Seto, Alexander Mraz, Yoshihide Sekimoto

2. RDD2020 dataset comprising 26,336 road images from India, Japan, and the Czech Republic with more than 31,000 instances of road damage.

3. Here we consider a subset of the dataset, i.e. the images from India alone, to reduce complexity.

4. There are four types of road damage: longitudinal cracks (D00), transverse cracks (D10), alligator cracks (D20), and potholes (D40).

5. The data is present in the PascalVOC format as bounding boxes labelled as xmin, ymin, xmax and ymax, stored as xml files.

<a name="model"></a>
# Section 2: Model

## About the YOLOv4 Model

## Downloading YOLOv4.

At the root of the project clone the darknet repository using the command:

        git clone https://github.com/AlexeyAB/darknet

This will create a folder called 'darknet' at the root of the project.

<a name="Configurations"></a>
# Section 3: Configurations

## Data

1. Collect the images and xml annotation files from RDD2020 into a single folder 'Data_India'.

2. The annotation of the bounding boxes are currently in an xml format, for example : 

        <annotation>
        <folder>images</folder>
        <filename>India_000312.jpg</filename>
        <size>
            <depth>3</depth>
            <width>720</width>
            <height>720</height>
        </size>
        <object>
            <name>D00</name>
            <bndbox>
            <xmin>420</xmin>
            <ymin>489</ymin>
            <xmax>546</xmax>
            <ymax>715</ymax>
            </bndbox>
        </object>
        </annotation>

3. This has to be converted into the YOLO format. The YOLO format for the above xml file is :
   
        0 0.6694444444444445 0.8347222222222223 0.17500000000000002 0.3138888888888889

4. In order to perform this, open the script `pascal_yolo_conversion.py` and edit the variables below. Here 'dir_path' denotes the folder where all images and xml files are stored, and 'classes' denotes the names of class of objects to detect.

        dir_path = 'Data_India/'
        classes = ['D00', 'D10', 'D20', 'D40', 'D44']

5.  Open command line cmd at the root of the repository.

6.  Run the command   

    `python pascal_yolo_conversion.py` 

7. A new folder called 'YOLO' is created in the 'dir_path' folder. Copy the contents of the 'YOLO' folder to the 'dir_path' folder. 

8. You can now remove all the xml files.
   
9.   Finally, your data for training the YOLOv4 model is now ready.

## Configuration Files

1. Create a configs folder at the root. This will contain all config files related to configuring the YOLO model.

2. `obj.data` : Change the number of classes to number of classes you are working on. Create a training folder at the root, and this will store your training weigths. Make sure to check all other paths. It is best to provide path relative to root. Check the sample provided.

3. `obj.names` : On every new line mention the names or labels of the objects to be detected. Check the sample provided.

4. `yolov4-custom.cfg` : This is a very important file and requires 5 important parameters changes. This file is also present in the 'darknet/cfg' folder as 'yolov4-custom.cfg'.
   1. Recommended having batch = 64 and subdivisions = 16 for ultimate results. If you run into any issues then up subdivisions to 32.
   2. Set max_batches = (# of classes) * 2000 (but no less than 6000). So if you are training for 1, 2, or 3 classes it will be 6000, however detector for 5 classes would have max_batches=10000.
   3. Set steps = (80% of max_batches), (90% of max_batches) (so if your max_batches = 10000, then steps = 8000, 9000).
   4. Search for classes and set it to number of classes. (You should find it 3 times).
   5. Above every classes, some lines above you should find a parameter names filters. Set filters = (# of classes + 5) * 3 (so if you are training for one class then your filters = 18, but if you are training for 4 classes then your filters = 27).


## Generate train.txt and test.txt

These files contain the path of the images to be used for training and testing relative to the darknet folder.

1. Inside the folder 'Data_India/' create two folders 'train' and 'test'.

2. Divide all images and xml files present in the folder 'Data_India/' into the 'train' and 'test' folders in a 90:10 ratio (or 80:20, if there are sufficient training images). You can do this manually or write a script for the same. The python package `split-folders` at [pypi](https://pypi.org/project/split-folders/) can also be helpful in this case.

3. Once the folders 'train' and 'test' are created, now run the commands at the root of the project:

    `python gen_train.py` 

    `python gen_test.py` 

4. Make sure that your paths present in the scripts are with respect to the root of the darknet folder.

5. Sample 'train.txt' and 'test.txt' is provided in the 'configs' folder.

## Pre-trained Yolo weights

Download and save pre-trained weights and save it in the configs folder using the command:
   
    !wget https://github.com/AlexeyAB/darknet/releases/download/darknet_yolo_v3_optimal/yolov4.conv.137



<a name="Training"></a>
# Section 4: Model Training

1.  Open command line cmd at the root of the repository.

2.  Run the command   

    `pip install -r requirements.txt` 

3. Open the Notebook `Training_Notebook.ipynb` to follow all the steps for training the model.

4. Every 100 iterations the weigths are stored in the folder mentioned in the 'obj.data' as parameter backup. And for every 1000 iteration a new weights file is created.


<a name="Results"></a>
# Section 5: Results

1. The training curve of MAP (Mean Average Precision) vs the iteration number is stored in the folder darknet as 'chart.png'. 
    
    This can be used to understand when to stop the training while evaluating the MAP value. 
    
    MAP is an evaluation metric, commonly used in the field of computer vision used for object detection (i.e. localisation and classification)

2. The results of our trained models is shown in image below.
   

<a name="Testing"></a>
# Section 6: Testing
Some samples of images tested with our model are shown below.

![Test Image 1](Images/test_1.png)

![Test Image 2](Images/test_2.png)

![Test Image 3](Images/test_3.png)


<a name="info"></a>
# Section 7: Additional Information

## Use of [Darknet](https://github.com/AlexeyAB/darknet)
References: The model for YOLOv4 is taken from repository of AlexeyAB.

## Python Version
The whole project is developed with python version `Python 3.7.7` and pip version `pip 19.2.3`.
## Contact
In case of error, feel free to contact us over Linkedin at [Adnan](https://www.linkedin.com/in/adnan-karol-aa1666179/) and [Niloy](https://www.linkedin.com/in/niloy-chakraborty/).
