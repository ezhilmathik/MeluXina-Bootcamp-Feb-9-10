## [How to login to MeluXina machine](https://docs.lxp.lu/first-steps/quick_start/)
- ### [Please take a look if you are using Windows](https://docs.lxp.lu/first-steps/connecting/)
- ### [Please take a look if you are using Linux/Mac](https://docs.lxp.lu/first-steps/connecting/)

## Use your username to connect to MeluXina
- ### For exmaple the below example shows the user of `u100490` 
  ```
  $ ssh u100490@login.lxp.lu -p 8822
  ```
## Once you have logged in
- ### Once you have logged in, you will be in a default home directory 
  ```
  [u100490@login02 ~]$ pwd
  /home/users/u100490
  ```
- ### After that go to project directory (Nvidia Bootcamp activites).
  ```
  [u100490@login02 ~]$ cd /project/home/p200117
  [u100490@login02 p200117]$ pwd
  /project/home/p200117
  ```
 - ## now copy climate.simg and climate.sh from project direcoty to your user (here is u100490) directory:
  ```
  [u100490@login02 p200117]$ cp /project/home/p200117/climate.simg /project/home/p200117/u100490
  [u100490@login02 p200117]$ cp /project/home/p200117/climate.sh /project/home/p200117/u100490
  [u100490@login02 p200117]$ cd u100490
  [u100490@login02 p200117]$ pwd
  [u100490@login02 p200117]$ /project/home/p200117/u100490
  [u100490@login02 p200117]$ ls -lthr
  total 7.2G
  -rwxrwx---. 1 u100490 p200117 7.2G Feb  3 14:53 climate.simg
  -rw-rwx---. 1 u100490 p200117  613 Feb  3 17:06 climate.sh
  ```
  
- ## For the dry run (9th February from 11:30-12:30), please follow the following steps:
  ```
  [u100490@login02 ~]$ salloc -A p200117 --res gpudev -q dev -N 1 -t 01:00:0
  [u100490@mel2123 p200117]$ mkdir -p $PROJECT/$USER/workspace-climate
  [u100490@mel2123 p200117]$ module load Singularity-CE/3.10.2-GCCcore-11.3.0
  
  [u100490@mel2123 p200117]$ singularity run --bind $PROJECT/$USER $PROJECT/climate.simg cp -rT /workspace $PROJECT/$USER/workspace-climate
  INFO:    Converting SIF file to temporary sandbox...
  INFO:    Cleaning up image...
  [u100490@mel2123 p200117]$ singularity run --nv --bind $PROJECT/$USER $PROJECT/climate.simg jupyter lab --notebook-dir=$PROJECT/$USER/workspace-climate/python/jupyter_notebook --port=8888 --ip=0.0.0.0 --no-browser --NotebookApp.token=""
  INFO:    Converting SIF file to temporary sandbox...
  WARNING: underlay of /usr/bin/nvidia-smi required more than 50 (452) bind mounts
  [W 10:10:32.723 LabApp] All authentication is disabled.  Anyone who can connect to this server will be able to run code.
  [I 10:10:33.043 LabApp] jupyter_tensorboard extension loaded.
  [I 10:10:33.047 LabApp] JupyterLab extension loaded from /usr/local/lib/python3.8/dist-packages/jupyterlab
  [I 10:10:33.047 LabApp] JupyterLab application directory is /usr/local/share/jupyter/lab
  [I 10:10:33.048 LabApp] [Jupytext Server Extension] NotebookApp.contents_manager_class is (a subclass of) jupytext.TextFileContentsManager already - OK
  [I 10:10:33.048 LabApp] Serving notebooks from local directory: /mnt/tier2/project/p200117/u100490/workspace-climate/python/jupyter_notebook
  [I 10:10:33.048 LabApp] Jupyter Notebook 6.2.0 is running at:
  [I 10:10:33.048 LabApp] http://hostname:8888/
  [I 10:10:33.049 LabApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
  ```
  - ## Now open a new terminal on your local computer, and again login to MeluXina to access the port
  - ## Make sure you use the name NODELIST (here it is mel2123 - from `squeue` command you will get this number)
  ```
  ssh -L8080:NODELIST:8888 USERNAME@login.lxp.lu -p 8822
  ssh -L8080:mel2123:8888 u100490@login.lxp.lu -p 8822
  ```
  - ## Keep those terminals open/alive (please do not close them)
  - ## Now copy and paste localhost to your browser either to Chrome or FireFox
  ```
  http://localhost:8080
  ```
  
- ## For the afternoon session (9th and 10th February)
- ## Now it is time to edit your batch script (climate.sh) before launhing your Jupyter notebook, please follow the following steps: 
  ```
  [u100490@login02 p200117]$ emacs(emacs -nw)/vim climate.sh
  #!/bin/bash -l
  #SBATCH --partition=gpu 
  #SBATCH --ntasks=1
  #SBATCH --nodes=1    
  ############  day one ##########
  #SBATCH --time=02:00:00         ## use this option for day one
  #SBATCH --res ai_bootcamp_day1   ## use this option for day one
  ################################
  
  ############  day two ##########
  ######SBATCH --time=04:00:00         ## use this option for day two
  ######SBATCH --res ai_bootcamp_day2  ## use this option for day two
  ################################
  #SBATCH -A p200117
  #SBATCH --qos default

  mkdir -p $PROJECT/$USER/workspace-climate
  module load Singularity-CE/3.10.2-GCCcore-11.3.0

  singularity run --bind $PROJECT/$USER $PROJECT/climate.simg cp -rT /workspace $PROJECT/$USER/workspace-climate
  singularity run --nv --bind $PROJECT/$USER $PROJECT/climate.simg jupyter lab --notebook-dir=$PROJECT/$USER/workspace-climate/python/jupyter_notebook  --port=8888 --ip=0.0.0.0 --no-browser --NotebookApp.token=""
  ```
- ## Once you have modified your climate.sh, please launch your batch script as below:
  ```
  [u100490@login03 p200117]$ sbatch climate.sh
  Submitted batch job 276009
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
- ## Now open a new terminal on your local computer, and again login to MeluXina to access the port
- ## Make sure you use the name NODELIST (here it is mel2077 - from `squeue` command you will get this number)
  ```
  ssh -L8080:NODELIST:8888 USERNAME@login.lxp.lu -p 8822
  ssh -L8080:mel2077:8888 u100490@login.lxp.lu -p 8822
  ```
- ## Keep those terminals open/alive (please do not close them)
- ## Now copy and paste localhost to your browser either to Chrome or FireFox
  ```
  http://localhost:8080
  ```
