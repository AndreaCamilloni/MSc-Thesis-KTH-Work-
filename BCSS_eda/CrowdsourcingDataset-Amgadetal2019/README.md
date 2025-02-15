# Breast Cancer Semantic Segmentation (BCSS) dataset

This repo contains the necessary information and download instructions to download the dataset associated with the paper:

***_[Amgad M, Elfandy H, ..., Gutman DA, Cooper LAD. Structured crowdsourcing enables convolutional segmentation of histology images. Bioinformatics. 2019. doi: 10.1093/bioinformatics/btz083](https://academic.oup.com/bioinformatics/article/35/18/3461/5307750)_***

This data can be visualized in a public instance of the Digital Slide Archive at [this link](https://demo.kitware.com/histomicstk/histomicstk#?image=5bbdee62e629140048d01b0d&bounds=-42540%2C0%2C127990%2C84350%2C0). If you click the “eye” image icon in the Annotations panel on the right side of the screen, you’ll see the results of a collaborative annotation.

## Usage

- Each mask is a .png image, where pixel values encode region class membership. The meaning of ground truth encoded can be found at the file `./meta/gtruth_codes.tsv`.

- The name of each mask encodes all necessary information to extract the corresponding RGB images from TCGA slides. For convenience, RGBs are also downloaded using the code used here. 

- **[CRITICAL] -** Please be aware that zero pixels represent regions outside the region of interest (“don’t care” class) and should be assigned zero-weight during model training; they do **NOT** represent an “other” class.

- The RGBs and corresponding masks will be at the set `MPP` resolution. If `MPP` was set to `None`, then they
would be at `MAG` magnification. If both are set to `None`, then they will be at the base (scan) magnification.

## Reproducibility

To reproduce the accuracy results presented in the paper, please be sure to read the paper methodology, including the supplementary methods section. Some details are also re-iterated [here](https://bcsegmentation.grand-challenge.org/Baseline/) for convenience (including train-test splits and class grouping). [This Github repository](https://github.com/kheffah/tensorflow_FCN8Workflow) contains the code we used, and the trained VGG16-FCN8 tensorflow model weights can be downloaded at [this link](https://drive.google.com/drive/folders/1mSd3ZG1lnno_RuTHQXSU0GRAhtR21yIy?usp=sharing).


## Download (single link - convenient)  

You can use [this link](https://drive.google.com/drive/folders/1zqbdkQF8i5cEmZOGmbdQm-EP8dRYtvss?usp=sharing) to download the dataset at 0.25 MPP resolution.


## Download (command line - flexible)

Use this to download all elements of the dataset using the command line. 

This script will download any or all of the following:
- annotation JSON files (coordinates relative to WSI base resolution)
- masks
- RGB images

Steps are as follows:

**Step 0: Clone this repo**

```bash
$ git clone https://github.com/CancerDataScience/CrowdsourcingDataset-Amgadetal2019
$ cd CrowdsourcingDataset-Amgadetal2019
```

**Step 1: Instal requirements**
  
  `pip install girder_client girder-client pillow numpy scikit-image imageio`

**Step 2 (optional): Edit `configs.py`**
  
  If you like, you may edit various download configurations. Of note:
  
  - `SAVEPATH` - where everything will be saved
  - `MPP` - microns per pixel for RGBs and masks (preferred, default is 0.25)
  - `MAG` - magnification (overridden by `MPP` if `MPP` is set. default is None)
  - `PIPELINE` - what elements to download?

**Step 3: Run the download script**
  
  `python download_crowdsource_dataset.py`
  
  The script will create the following sub-directories in `SAVEPATH`: 
  
    |_ annotations : where JSON annotations will be saves for each slide
    |_ masks : where the ground truth masks to use for training and validation are saved
    |_ images: where RGB images corresponding to masks are saved
    |_ wsis (legacy) : Ignore this. No longer supported.
    |_ logs : in case anythign goes wrong
  

## Dataset Licensing
This dataset itself is licensed under a [CC0 1.0 Universal (CC0 1.0) license](https://creativecommons.org/publicdomain/zero/1.0/). 
We would appreciate it if you cite our paper if you use the data.

## Code licensing
Thise codebase is licensed with an MIT license.
