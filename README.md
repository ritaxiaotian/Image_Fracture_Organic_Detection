# Image_Fracture_Organic_Detection Readme
object detection

Steps for fracture and OM detection

1. Create xml files for SEM images using Lableimg : https://github.com/ritaxiaotian/labelImg

2. Creating TFRecords:    
* xml_to_csv.py: generate train.csv, test.csv
  * python3 xml_to_csv.py
* generate_tfrecord.py: generate tfrecord   
  * From tensorflow/models/   
  * Create train data:    
  * python3 generate_tfrecord.py --csv_input=data/train_labels.csv --output_path=data/train.record    
  * Create test data:   
  * python3 generate_tfrecord.py --csv_input=data/test_labels.csv  --output_path=data/test.record   

3. Training fracture and organic matter object detector: set up configure files & pick up a model
* 3a configuring the object detection training pipeline: configure your own model, Github
* 3b use ssd_mobilenet model (https://github.com/tensorflow/models/tree/master/research/object_detection/samples/configs)


**file configure** 
-----------------------------------------------------------------------------------------------------------------------

Object-Detection    
-data/    
--test_labels.csv   
--train_labels.csv    
-images/    
--test/   
---testingimages.jpg    
--train/    
---testingimages.jpg    
--...yourimages.jpg   
-training   
-xml_to_csv.py    

--------------------------------------------------------------------------------------------------------------------------
* under models/research   
protoc object_detection/protos/*.proto --python_out=.
 
* under models/research   
*under models/research/object_detection  
export PYTHONPATH=$PYTHONPATH:`pwd`:`pwd`/slim    

* under models/research   
python3 train.py --logtostderr --train_dir=training/ --pipeline_config_path=training/ssd_mobilenet_v1_pets.config

python3 train.py --logtostderr --train_dir=training/ --pipeline_config_path=training/ssd_mobilenet_v1_pets.config

**look at tensor flow training log**

at terminal: models/object_detection

tensorboard --logdir=training/

4. Testing fracture and organic matter object detector

**export_inference_graph.py: **

python3 export_inference_graph.py \
    --input_type image_tensor \
    --pipeline_config_path training/ssd_mobilenet_v1_pets.config \
    --trained_checkpoint_prefix training/model.ckpt-43215 \
    --output_directory frac_inference_graph


references : https://www.youtube.com/watch?v=COlbP62-B-U&list=PLQVvvaa0QuDcNK5GeCQnxYnSSaar2tpku

references : https://github.com/tensorflow/models/tree/master/research/object_detection

**Google Cloud: cd ..   
cd xiaoran    
sudo jupyter notebook --ip 0.0.0.0 --port 8888 --allow-root**
