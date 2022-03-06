# Yolov4_Road_Damage_Detection
A Repository to Train a Custom Yolov4 based object detector using the RDD2020 dataset.

# Table of Contents
1. [ Dataset ](#data)
2. [ Model ](#model)
3. [ Configurations ](#Configurations)
4. [ Model Training ](#Training) 
5. [ Additional Information ](#info)


<a name="data"></a>
# Dataset

1. Dataset is taken from RDD2020: https://data.mendeley.com/datasets/5ty2wb6gvg/1

    Deeksha Arya, Hiroya Maeda, Sanjay Kumar Ghosh, Durga Toshniwal, Hiroshi Omata, Takehiro Kashiyama, Toshikazu Seto, Alexander Mraz, Yoshihide Sekimoto

2. RDD2020 dataset comprising 26,336 road images from India, Japan, and the Czech Republic with more than 31,000 instances of road damage.

3. Here we consider a subset of the dataset, i.e. the images from India alone, to reduce complexity

4. There are four types of road damage: longitudinal cracks (D00), transverse cracks (D10), alligator cracks (D20), and potholes (D40)

5. The data is present in the PascalVOC format as bounding boxes labelled as xmin, ymin, xmax and ymax, stored as xml files

<a name="model"></a>
# Model

Yolov4


<a name="Configurations"></a>
# Configurations



<a name="Training"></a>
# Model Training

1.  Open command line cmd at the root of the repository.

2.  Run the command   

    `pip install -r requirements.txt` 

3. Open the Notebook `Training_Notebook.ipynb` to follow all the preprocessing and training steps of the model.


<a name="Version"></a>

<a name="info"></a>
# Additional Information

## Use of [darknet](https://github.com/AlexeyAB/darknet)
The model for YOLOv4 is taken from repository of AlexeyAB.

## Python Version
The whole project is developed with python version `Python 3.7.7` and pip version `pip 19.2.3`.
## Contact
In case of error, feel free to contact us over Linkedin at [Adnan](https://www.linkedin.com/in/adnan-karol-aa1666179/) and [Niloy](https://www.linkedin.com/in/niloy-chakraborty/).
