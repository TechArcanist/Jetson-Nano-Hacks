### **To start the Camera** 
```python
import cv2
import ipywidgets as widgets
from IPython.display import display
import threading
import time


running = True
camera_index = 0  
camera_widget = widgets.Image()

def update_camera_feed():
    global running
    camera = cv2.VideoCapture(camera_index)
    if not camera.isOpened():
        print("Error: Could not open camera.")
        return

    while running:
        ret, frame = camera.read()
        if not ret:
            print("Error: Could not read frame.")
            break
        

        _, jpeg = cv2.imencode('.jpg', frame)
        camera_widget.value = jpeg.tobytes()
        
     
        time.sleep(0.1)
    
    camera.release()
    print("Camera feed closed.")


camera_thread = threading.Thread(target=update_camera_feed)
camera_thread.start()


display(camera_widget)

```
![image](https://github.com/user-attachments/assets/fe936ab8-5704-49f1-ab70-2ce654192a15)

### **To stop the Camera** 
```python
def stop_camera():
    global running
    running = False 
    camera_thread.join()
    print("Camera stopped.")

stop_camera()

```
