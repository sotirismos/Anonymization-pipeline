## Preliminary ##

Download hdf model files:

- Pretrained model on MS coco
	```
    wget https://github.com/OlafenwaMoses/ImageAI/releases/download/1.0/yolo.h5
	```
	or
	```
	 git lfs pull --include "yolo.h5"
	```
	
	Pretrained model in weights format
	```
	 wget https://pjreddie.com/media/files/yolov3.weights
	```
	
	
- Custom model for LP
 ```
 	git lfs pull --include "yolov3_custom.h5"
 ```
 
 Install git-lfs through the package manager.

---

## Depersonalization pipeline ##


### Two stage license plate detector ###

In the first stage all bounding boxes associated with **car, truck, motorbike, bus** labels 
are selected and cropped serving as regions of interest, then 
in the second stage the cropped region of the original image is fed 
to the custom model trained to detect **license plates**.
This script returns a **pkl** file containing a list.
This list can be used to calculate various [metrics](https://github.com/sotirismos/Object-Detection-Metrics).

---
Each list element is a dictionary that contains information about
the detected and ground truth objects in an image,
a summary of its key/value pairs is presented below:

| Key      | Value |
| -------  | ----- |
| det      | list of detected bboxes in [imageAI](https://github.com/OlafenwaMoses/ImageAI) format|
| filename | the filename of the related image |
| gt       | a list of ground truth objects |
| matches  | a list of tupled index pairs |

---

Usage:

``` sh
python two_stage_lp.py --thresh 0.5 
					   --anno_dir <path to annotations directory> 
					   --img_dir <path to images directory> 
					   --config ./config/yolo_config.json ./config/yolov3_custom_config.json 
					   --model ./models/yolo.h5 ./models/yolov3_custom.h5 
					   --out_dir <output directory for the annotated images>
					   --vehicle_thresh <vehicle detection threshold>
					   --lp_thresh <lp detection threshold>
```

---

### Weights to HDF conversion ###

Convert darknet weights format to hdf5 format used by keras and its wrapper imageai.

Original code adapted from the folowing repo:
[experiencor/keras-yolo3](https://github.com/experiencor/keras-yolo3)

Requirements file:
[requirements.txt](https://github.com/experiencor/keras-yolo3/blob/master/requirements.txt)

Usage:

```
 python weights_to_h5.py --weights <path to .weights file> --hdf5 <path to .h5 file > --config <path to json configuration file>
```

---

### Model files and configuration ###

The table below describes the classes predicted by each model file

| Model file | Class list | 
| ---------- | ---------- |
| [yolo.h5](https://github.com/sotirismos/GRUBLES/blob/master/models/yolo.h5) | MS COCO (80 classes, including cars) |
| [yolov3_custom.h5](https://github.com/sotirismos/GRUBLES/blob/master/models/yolov3_custom.h5) | License plates |


The table below describes the model - config file relations

| Model file | Config file | 
| ---------- | ----------  |
| [yolo.h5](https://github.com/sotirismos/GRUBLES/blob/master/models/yolo.h5) | [coco_config.json](https://github.com/sotirismos/GRUBLES/blob/master/configs/coco_config.json) |
| [yolov3_custom.h5](https://github.com/sotirismos/GRUBLES/blob/master/models/yolov3_custom.h5) | [darknet_config.json](https://github.com/sotirismos/GRUBLES/blob/master/configs/darknet_config.json) |


