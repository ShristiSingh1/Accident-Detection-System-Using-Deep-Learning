
# Accident Detection with a Reporting System 🚗🚨

## Abstract 📜
The rise in global population and economic growth has led to an increase in the number of vehicles on the roads, which unfortunately also results in a higher incidence of road accidents. Quickly identifying accident locations and reporting them to emergency services can save many lives. This project leverages **Convolutional Neural Networks (CNN)**, **Python**, and other tools to create an accident detection system that can analyze accident footage, identify incidents, and alert emergency services promptly.

## Features ✨
- Automated accident detection using **Deep Learning (CNN)**.
- Reporting system using **Twilio SMS** for quick alerts.
- Data visualization and analysis for understanding accident trends.

## Requirements ⚙️

1. **Python**: Install [Anaconda](https://www.anaconda.com/products/distribution) to manage dependencies.
2. **Twilio**: Sign up for an account on [Twilio](https://www.twilio.com/) to enable SMS notifications (you can use the 7-day free trial).
3. **Pre-trained VGG16 model** for feature extraction.

## Installation 📥

1. **Set up the environment**:
   ```bash
   conda create -n accident_detection python=3.8
   conda activate accident_detection
   ```
2. **Install the necessary libraries**:
   ```bash
   pip install numpy pandas matplotlib opencv-python-headless geopy keras tensorflow scikit-image twilio
   ```
3. **Clone or download this repository** and extract it to a suitable location.

4. **Twilio Setup**: Create an account on [Twilio](https://www.twilio.com/) and retrieve the following:
   - Virtual phone number
   - Account SID
   - Authentication token

## Usage 🚀

### Step 1: Capture Frames from Video Footage
The script reads accident footage (e.g., `Accidents.mp4`) and saves frames for analysis.
- Ensure your video file is located in the same directory.
- Run the script to extract frames:
  ```python
  cap = cv2.VideoCapture("Accidents.mp4")
  # Frame extraction code here...
  ```
  
### Step 2: Run the Detection Model
The model will analyze each frame and detect accidents based on the trained CNN model.
- **Training**: Load the `training_data.csv` to train the model with images labeled as accident or no accident.
- **Prediction**: Once the model is trained, run predictions on test data and check the output in real-time.

### Step 3: Send Notifications via Twilio SMS
In the script `Accident Detection-Video.ipynb`, enter your Twilio credentials and the recipient’s phone number:
   ```python
   account_sid = 'your_account_sid'
   auth_token = 'your_auth_token'
   client = Client(account_sid, auth_token)
   ```
When an accident is detected, the script will automatically send an SMS alert.

## Code Snippets 🖥️

### Frame Extraction
```python
cap = cv2.VideoCapture("Accidents.mp4")
frameRate = cap.get(5)
count = 0
while(cap.isOpened()):
    ret, frame = cap.read()
    if not ret:
        break
    if int(cap.get(1)) % math.floor(frameRate) == 0:
        filename = f"{count}.jpg"
        cv2.imwrite(filename, frame)
        count += 1
cap.release()
```

### Accident Detection Model Training
```python
from keras.applications.vgg16 import VGG16
from keras.models import Sequential
from keras.layers import Dense, InputLayer
# Preprocess data and train the model
model = Sequential()
model.add(InputLayer((7*7*512,)))
model.add(Dense(units=1024, activation='sigmoid'))
model.add(Dense(2, activation='softmax'))
model.compile(loss='categorical_crossentropy', optimizer='adam', metrics=['accuracy'])
model.fit(X_train, y_train, epochs=100, validation_data=(X_valid, y_valid))
```

### SMS Notification
```python
from twilio.rest import Client
client = Client(account_sid, auth_token)
message = client.messages.create(
    body="Accident detected at [location]! Please dispatch emergency services.",
    from_='+1234567890', # Twilio number
    to='+0987654321'     # Recipient's number
)
```

## Data Visualization 📊

The project uses **Matplotlib** to visualize accident occurrences. The `value` column in `training_data.csv` categorizes images into `Accident` or `No-accident`, providing a breakdown in a pie chart.

```python
import matplotlib.pyplot as plt

labels = ['No-accident', 'Accident']
sizes = data['value'].value_counts().values
colors = ['pink', 'red']

plt.pie(sizes, labels=labels, colors=colors, autopct='%1.1f%%', startangle=90)
plt.axis('equal')
plt.title('Distribution of Accident and No-accident Data')
plt.show()
```

## Folder Structure 📂

```
Accident-Detection-With-Reporting-System/
├── Accident Detection-Video.ipynb  # Main code
├── Accidents.mp4                   # Sample video for testing
├── training_data.csv               # Labeled data for model training
├── test.csv                        # Test data for validation
├── README.md                       # Documentation
└── requirements.txt                # Dependencies
```

## Author 🧑‍💻

- **Shristi Singh**  
  [LinkedIn](https://www.linkedin.com/in/shristi-singh-data-analyst/)

---

## License 📝
This project is licensed under the MIT License. Feel free to use, modify, and distribute this code as per the license conditions.

---

## Notes 🗒️
- Ensure your video files and images are correctly named and located in the specified directory.
- Use a GPU if possible for faster model training.
- Update Twilio credentials before running the SMS functionality.
  
---
