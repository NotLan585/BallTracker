Red Ball Tracking using OpenCV

This project uses OpenCV and NumPy to detect and track a red ball in real-time using a webcam. The program captures video, converts it to HSV color space, applies a mask for red color detection, and then uses contour detection to locate and highlight the red ball.

📌 Requirements

Make sure you have the following dependencies installed:

pip install opencv-python numpy

🚀 How It Works
1.	Captures Video Feed:
•	Uses cv2.VideoCapture(0) to capture live video from the default webcam.
2.	Converts Frame to HSV:
•	The BGR frame is converted to HSV color space for better color segmentation.
3.	Applies Color Mask:
•	A mask is created to detect red objects within a specified HSV range.
4.	Finds Contours & Draws Circle:
•	Contours are detected around red objects.
•	If a detected object is large enough, a circle is drawn around it.
5.	Displays Results:
•	Shows both the original frame with tracking and the binary mask.
6.	Exit Condition:
•	Press ‘q’ to stop the program and close all windows.

📝 Code

import cv2
import numpy as np

cap = cv2.VideoCapture(0)

upper_red2 = np.array([180, 255, 255], dtype=np.uint8)
lower_red2 = np.array([170, 120, 70], dtype=np.uint8)

while True:
ret, frame = cap.read()
if not ret:
break

    hsv = cv2.cvtColor(frame, cv2.COLOR_BGR2HSV)

    mask_red = cv2.inRange(hsv, lower_red2, upper_red2)

    mask_red = cv2.morphologyEx(mask_red, cv2.MORPH_OPEN, np.ones((5, 5), np.uint8))

    contours, _ = cv2.findContours(mask_red, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)

    for contour in contours:
        if cv2.contourArea(contour) > 500:
            (x, y), radius = cv2.minEnclosingCircle(contour)
            center = (int(x), int(y))
            radius = int(radius)

            cv2.circle(frame, center, radius, (0, 0, 255), 2)

            cv2.putText(frame, "Red Ball", (center[0] - 20, center[1] - 10),
                        cv2.FONT_HERSHEY_SIMPLEX, 0.5, (0, 0, 0), 2)

    cv2.imshow("Red Ball Tracking", frame)
    cv2.imshow("Mask", mask_red)

    if cv2.waitKey(1) & 0xFF == ord('q'):
        break

cap.release()
cv2.destroyAllWindows()

🎯 How to Run
1.	Save the script as red_ball_tracking.py.
2.	Run the script using:

python red_ball_tracking.py


	3.	Make sure your webcam is connected!
	4.	Press ‘q’ to exit the program.

🔧 Customization
•	Adjust the HSV values in lower_red2 and upper_red2 to fine-tune red detection.
•	Modify the contour area threshold (500) to detect different sizes of red objects.
•	Change the color and text properties in cv2.putText() or cv2.circle().

🖥 Example Output

The program will display:
✅ A live video feed with the detected red ball highlighted.
✅ A mask window showing only the red areas detected.

📌 Notes
•	The HSV color range is optimized for bright red objects. If the ball is not detected, try adjusting the lower and upper HSV bounds.
•	Works best in well-lit environments to avoid color distortion.
•	You can modify the script to track other colors by changing the HSV values accordingly.


This project is open-source and can be modified or redistributed freely. 🎯

