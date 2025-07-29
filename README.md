In this repository you can find instructions on how to use several end-to-end trained nnU-Net models for Liver segmentation.

Please cite the [following paper](https://doi.org/10.1038/s41598-025-07084-5) when using our model in your work:
```
Raab, F., Strotzer, Q., Stroszczynski, C. et al. Automatic segmentation of liver structures in multi-phase MRI using variants of nnU-Net and Swin UNETR. Sci Rep 15, 25740 (2025). 
```


The models were trained with four different test sets, therefore four different tasks were created (951, 952, 953, 954). Every task was trained in five fold cross-validation.

You can download the trained models (currently only standard nnU-Net, which performed best) with their .plans files under the link: https://drive.google.com/drive/folders/1bhR_L3J3bHA8jRcjVnhxTpUdZF2f4N_7?usp=share_link

**Setting up your machine (tested on two machines with A100s and H100s):**

**Conda:** Install a new environment with the following lines:

```
conda create -n nnunetv2_cu121 python=3.9 -y
conda activate nnunetv2_cu121

pip install torch==2.2.0 torchvision==0.17.0 torchaudio==2.2.0 --index-url https://download.pytorch.org/whl/cu121
pip install --no-deps git+https://github.com/MIC-DKFZ/acvl_utils.git@v0.2.4
pip install blosc2
pip install nnunetv2==2.2
pip install batchgenerators==0.25.1
pip install SimpleITK==2.3.1 scikit-image==0.24.0 connected-components-3d==3.24.0
```

After that, follow the instructions from https://github.com/MIC-DKFZ/nnUNet to set up your paths (nnunet_raw, nnunet_preprocessed, nnunet_results; either in your .bashrc file or just with terminal commands for the active session) for nnU-Net to work correctly. (Don't forget to source the bashfile, if you choose to set your paths there and activate your conda environment again)

Place the downloaded models inside the nnunet_results folder, defined by you. 
Structure should be like:   ~/nnunet_results/Dataset951...
                            ~/nnunet_results/Dataset952...
                            ...
                            ...

**Usage:**
As described in our publication, the models were trained on Gd-EOB-DTPA enhanced T1-VIBE sequences. (transversal acquisition)

The name convention of the different phases for training and evaluating the models was as follows:
native:           pat1_0000.nii.gz
arterial:         pat1_0001.nii.gz
late_arterial:    pat1_0002.nii.gz
portalvenous:     pat1_0003.nii.gz
HBP_20_phase:     pat1_0004.nii.gz

There, pat1 can be anything you want, but has to be consistent across the five corresponding phases.
Please follow this convention for inference in your datasets.

Have one folder, where your whole cohort is stored with the pre-described naming convention, so that the model knows, which images belong to the same patient.

As described in nnU-Nets repository, inference is done with the following command:

```
nnUNetv2_predict -i INPUT_FOLDER -o OUTPUT_FOLDER -d DATASET_NAME_OR_ID -c 3d_fullres --save_probabilities
```

Here, ```--save_probabilities``` is optional, if you intend to do ensembling of different predictions from the different Datasets models. This will make the command save the predicted probabilities alongside of the predicted segmentation masks, which requires a lot of disk space.
```DATASET_NAME_OR_ID``` is 951, 952, 953 or 954 for the models shared in this repository.




**
Docker: TO DO**
