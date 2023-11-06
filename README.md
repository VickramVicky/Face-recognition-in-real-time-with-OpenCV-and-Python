# Face-recognition-in-real-time-with-OpenCV-and-Python

# What is face recognition?
With face recognition, we not only identify the person by drawing a box on his face but we  also know how to give a precise name. With OpenCV and Python, through a database, we  compare the person’s photo and we know how to identify it precisely.

# 1. Installations
For convenience, I invite you to download the package with all the codes and photos that you will find in my lesson, and then we proceed with the installation of the basic libraries.The first library to install is opencv-python, as always run the command from the terminal.
• pip install opencv-python
then proceed with face_recognition, this too installs with pip.
• pip install face_recognition

# 2. Face recognition on image
To make face recognition work, we need to have a dataset of photos also composed of a single image per character and comparison photo. For example, in our example, we have a dataset consisting of 1 photo each of Elon Musk, Jeff Bezos, Lionel Messi, Ryan Reynolds, Vickram and in the comparison, we will use the photo of Vickram

# Call the libraries
The first step is always to recall the libraries we have 
installed OpenCV and face_recognition in our project.

import cv2
import face_recognition

# Face encoding first image
With the usual OpenCV procedure, we extract the image, in this case, Messi1.webp, and convert it into RGB color format. Then we do the “face encoding” with the functions of the Face recognition library.

img = cv2.imread(r"C:\Users\vickr\PycharmProjects\FaceRecognition\Vicky.jpg")
rgb_img = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)
img_encoding = face_recognition.face_encodings(rgb_img)[0]

# Face encoding second image
Same procedure for the second image, we only change the name of the variables and obviously the path of the second image, in this case: r"C:\Users\vickr\PycharmProjects\FaceRecognition\images\Vickram.JPG"

img2 = cv2.imread(r"C:\Users\vickr\PycharmProjects\FaceRecognition\images\Vickram.JPG")
rgb_img2 = cv2.cvtColor(img2, cv2.COLOR_BGR2RGB)
img_encoding2 = face_recognition.face_encodings(rgb_img2)[0]

# Comparison of images
With a single line, we make a simple face comparison and print the result. If the images are the same it will print True otherwise False. 

result = face_recognition.compare_faces([img_encoding], img_encoding2)
print("Result: ", result)

# Encode all face in the dataset
Now we have to encode all the images in our database, so that through the webcam video stream if it finds the match it shows the name otherwise it says “name not found”.This is a function of the file I have prepared and it simply takes all the images contained in the images/ folder and encodes them. In our case, there are 5 images.

# Encode faces from a folder

sfr = SimpleFacerec()
sfr.load_encoding_images(r"C:\Users\vickr\PycharmProjects\FaceRecognition\images")
SimpleFacerec()

# Constructor(‘__init__’):
It is a python function to do the following process
• Initializes two empty lists which are ‘known_face_encodings’ and ‘known_face_names’
• Sets the ‘frame_resizing’ attribute to 0.25, which reduces the frame size for faster processing.

#‘load_encoding_images’ method:
• Loads encoding images from a specified directory (images_path) containing image files.
• It uses glob to retrieve a list of image file paths in the specified directory.
• For each image, it reads the image with OpenCV and converts it from BGR to RGB color format.
• It extracts the filename (without the extension) to use as the face name.
• It uses face_recognition to compute the face encoding for the image and appends it to known_face_encodings.
• It also appends the corresponding face name to known_face_names.‘detect_known_faces’ method:
• Takes an input frame (image) as an argument.
• Resizes the frame according to the frame_resizing attribute to optimize face recognition.
• Converts the resized frame from BGR to RGB color format.
• Uses face_recognition to detect face locations and compute face encodings in the frame.
• Compares the computed face encodings with the known face encodings stored in the known_face_encodings list.
• For each detected face, it identifies the best match among known faces based on the smallest distance between face encodings.
• Returns a list of face locations and a list of corresponding face names for the detected faces.

# 3. Face recognition in real-time on a webcam
Take webcam stream
With a simple Opencv function, We take the webcam stream and loop it

# Load Camera
cap = cv2.VideoCapture(2)
while True:
ret, frame = cap.read()
Face location and face recognition
Now we identify the face passing the frame of the webcam to this 
function detect_known_faces(frame). It will give us the name of the person and an array 
with the position at each moment of the movement.

# Detect Faces
face_locations, face_names = sfr.detect_known_faces(frame)
for face_loc, name in zip(face_locations, face_names):
y1, x2, y2, x1 = face_loc[0], face_loc[1], face_loc[2], face_loc[3]
Show name and rectangle
Now that we have all the elements we show them with OpenCV. 
cv2.putText(frame, name,(x1, y1 - 10), cv2.FONT_HERSHEY_DUPLEX, 1, (0, 0, 200), 2)
cv2.rectangle(frame, (x1, y1), (x2, y2), (0, 0, 200), 4)
cv2.imshow("Frame", frame)
key = cv2.waitKey(1)
if key == 27:
break
