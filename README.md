# SoccerNet Tracking Challenge
### DL-Assisted-Referee: Kuai Yu (ky2589), Ziwei Wang (zw2915), Emily Wang (eaw2233)

This project includes two different implementations of the task: a modified SoccerNet-Tracking-Challenge ByteTrack baseline, as well as a version of SoccerNet's Game State Reconstruction Challenge baseline, enhanced with domain knowledge to optimize Tracking performance.  

The SoccerNet Tracking Challenge involves detecting bounding boxes for objects from live broadcast footage of soccer games, as well as assigning consistent tracklet IDs for objects moving in and out of frame.

## How to run code

### ByteTrack Implementation:
This implementation utilizes the (sn-tracking)[https://github.com/SoccerNet/sn-tracking.git] module, the (ByteTrack)[https://github.com/ifzhang/ByteTrack.git] module, and the (YOLOX)[https://github.com/Megvii-BaseDetection/YOLOX.git] modules.  However, due to some import and setup errors, these modules are cloned and modified locally for the moment to fix these issues.  Thus, there is no need to install them individually, but their requirements are still needed.

#### ByteTrack: These instructions are in ByteTrack_HOME's README, but skip the installation of ByteTrack itself.
1. Requirements.txt
'''
cd ByteTrack_HOME
pip3 install -r requirements.txt
python3 setup.py develop
'''
2. Install pycocotools: 'pip3 install cython; pip3 install 'git+https://github.com/cocodataset/cocoapi.git#subdirectory=PythonAPI''
3. 'pip3 install cython_bbox'
'''
PYTHONPATH=~/ByteTrack_Implementation/ByteTrack_HOME:$PYTHONPATH bash run_bytetrack_no_gt_batch.sh
'''

## Directory Structure, Key Files
This repo contains the two implementations: our modified ByteTrack baseline, and our improved GSR-inspired method.  Also in the home directory is the dataset sample.

## Dependencies, Setup

#### ByteTrack Implementation: These instructions are in ByteTrack_HOME's README, but skip the installation of ByteTrack itself.
1. Requirements.txt
'''
cd ByteTrack_HOME
pip3 install -r requirements.txt
python3 setup.py develop
'''
2. Install pycocotools: 'pip3 install cython; pip3 install 'git+https://github.com/cocodataset/cocoapi.git#subdirectory=PythonAPI''
3. 'pip3 install cython_bbox'

#### SoccerNet:
'pip install soccernet'

