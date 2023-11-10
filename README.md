## Step 1: Installing dlib:
-download zip from https://github.com/Murtaza-Saeed/dlib
-go to cmd then type : pip install dlib-19.24.1-cp311-cp311-win_amd64.whl (if you have have python 3.11 depend cp311)
					   pip install dlib-19.22.1-cp39-cp39-win_amd64.whl (if you have have python 3.9 depend cp39)
## Step 2: Donwload dat 
-https://drive.google.com/file/d/1CYMKQn5cy9Fg-lbOI0Sp2lUl5k8gdXrI/view?usp=sharing
-copy it in the same folder

### Step 3: 
Install all the system requirments by:
```bash 
pip install -r requirements.txt
```
### Step 4: 
After the system has been setup. Run the command: 
```bash 
python app1.py
```
## Working Details: 

The basic thing about drowsiness detection is pretty simple. We first detect a face using dlib's frontal face detector. Once the face is detected , we try to detect the facial landmarks in the face using the dlib's landmark predictor. The landmark predictor returns 68 (x, y) coordinates representing different regions of the face, namely - mouth, left eyebrow, right eyebrow, right eye, left eye, nose and jaw. Ofcourse, we don't need all the landmarks, here we need to extract only the eye and the mouth region. 

Now, after extraxting the landmarks we calculate the <b>Eye Aspect Ratio (EAR)</b> as: 

```python 
def eye_aspect_ratio(eye):
	# Vertical eye landmarks
	A = dist.euclidean(eye[1], eye[5])
	B = dist.euclidean(eye[2], eye[4])
	# Horizontal eye landmarks 
	C = dist.euclidean(eye[0], eye[3])

	# The EAR Equation 
	EAR = (A + B) / (2.0 * C)
	return EAR
```
The eye region is marked by 6 coordinates. These coordinates can be used to find whether the eye is open or closed if the value of EAR is checked with a certain threshold value.<br>
![blink_detection_plot](https://user-images.githubusercontent.com/35571958/87878670-62d41400-ca03-11ea-8b96-fc4344c61a21.jpg)

In the same way I have calculated the aspect ratio for the mouth to detect if a person is yawning. Although, there is no specific metric for calculating this, so I have taken for points, 2 each from the upper and lower lip and calculated the mean distance between them as: 
```python 
def mouth_aspect_ratio(mouth): 
	A = dist.euclidean(mouth[13], mouth[19])
	B = dist.euclidean(mouth[14], mouth[18])
	C = dist.euclidean(mouth[15], mouth[17])

	MAR = (A + B + C) / 3.0
	return MAR
```
<b>Note: Learn more about dlib</b> <a href = "http://dlib.net/">here.</a>

## Results: 
The GUI has been created using basic HTML, CSS and JavaScript and we have used Flask to render the python code into the website. Tkinter has also been used in order to make things simpler. It has 2 buttons: Run and Exit. The GUI looks like: 
![df01ae7c-afc9-4676-b95b-b6cec592ddf0 (online-video-cutter com) (1)](https://user-images.githubusercontent.com/35571958/87902089-589f2d80-ca76-11ea-9eda-a53a83662721.gif)

The outputs of the working system detecting drowsiness is shown as: <br>
![frame_yawn1](https://user-images.githubusercontent.com/35571958/87904322-ab2f1880-ca7b-11ea-97d2-82f9dd0c318a.jpg) ![Screenshot (405)](https://user-images.githubusercontent.com/35571958/87904406-dd407a80-ca7b-11ea-982d-1852e2228765.png)

Also, in order to keep a proof of the moment when the person was sleeping or yawning, we kept a seperate folder where those frames are stored as: <br>
![Screenshot (408)](https://user-images.githubusercontent.com/35571958/87904688-7e2f3580-ca7c-11ea-839b-c049bace332f.png)

## References: 
[1]Facial landmarks with dlib, OpenCV and Python: https://www.pyimagesearch.com/2017/04/03/facial-landmarks-dlib-opencv-python/ <br>
[2]Eye blink detection with OpenCV, Python, and dlib: https://www.pyimagesearch.com/2017/04/24/eye-blink-detection-opencv-python-dlib/ <br>
[3]Drowsiness Detection with OpenCV: https://www.pyimagesearch.com/2017/05/08/drowsiness-detection-opencv/ <br>
[4]Real-Time Eye Blink Detection using Facial Landmarks: http://vision.fe.uni-lj.si/cvww2016/proceedings/papers/05.pdf 
