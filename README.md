# AI-Powered-Face-Recognition-System-

# Face Recognition Application - README

## Introduction

This is a **Face Recognition Application** built using **Python**, **PyQt6**, **OpenCV**, and **face_recognition** libraries. It provides a graphical interface where users can drag and drop images to identify faces. If a face is unknown, the user can add it to the system's face database. The application is capable of recognizing previously identified faces by matching them against stored face encodings.

## Features

- **Face Identification**: The application identifies faces in images using pre-encoded data or classifies them as "unknown."
- **Drag-and-Drop Interface**: Users can easily drag and drop images for face recognition.
- **Face Registration**: Users can input names for unidentified faces and add them to the database.
- **Real-time Image Display**: Detected faces are highlighted with bounding boxes and labels on the image.
- **Persistence**: Stores face encodings for future use.

## Requirements

To run this project, you will need to install the following dependencies:

1. Python 3.x
2. PyQt6
3. OpenCV (`cv2`)
4. face_recognition
5. Numpy

### Install Dependencies

Run the following command to install required packages:

```bash
pip install pyqt6 opencv-python face_recognition numpy
```

## How to Use

1. **Clone the Repository**:
   Clone or download the project to your local machine.

2. **Run the Application**:
   Open a terminal and navigate to the project folder. Run the following command:

   ```bash
   python app.py
   ```

3. **Drag and Drop**:
   - Drag and drop an image file into the window to identify the faces.
   - Detected faces will be highlighted with bounding boxes.

4. **Add New Faces**:
   - If an unknown face is detected, a text box will be enabled.
   - Enter a name for the face and click the "Submit" button to add it to the database.

5. **Face Recognition**:
   - The next time the same face is recognized, it will be labeled with the name you provided.

## File Structure

- `app.py`: Main application script.
- `faces/`: Directory where face images are stored for encoding.

## How it Works

- **Face Recognition**: The application uses the `face_recognition` library to detect faces and encode them into numerical representations. These encodings are compared to the stored face data to recognize known individuals.
- **User Input**: If a face is unidentified, the user can enter the name, and the system will store the image and its encoding for future reference.
- **PyQt6 GUI**: The application provides a user-friendly interface using PyQt6 for intuitive interaction with the system.

## Troubleshooting

- If faces are not detected, ensure the image contains clear, front-facing images of individuals.
- Make sure the `faces/` directory exists before using the program. The system will store images and encodings here.

---
