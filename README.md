# Multimodal learning

How to use this repository:
1) Extract optical flows from the video
2) Create data blobs
2) Train a model 
3) Evaluate the trained model and get different results including U-map plots, gesture classification, skill classification, task classification

Even if you use a remote machine for the training and evaluation, it is recommended that you use a local machine to do steps 1 and 2 so you can inspect the data preprocessing steps and they are both CPU only jobs. The main steps are each a mode in main.py and we will be using main.py to call them. It is recommended to read through main.py and see what options there are and what default parameters you may be using to call each step. We are only setting the path parameters below, not changing the default scaling and other frame counts so use the default parameters in your analysis. 

### To install dependencies, please run

<code>pip install sklearn opencv-python tqdm pandas xgboost umap seaborn multipledispatch barbar</code>

### To generate the optical flow, run the following command. You may need to adjust the path for where you downloaded your JIGSAWS directory. This call will perform two steps. The first step will resize the video and the secodn will generate the optical flow. 

<code>python3 main.py --mode 'optical_flow' --source_directory ../JIGSAWS/Suturing/video/ --resized_video_directory ../JIGSAWS/Suturing/resized_video --destination_directory ../JIGSAWS/Suturing/optical_flow  </code> 

Make sure the code started running OK (it should say 'Rescaling videos; Processing video Suturing_B001_capture1.avi; 100; 200;...') then go get some coffee, take a nap. The "generating optical flow" step can take a while. In the mean time, once the resizing step is done, you can check that you have the same number of videos in the resized_video folder as the original video directory and you can play the videos (they should be smaller than the original though). For reference, on my 6-years-old desktop that had a pretty good CPU for when it came out, the resizing took about 3 min and the optical flow generation took about 1 h 20 min. 

### To generate prepare the data for training, you need to generate 'data_blobs'. Run the following code:  

<code>python3 main.py --mode 'data_blob' --optical_flow_path ../JIGSAWS/Suturing/optical_flow --transcriptions_path ../JIGSAWS/Suturing/transcriptions --kinematics_path ../JIGSAWS/Suturing/kinematics --blobs_folder_path ../JIGSAWS/Suturing/blobs </code> 

### To generate prepare the data for training, you need to generate 'data_blobs'. Run the following code:  

<code>python3 main.py --mode 'data_blob' --optical_flow_folder_path ../JIGSAWS/Suturing/optical_flow --transcriptions_folder_path ../JIGSAWS/Suturing/transcriptions --kinematics_folder_path ../JIGSAWS/Suturing/kinematics --blobs_save_folder_path ../JIGSAWS/Suturing/blobs </code> 

### To train the network using your data_blobs. Run the following code:  

<code>python3 main.py --mode 'train' --blobs_folder_path ../JIGSAWS/Suturing/blobs --weights_save_path model </code> 

### Come up with your own command for evaluation. Look at the parameters that are required in main.py (lines 144-162) and see what needs to be filled in, and what parameters are used by default. 


The paper associated with this repository can be found at https://link.springer.com/article/10.1007/s11548-021-02343-y. The citation details are as follows.

@article{wu2021cross,
  title={Cross-modal self-supervised representation learning for gesture and skill recognition in robotic surgery},
  author={Wu, Jie Ying and Tamhane, Aniruddha and Kazanzides, Peter and Unberath, Mathias},
  journal={International Journal of Computer Assisted Radiology and Surgery},
  volume={16},
  number={5},
  pages={779--787},
  year={2021},
  publisher={Springer}
}

