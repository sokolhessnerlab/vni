# VNI fMRI data set-up and quality check 
-	Goal: make data BIDS compliant and visually inspect T1w and functional runs 	
- 	This was originally done on HPC. We moved everything to RDAC after dataset was BIDS compliant 

## STEP 1: Create directory for participant’s BIDS compliant data
1. cd /data/psychology/sokol-hessnerlab/VNI/scripts
2. ./makeSubFolderBIDS subID (subID is 3 digits, e.g. 001)
3. In vniBIDS, there will be a subject specific directory with 3 sub-directories:
•	sub-001/{anat, fmap, func}

## STEP 2: Copy (do not move) the raw (DICOM) data into the subject directory. For VNI, this data is located in /data/psychology/sokol-hessnerlab/VNI/sourceData/M803#####. The M803 numbers were generated by the COINS database used by CINC.
1. In sub-###/anat, copy the structural folder 
    * t1w_32ch_mpr_p3_08mm_0005
2. In sub-###/fmap, copy the distortion correction folders
    * distortion_corr_32ch_ap_006
    * distortion_corr_32ch_ap_polarity_invert_pa_0007
3. In sub-###/func, copy the 3 functional and 3 SBRef folders
    * vni_ap_32ch_mb8_v01_r01_0009
    * vni_ap_32ch_mb8_v01_r01_0011
    * vni_ap_32ch_mb8_v01_r01_0013
    * vni_ap_32ch_mb8_v01_r01_SBRef_0008
    * vni_ap_32ch_mb8_v01_r01_SBRef_0010
    * vni_ap_32ch_mb8_v01_r01_SBRef_0012

## STEP 3: convert DICOM to NIFTI (~5 minutes for 1 participant)
1. cd /data/psychology/sokol-hessnerlab/VNI/scripts
2. ./dcm2niftiBatch SubID

Now all raw files in anat, fmap, func directories are in NIFTI + BIDS compliant format and the raw DICOMs have been removed.


## STEP 4: quality check (notes made in vniQCnotes.xlsx)
(this part has not been updated for RDAC where loading FSL will be a little different)
1.	First, make sure that the dcm2niix process occurred with no errors. 
•	Open a new terminal window, load FSL, then change the directory to the functional folder of the participant you are checking: cd /data/psychology/sokol-hessnerlab/VNI/vniBIDS/sub-###/func. 
•	Type ‘fslinfo sub-###_task-run#_bold.nii.gz’ (change the sub ID and the run number)
•	There should be the number ‘1608’ next to the dim4 output (implying there are 1608 volumes). If there is a number other than 1608, then go back to the raw dicom file to check that there are 1608 files in each of the raw data folders for each run. If there are 1608 dicom files then its probably an issue with dcm2niix. Try running it again. 
2.	Second, load the structural (T1) scan into the FSL eyes and click around to examine the structural image and make note of any artifacts.
3.	Third, load a single functional run into FSL (it be much slower if you load all 3) and then turn on the ‘movie mode’. You may need to go to the settings to unclick the ‘synchronize movie mode’ and to increase the speed at which the movie is played. Make note of any abrupt movements or anything that looks problematic.

