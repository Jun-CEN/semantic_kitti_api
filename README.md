# Open-world metrics calculation for SemanticKITTI

We modify the official SemanticKITTI api to calculate the closed-set mIoU and open-set metrics
including AURP, AUROC, and FPR95 in this repository.

To use this repository for calculating metrics, the closed-set prediction labels and uncertainty scores
for each points in the dataset should be generated and saved as:
### SemanticKITTI
```
./
├── 
├── ...
└── dataset/
    ├──sequences
        ├── 00/           
        │   ├── velodyne/	
        |   |	├── 000000.bin
        |   |	├── 000001.bin
        |   |	└── ...
        │   └── labels/ 
        |       ├── 000000.label
        |       ├── 000001.label
        |       └── ...
        ├── 08/ # for validation
        ├── 11/ # 11-21 for testing
        └── 21/
	    └── ...
└── predictions/
    ├──sequences
        ├── 08/           
        │   ├── closed-set_prediction_results/	
        |   |	├── 000000.label
        |   |	├── 000001.label
        |   |	└── ...
        │   └── uncertainty_scores/ 
        |       ├── 000000.score
        |       ├── 000001.score
        |       └── ...
```

## Evaluation
### SemanticKITTI
- First, remap the closed-set prediction results to the non-entropy type, the path is determined
in the `remap_semantic_label.sh` and line 74 of `remap_semantic_label.py`.
```
./remap_semantic_labels.sh
```
- Then, run the evaluation script to obtain the closed-set mIoU and open-set metrics including AUPR,
AUROC, and FPR95. The path is determined in the `evalute_semantics.sh` and line 158, 171 of `evaluate_semantics.py`.
```
./evaluate_semantics.sh
```
- The result is shown like:
```
********************************************************************************
INTERFACE:
Data:  /harddisk/semantic_kitti/dataset
Predictions:  /harddisk/semantic_kitti/predictions
Backend:  numpy
Split:  valid
Config:  config/semantic-kitti.yaml
Limit:  None
Codalab:  None
********************************************************************************
Opening data config file config/semantic-kitti.yaml
Ignoring xentropy class  0  in IoU evaluation
[IOU EVAL] IGNORE:  [0]
[IOU EVAL] INCLUDE:  [ 1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19]
labels:  4071
predictions:  4071
Evaluating sequences: 10% 20% 30% 40% 50% 60% 70% 80% 90% AUPR is:  0.20303703822506583
AUROC is:  0.852544601771817
FPR95 is:  0.4676692998629576
Validation set:
Acc avg 0.900
IoU avg 0.574
IoU class 1 [car] = 0.923
IoU class 2 [bicycle] = 0.415
IoU class 3 [motorcycle] = 0.553
IoU class 4 [truck] = 0.762
IoU class 5 [other-vehicle] = 0.000
IoU class 6 [person] = 0.651
IoU class 7 [bicyclist] = 0.821
IoU class 8 [motorcyclist] = 0.000
IoU class 9 [road] = 0.945
IoU class 10 [parking] = 0.447
IoU class 11 [sidewalk] = 0.813
IoU class 12 [other-ground] = 0.011
IoU class 13 [building] = 0.857
IoU class 14 [fence] = 0.405
IoU class 15 [vegetation] = 0.870
IoU class 16 [trunk] = 0.621
IoU class 17 [terrain] = 0.754
IoU class 18 [pole] = 0.582
IoU class 19 [traffic-sign] = 0.465
********************************************************************************
below can be copied straight for paper table
0.923,0.415,0.553,0.762,0.000,0.651,0.821,0.000,0.945,0.447,0.813,0.011,0.857,0.405,0.870,0.621,0.754,
0.582,0.465,0.574,0.900

```