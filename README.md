# Image_Fracture_Organic_Detection Readme
object detection

Steps for fracture and OM detection

1. Create xml files for SEM images using Lableimg : https://github.com/ritaxiaotian/labelImg

2. Creating TFRecords:    
* xml_to_csv.py: generate train.csv, test.csv
  * python3 xml_to_csv.py
  
  modify code:
  
  def main():
    for directory in ['train','test']:
        image_path = os.path.join(os.getcwd(), 'images/{}'.format(directory))
        xml_df = xml_to_csv(image_path)
        xml_df.to_csv('data/{}_labels.csv'.format(directory), index=None)
        print('Successfully converted xml to csv.')

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

python3 train.py --logtostderr --pipeline_config_path=${PATH_TO_YOUR_PIPELINE_CONFIG} --train_dir=${PATH_TO_TRAIN_DIR}

python3 train.py --logtostderr --train_dir=training/ --pipeline_config_path=training/faster_rcnn_inception_resnet_v2_atrous_coco.config

#### Evaluation Job

PATH_TO_TRAIN_DIR  = training/ 

PATH_TO_YOUR_PIPELINE_CONFIG = training/ssd_mobilenet_v1_pets.config


python3 eval.py --logtostderr --pipeline_config_path=${PATH_TO_YOUR_PIPELINE_CONFIG} --checkpoint_dir=${PATH_TO_TRAIN_DIR} \
    --eval_dir=${PATH_TO_EVAL_DIR}
    
python3 eval.py --logtostderr --training/ssd_mobilenet_v1_pets.config --checkpoint_dir=training/ --eval_dir=Eval/
    
**python3 train.py --logtostderr --train_dir=training/ --pipeline_config_path=training/faster_rcnn_inception_v2_coco.config**

**look at tensor flow training log**

at terminal: models/object_detection

tensorboard --logdir=training/

4. Testing fracture and organic matter object detector

**export_inference_graph.py: **

python3 export_inference_graph.py \
    --input_type image_tensor \
    --pipeline_config_path training/ssd_mobilenet_v1_pets.config \
    --trained_checkpoint_prefix training/model.ckpt-100252 \
    --output_directory ssd_mobilenet_v1_OverviewPlusFrac_inference_graph
    
    
5. Test

# What model to download.
MODEL_NAME = 'ssd_mobilenet_v1_OverviewPlusFrac_inference_graph'

# Path to frozen detection graph. This is the actual model that is used for the object detection.
PATH_TO_CKPT = MODEL_NAME + '/frozen_inference_graph.pb'

# List of the strings that is used to add correct label for each box.
PATH_TO_LABELS = os.path.join('training', 'object-detection.pbtxt')

NUM_CLASSES = 1
Next, we can just delete the entire Download Model section, since we don't need to download anymore.

Finally, in the Detection section, change the TEST_IMAGE_PATHS var to:

TEST_IMAGE_PATHS = [ os.path.join(PATH_TO_TEST_IMAGES_DIR, 'image{}.jpg'.format(i))
    
**Google Cloud: cd ..   
cd xiaoran    
sudo jupyter notebook --ip 0.0.0.0 --port 8888 --allow-root**


