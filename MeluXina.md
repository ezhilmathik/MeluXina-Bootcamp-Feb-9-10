### [How to login to MeluXina machine](https://docs.lxp.lu/first-steps/quick_start/)
- ## Please take a look if you are using Windows
- ## Please take a look if you are using Mac
- ## Please take a look if you are using Linux

## Use user account to connect to MeluXina
- ### For exmaple the below example shows the user of `u100490` 
  ```sh
  ssh u100490@login.lxp.lu -p 8822
  ```
## Once you have logged in
- ### Once you have logged in you will be in a default home directory 
  ```
  [u100490@login02 ~]$ pwd
  /home/users/u100490
  ```
- ### After that go to project directory (Nvidia Bootcamp activites)
  ```
  [u100490@login02 ~]$ cd /project/home/p200117
  [u100490@login02 p200117]$ pwd
  /project/home/p200117
  ```
- ### Once you are in the project dicretory, please copy climage.simg and climate.sh
  ``` 
  [u100490@login02 p200117]$ cp /tmp/climate.simg .
  [u100490@login02 p200117]$ cp /tmp/climate.sh .
  ```
- ## Now it is time to launch the your Jupyter notebook 
  ```
  [u100490@login03 p200117]$ sbatch climate.sh
  [u100490@login03 p200117]$ squeue 
  JOBID PARTITION     NAME     USER    ACCOUNT    STATE       TIME   TIME_LIMIT  NODES NODELIST(REASON)
  276009       gpu climate.  u100490    p200117  RUNNING       0:16        20:00      1 mel2077
  ```
- ## Now you have initiated your singularity container which will help you to open the Jupyter nootebook
  ```
  [u100490@login03 p200117]$ ls -lthr
  total 7.2G
  -rwxr-x---. 1 u100490 p200117 7.2G Feb  3 14:53 climate.simg
  -rw-r-----. 1 u100490 p200117  613 Feb  3 17:06 climate.sh
  drwxr-s---. 3 u100490 p200117 4.0K Feb  3 17:09 u100490
  -rw-r--r--. 1 u100490 p200117 1.1K Feb  3 17:58 slurm-276009.out
  ```
- ## Now we need to get the port that can be used to open from your browser, for that do the following steps:
  ```
  [u100490@login03 p200117]$ head -30 slurm-276009.out 
  INFO:    Converting SIF file to temporary sandbox...
  INFO:    Cleaning up image...
  INFO:    Converting SIF file to temporary sandbox...
  WARNING: underlay of /usr/bin/nvidia-smi required more than 50 (452) bind mounts
  [W 17:58:37.489 LabApp] All authentication is disabled.  Anyone who can connect to this server will be able to run code.
  [I 17:58:37.807 LabApp] jupyter_tensorboard extension loaded.
  [I 17:58:37.811 LabApp] JupyterLab extension loaded from /usr/local/lib/python3.8/dist-packages/jupyterlab
  [I 17:58:37.811 LabApp] JupyterLab application directory is /usr/local/share/jupyter/lab
  [I 17:58:37.813 LabApp] [Jupytext Server Extension] NotebookApp.contents_manager_class is (a subclass of) jupytext.TextFileContentsManager already - OK
  [I 17:58:37.813 LabApp] Serving notebooks from local directory: /mnt/tier2/project/p200117/u100490/workspace-climate/python/jupyter_notebook
  [I 17:58:37.813 LabApp] Jupyter Notebook 6.2.0 is running at:
  [I 17:58:37.813 LabApp] http://hostname:8888/
  [I 17:58:37.813 LabApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
  ```
- ## Now open a new terminal on your local computer, and login again to MeluXina to access the port
- ## Make sure you use the name NODELIST (here is is mel2077)
  ```
    ssh -L8080:NODELIST:8888 USERNAME@login.lxp.lu -p 8822
  ssh -L8080:mel2077:8888 u100490@login.lxp.lu -p 8822
  ```
- ## Now copy and paste localhost to your browser either Chrome or FireFox
  ```
  http://localhost:8080
  ```
