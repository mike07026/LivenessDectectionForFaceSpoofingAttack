# LivenessDectectionForFaceSpoofingAttack

### This is a project designed to handle face spoofing attack in the face recognition camera. (on going)

This project is designed to distinguish between real person and fake images or videos shows up in front of the security camera.

This project contains a SVM models trained on Rotation Invariant Uniform LBP histogram. The historgram vector is extraced from the DOG(difference of gaussian iamge) on all channels of HSV and YCbCr colorspaces. 

#### Usage:
#####C++ program:
dependency: opencv(with HDF5 module support)
generates the train and test data stored in two HDF5 files, and train a SVM model then output the SVM model accuracy.
(The generated HDF5 files can be used to generate a MLP model by python in the /src/python/MLP folder)

```
$ cd {project root}/src
$ make
$ cd {project root}/bin    
$ ./main -d <path-to-the-data-root-directory>  
```
[note: data root directory should contain two folders: train and test.]
Then you should see generated data files and one model file. Accuracy and detial will show in the command window. 

#####Python program:
dependency: python-opencv, h5py, caffe
```
$ cd {project root}/src/python/MLP
$ python main.py -e <number of desired epochs> 
```
This will train (test while traininig) a MLP model on the same HDF5 data generated in the /bin directory by the C++ program. User can specify their own data by using other input arguments. See detail usage in main.py
[note: Please be carefull that the train and test data format has to match the MLP model definition, which is configured in the MLP.py file]

```
$ python inference.py -m <model path> -c<checkpoint file path> OPTIONAL[-d <path to the hdf5 file>]
```
This will run the test on the whole data set (default 'test.h5' in /bin) after the MLP model training finished.


#### Note:
  1. Currently, test and train are integrated in main.cpp and main.py
  2. Current version performs DOG on all colorspace channels followed by LBP feature extraction on all channels.
  3. Best accuracy can be achieve by trainig MLP model. (around 97%) 

---------------------
update 10/09/2018:
  1. Optimized the baisc LBP image calculation.
  2. Add wrapper for all 3 modes of LBP feature extraction and reorganized the class structure.
  
update 10/11/2018:
  1. Added difference of gaussian on each colorspace channel before the LBP histogram feature vector extraction. Now the accuracy is 87.47% on the current dataset.
  2. Current configuration has image resize to 96x96 and LBP cellsize of 16.

update 10/12/2018:
  1. Fixed bug in calculating uniform LBP pattern.
  2. Added test only option in main.cpp
  
update 10/15/2018:
  1. Added option for export processed train/test set into txt files.
  2. Added generic python util for generating autoencoder on given feature vectors.
  3. Start working on generic python MLP model generation. 

update 10/17/2018:
  1. Now data can be written into HDF5 file directly after processing.
  2. Python MLP training can directly proceed from the C++ generated HDF5 file.

update 10/18/2018:
  1. generated a deploy model for single inference purpose. 
  2. added a inference script to run the test on whole test set after training the MLP.
