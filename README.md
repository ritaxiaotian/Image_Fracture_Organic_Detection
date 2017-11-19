# Image_Fracture_Organic_Detection Readme
object detection

Steps for fracture and OM detection

1. Create xml files for SEM images using Lableimg : https://github.com/ritaxiaotian/labelImg

2. Creating TFRecords: xml_to_csv.py

3. Training fracture and organic matter object detector: set up configure files & pick up a model
* 3a configuring the object detection training pipeline: configure your own model, Github
* 3b use ssd_mobilenet model (existing models

4. Testing fracture and organic matter object detector

references : https://www.youtube.com/watch?v=COlbP62-B-U&list=PLQVvvaa0QuDcNK5GeCQnxYnSSaar2tpku

references : https://github.com/tensorflow/models/tree/master/research/object_detection
