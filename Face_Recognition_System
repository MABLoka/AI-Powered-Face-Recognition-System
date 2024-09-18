import os, sys
from PyQt6.QtWidgets import *
from PyQt6.QtCore import *
from PyQt6.QtGui import *
import face_recognition as fr

import cv2
import numpy as np
from time import sleep

#import arcpy
import re
import traceback
import collections
import shutil
from shutil import copyfile

class newFace(QWidget):
        def __init__(self):
                super().__init__()
                layout = QVBoxLayout()
                self.label = QLabel("Another Window")
                layout.addWidget(self.label)
                self.setLayout(layout)        
                
class AppDemo(QMainWindow):
     
        def __init__(self):
                super().__init__()
                self.setAcceptDrops(True)
                self.resize(1200,600)

                #Button to identify the faces
                self.label = QLabel(self)
                self.btn = QPushButton('Identify', self)
                self.btn.setGeometry(20,20,100,20)
                
                #Text box to take user input
                self.txtbox = QLineEdit(self)
                self.txtbox.move(200, 20)
                self.txtbox.resize(280,40)
                self.txtbox.setDisabled(True)

                #button to submit user input
                self.btn2 = QPushButton('Submit', self)
                self.btn2.setGeometry(490,20,100,20)
                self.btn2.setDisabled(True)

                self.btn.clicked.connect(lambda :self.on_button_click())
                self.btn2.clicked.connect(lambda :self.addFace())


                self.path = None
                self.face_locations = []
                self.face_names = []

        def on_button_click(self):
            if self.path:
                self.classify_faces(self.path)
            else:
                print("No file selected.")


        def dragEnterEvent(self, event):
                if event.mimeData().hasUrls:
                        event.accept()
                else:
                        event.ignore

        def dragMoveEvent(self, event):
                if event.mimeData().hasUrls():
                        event.setDropAction(Qt.DropAction.CopyAction)
                        event.accept()
                else:
                        event.ignore()

        def dropEvent(self, event):
                if event.mimeData().hasUrls():
                        event.setDropAction(Qt.DropAction.CopyAction)
                        event.accept()

                        links = []

                        print(event.mimeData().urls()[0].toLocalFile())

                        for url in event.mimeData().urls():
                                if url.isLocalFile():
                                    links.append(str(url.toLocalFile()))
                                else:
                                    links.append(str(url.toString()))

                        

                        self.path = event.mimeData().urls()[0].toLocalFile()

                        self.pixmap = QPixmap(event.mimeData().urls()[0].toLocalFile())

                        self.label.setPixmap(self.pixmap)

                        self.label.setGeometry(150, 150, self.pixmap.width(), self.pixmap.height())

                        self.update() 
                        
                else:
                        event.ignore()

        def paintEvent(self, event):
            if not self.face_locations:
                return  # If no faces are detected, don't draw anything

            self.painter = QPainter(self)
            self.rect = QRect(self.pixmap.width()+250, 150, self.pixmap.width(), self.pixmap.height())

            self.painter.drawPixmap(self.rect, self.pixmap)
            pen = QPen(QColor(Qt.GlobalColor.red))
            # Draw bounding boxes around faces
            for (top, right, bottom, left), name in zip(self.face_locations, self.face_names):
                self.painter.setPen(pen)  # Correct pen color
                self.painter.drawRect(left+self.pixmap.width()+250, top+150, right-left, bottom-top)
                self.painter.drawText(left+self.pixmap.width()+250, bottom+160, name)

            self.painter.end()

            if(len(self.face_names) == 1 and self.face_names[0] == "unknown"):
                self.txtbox.setDisabled(False)
                self.btn2.setDisabled(False)
            elif(len(self.face_names) > 1 and self.face_names.__contains__("unknown")):
                print("Submit a name of each person in individual photos")


        def addFace(self):
              
              filename = self.txtbox.text()
              
              i = 1

              if os.path.exists("./faces"):
                if not(os.path.exists(os.getcwd() + "/faces"+ "/" + filename + ".jpg")):
                        copyfile(self.path, os.getcwd() + "/faces"+"/" + filename + ".jpg")

                else:
                        while True:
                                #(os.path.exists(os.getcwd() + "/faces"+ "/" + filename + ".jpg") or os.path.exists(os.getcwd() + "/faces"+"/"+filename + "_" + str(i) + ".jpg")):
                                
                                if not os.path.exists(os.getcwd() + "/faces"+ "/" + filename + str(i) + ".jpg"):
                                        copyfile(self.path, os.getcwd() + "/faces"+"/" + filename + str(i) +".jpg")
                                        break
                                i = i+1

              else:
                print("failure")

              self.txtbox.clear()
              self.txtbox.setDisabled(True)
            
            
                              
             
        def get_encoded_faces(self):
    
    
            encoded = {}
    
            for dirpath, dnames, fnames in os.walk("./faces"):
                for f in fnames:
                    if f.endswith(".jpg") or f.endswith(".png"):
                        face = fr.load_image_file("faces/" + f)
                        encoding = fr.face_encodings(face)[0]
                        encoded[f.split(".")[0]] = encoding
                
                
            return encoded
        
        def unknown_image_encoded(self, img):

            face = fr.load_image_file("faces/" + img)
            encoded = fr.face_encodings(face)[0]
    
            return encoded

        def classify_faces(self, im):
            
            painter = QPainter(self)

            faces = self.get_encoded_faces()
            faces_encoded = list(faces.values())
            known_face_names = list(faces.keys())
    
    
            img = cv2.imread(im, 1)
    
            self.face_locations = fr.face_locations(img)
            unknown_face_encodings = fr.face_encodings(img, self.face_locations)
    
            self.face_names = []
            for face_encoding in unknown_face_encodings:
        
                matches = fr.compare_faces(faces_encoded, face_encoding)
                name = "unknown"
        
                face_distances = fr.face_distance(faces_encoded, face_encoding)
                best_match_index = np.argmin(face_distances)
                if matches[best_match_index]:
                    name = known_face_names[best_match_index]
        
                self.face_names.append(name)

            self.update()


app = QApplication(sys.argv)

demo = AppDemo()
demo.show()

sys.exit(app.exec())
