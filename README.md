# Color-Detection-Using-Python-and-OpenCV
Requirements

	1.	Python 3.x: Make sure you have Python installed on your system.
	2.	OpenCV: A library for image and video processing.
	3.	NumPy: A library for numerical computations.

Step 1: Install Required Libraries

Before running the script, ensure you have the necessary libraries installed. Open the terminal or command prompt and run:

pip install opencv-python numpy

Step 2: Create the Python Script

Create a new Python file (e.g., color_detection.py) and insert the following code:

import cv2
import numpy as np

# Open the webcam
cap = cv2.VideoCapture(0)

if not cap.isOpened():
    print("Error: Unable to access the camera!")
    exit()

# Define color ranges in HSV
lower_red = np.array([0, 120, 70])
upper_red = np.array([10, 255, 255])

lower_blue = np.array([100, 150, 0])
upper_blue = np.array([140, 255, 255])

lower_green = np.array([40, 40, 40])
upper_green = np.array([70, 255, 255])

while True:
    ret, frame = cap.read()

    if not ret:
        print("Error: Unable to read the frame!")
        break

    # Convert frame to HSV color space
    hsv_frame = cv2.cvtColor(frame, cv2.COLOR_BGR2HSV)

    # Create masks for each color
    mask_red = cv2.inRange(hsv_frame, lower_red, upper_red)
    mask_blue = cv2.inRange(hsv_frame, lower_blue, upper_blue)
    mask_green = cv2.inRange(hsv_frame, lower_green, upper_green)

    # Apply masks to extract colors
    result_red = cv2.bitwise_and(frame, frame, mask=mask_red)
    result_blue = cv2.bitwise_and(frame, frame, mask=mask_blue)
    result_green = cv2.bitwise_and(frame, frame, mask=mask_green)

    # Count non-zero pixels to determine detected color
    red_count = cv2.countNonZero(mask_red)
    blue_count = cv2.countNonZero(mask_blue)
    green_count = cv2.countNonZero(mask_green)

    detected_color = "No Color"
    if red_count > 500:
        detected_color = "Red"
    elif blue_count > 500:
        detected_color = "Blue"
    elif green_count > 500:
        detected_color = "Green"

    # Display detected color on the frame
    cv2.putText(frame, f"Detected Color: {detected_color}", (10, 50), cv2.FONT_HERSHEY_SIMPLEX, 1, (255, 255, 255), 2)

    # Show the webcam feed with color detection
    cv2.imshow('Webcam View', frame)

    # Press 'q' to exit
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break

# Release camera and close windows
cap.release()
cv2.destroyAllWindows()

Step 3: Explanation of the Code

1. Opening the Camera

	•	cv2.VideoCapture(0) initializes the webcam.
	•	If the camera cannot be accessed, it prints an error message and exits.

2. Defining Color Ranges in HSV

	•	Colors are defined in the HSV (Hue, Saturation, Value) color space for better accuracy.
	•	The lower and upper HSV values for red, blue, and green are specified.

3. Converting the Frame to HSV

	•	The captured frame is converted from BGR (default in OpenCV) to HSV using:

hsv_frame = cv2.cvtColor(frame, cv2.COLOR_BGR2HSV)



4. Creating Masks for Each Color

	•	cv2.inRange() creates a binary mask where the pixels within the specified range are white, and others are black.
	•	Three masks are created for red, blue, and green colors.

5. Applying the Masks

	•	The bitwise AND operation extracts the color from the original frame.

6. Counting Non-Zero Pixels

	•	cv2.countNonZero() counts the number of pixels that match each color mask.
	•	If the count exceeds 500 pixels, it assumes that color is detected.

7. Displaying the Detected Color

	•	cv2.putText() overlays the detected color name on the frame.

8. Exiting the Program

	•	Press ‘q’ to exit the webcam feed and close the program.

Step 4: Running the Script

	•	Save the file as color_detection.py.
	•	Open a terminal in the same directory and run:

python color_detection.py


	•	The webcam will open and detect colors in real time.

Controls:

	•	The detected color will be displayed on the screen.
	•	Press ‘q’ to exit the program.

Step 5: Notes

	1.	Ensure OpenCV is installed before running the script.
	2.	Adjust color ranges if detection is inaccurate.
	3.	Ensure your webcam is connected and working.

Conclusion

You have successfully built a real-time color detection system using Python and OpenCV! This code can be improved further by adding more colors or optimizing detection accuracy.
