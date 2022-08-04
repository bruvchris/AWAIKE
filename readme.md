# AWAIKE

### Overview
AWAIKE is a machine learning model to detect driver drowsiness using computer vision. The minimum viable product is to detect the driver's face and determine their status from awake, drowsy, or asleep. We have designed this project to improve and ensure better safety on the road for both drivers and pedestrians. 

### Dataset

Our dataset utilizes about 818 total images, but is more than doubled to 1776 images after data augmentation. Several types of image preprocessing was done to improve model performance, such as brightness adjustments to improve its robustness during both day and night. The images include several variations of people driving, while others are simply faces. The images were manually annotated with bounding boxes by our labeling team. 

### Training

The model was trained using YOLOv5s in Google Colabs. The images were input at a size of 416x416 and ran for 174 epochs with a batch size of 16. The weights were initialized with the pretrained COCO checkpoint. The confusion matrix can be seen below:

![confusion matrix](readme_img/awaikeconfusionmatrix.PNG?raw=true "Title")

### Results

The following are examples of the three classes that our model detects with bounding boxes and confidence levels:

![asleep](readme_img/asleep.jpg?raw=true "Title")
![awake](readme_img/awake.jpg?raw=true "Title")
![drowsy](readme_img/drowsy.jpg?raw=true "Title")
![null](readme_img/null.jpg?raw=true "Title")

### Deployment

First clone this repository through 

`https://github.com/organization-x/omni`

cd into the `/app` folder

`python3 -m pip install -r requirements.txt`

edit line 29 the `main.py` file to either the URL of the cocalc server you are on or `localhost` if you are running it on your own PC

Then, clone ultralytics yolov5 in the app folder, by running 

`git clone https://github.com/ultralytics/yolov5`
`pip install -r yolov5/requirements.txt`

Run

 `python3 -m main`

to start the server on local, most changes while developing will be picked up in realtime by the server

### Quickstart Guide for Local Deployment

Make sure docker is installed on your system. 

cd into the root director of the repo then run 

`docker build -t omni .`

once built, run

`docker run -d -p 9000:80 --restart=unless-stopped --name omni omni`

you should then be able to see the `omni` container running when you run 

`docker ps -a`

if it seems to be stuck (i.e. constantly listed as `Restarting`), something is wrong with the docker image or code inside causing it to repeatedly fail.

you can start debugging the project by running 

`docker logs -f omni` 

or

`docker exec -it omni /bin/bash` for an interactive bash terminal (this option only works if the container is running and not stuck in a restart loop)

### Common Issues

`$'\r': command not found` when attempting to start docker container

this is caused by the the `entrypoint.sh` script somehow having CLRF line endings instead of LF line endings.

to fix this run

`sed -i 's/\r$//' entrypoint.sh`

### File Structure
The editable files/directories are **bolded**

**DO NOT TOUCH OTHER FILES. THIS MAY RESULT IN YOUR PROJECT BEING UNABLE TO RUN**

- .gitignore
- config.py
- Dockerfile
- READMD.md
- entrypoint.sh
- nginx_host
- host_config
- app/
     - **main.py**
     - **best.pt** 
     - **requirements.txt**
     - **utils.py**
     - templates/
     - **index.html**

### best.pt ###
The weights file.
### main.py ###
Contains the main flask app itself.
### requirements.txt ###
Contains list of packages and modules required to run the flask app. Edit only if you are using additional packages that need to be pip installed in order to run the project.

To generate a requirements.txt file you can run

`pip list --format=freeze > app/requirements.txt`

the requirements.txt file will then be updated. Keep in mind: some packages you install on one operating system may not be available on another. You will have to debug and resolve this yourself if this is the case.
### static/ ###
Contains the static images, CSS, & JS files used by the flask app for the webpage. 
### utils.py ###
Contains common functions used by the flask app. Put things here that are used more than once in the flask app.
### templates/ ###
Contains the HTML pages used for the webpage, index.html is the demo page.
### Files used for deployment ###
`config.py`
`Dockerfile`
`entrypoint.sh`
`nginx_host`
`host_config`
**Only modify `host_config`. Do not touch the other files.**

### Team

- Haziq Rahat
- Chris Dugan
- Adrian Escobar
- Samantha Manuel
- Praneeth Muvva
- Dylan Lim
- Santosh Patapati
