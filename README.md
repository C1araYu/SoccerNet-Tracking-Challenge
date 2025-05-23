# SoccerNet Tracking Challenge
### DL-Assisted-Referee: Kuai Yu (ky2589), Ziwei Wang (zw2915), Emily Wang (eaw2233)

This project includes two different implementations of the task: a modified SoccerNet-Tracking-Challenge ByteTrack baseline, as well as a version of SoccerNet's Game State Reconstruction Challenge baseline, enhanced with domain knowledge to optimize Tracking performance.  

The SoccerNet Tracking Challenge involves detecting bounding boxes for objects from live broadcast footage of soccer games, as well as assigning consistent tracklet IDs for objects moving in and out of frame.

## How to run code
### Running ByteTrack_Implementation
TO set up:
```
    cd <SN_TRACKING_HOME>/Benchmarks/ByteTrack
    cp -i demo_track_no_gt.py <ByteTrack_HOME>/tools/demo_track_no_gt.py
    cp -i yolox_x_soccernet_no_gt.py <ByteTrack_HOME>/exps/example/mot/
    cp run_bytetrack_no_gt_batch.sh <ByteTrack_HOME>
    cp tools/evaluate_soccernet_v3_tracking.py <ByteTrack_HOME>
```
To obtain output: 
```
cd <SN_TRACKING_HOME>/Benchmarks/ByteTrack
PYTHONPATH=~/ByteTrack_Implementation/ByteTrack_HOME:$PYTHONPATH bash run_bytetrack_no_gt_batch.sh
```

To evaluate:
```
    export ByteTrack_HOME=<ByteTrack_HOME>
    cd <ByteTrack_HOME>
    export SN_TRACKING_MODE=test
    bash run_bytetrack_gt_batch.sh
```

### Running GSR_Implementation
Please refer to the README file in /GSR_Implementation.
## Directory Structure, Key Files
This repo contains the two implementations: our modified ByteTrack baseline, and our improved GSR-inspired method.  Also in the home directory is the dataset sample.

### ByteTrack Implementation
Key files of the ByteTrack_Implementation are the ByteTrack_Implementation/SN_TRACKING_HOME/Benchmarks/ByteTrack, which is where the commands to run the above code are done. The dataset should be stored in ByteTrack_Implementation as tracking-2023, and filenames may need to be changed to match that.   
Other useful files are MOTVisualize.py in ByteTrack_Implemenation, if you change the filepaths at the bottom, you can turn your detections and tracklet IDs into annotated video output.  

### GSR_Implementation
#### Key File Descriptions

- **`configs/soccernet_modified.yaml`**  
  Contains the complete Hydra configuration used to run the improved GSR pipeline. It enables transformer-based association, learned ReID, and filtering heuristics.

- **`src/data/soccernet_dataset.py`**  
  Responsible for loading SoccerNet player annotations and preparing video frame inputs. Ensures compatibility with our modified detection and tracking models.

- **`src/model/transformer_assoc.py`**  
  Implements a lightweight transformer that performs temporal association between detected player instances across frames, reducing ID switches caused by occlusion or camera movement.

- **`src/model/mlp_reid.py`**  
  Defines a small MLP architecture to refine appearance embeddings extracted from detections, used in pairwise player ReID.

- **`src/utils/pitch_filter.py`**  
  Applies geometric constraints to exclude bounding boxes detected outside the soccer pitch (e.g., crowd or sideline), improving tracking focus and accuracy.

- **`src/utils/eval_metrics.py`**  
  Includes standard evaluation metrics such as HOTA, MOTA, MOTP, and IDF1, providing consistent comparisons across methods.

- **`src/run_gsr.py`**  
  Main entry point that ties the entire pipeline together: detection → ReID embedding → transformer-based association → pitch filtering → evaluation and result saving.

- **`pretrained_models/`**  
  Contains pretrained weights for the transformer association module and MLP ReID head. These are loaded during inference to reproduce reported results.

### How to Run

See this folder's `README.md` for step-by-step instructions on how to configure, run, and evaluate the tracking pipeline.


## Dependencies, Setup
### ByteTrack ImplementationSetup
This implementation utilizes the (sn-tracking)[https://github.com/SoccerNet/sn-tracking.git] module, the (ByteTrack)[https://github.com/ifzhang/ByteTrack.git] module, and the (YOLOX)[https://github.com/Megvii-BaseDetection/YOLOX.git] modules.  However, due to some import and setup errors, these modules are cloned and modified locally for the moment to fix these issues.  Thus, there is no need to install them individually, but their requirements are still needed.

#### ByteTrack: These instructions are in ByteTrack_HOME's README, but skip the installation of ByteTrack itself.
1. Requirements.txt
```
cd ByteTrack_HOME
pip3 install -r requirements.txt
python3 setup.py develop
```
2. Install pycocotools: `pip3 install cython; pip3 install 'git+https://github.com/cocodataset/cocoapi.git#subdirectory=PythonAPI'`
3. `pip3 install cython_bbox`

#### SoccerNet:
Follow the instructions in the README from ByteTrack_Implementation/SN_TRACKING_HOME
1.  Step 1
`pip install soccernet`
2. Step 2, in Python
```
from SoccerNet.Downloader import SoccerNetDownloader
mySoccerNetDownloader = SoccerNetDownloader(LocalDirectory="path/to/SoccerNet")
mySoccerNetDownloader.downloadDataTask(task="tracking", split=["train","test","challenge"])
mySoccerNetDownloader.downloadDataTask(task="tracking-2023", split=["train", "test", "challenge"])
```
#### GSR Implementation and SETUP: These instructions are in GSR_Implementation's README



