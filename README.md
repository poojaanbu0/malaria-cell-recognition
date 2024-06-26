# Deep Neural Network for Malaria Infected Cell Recognition

## AIM

To develop a deep neural network for Malaria infected cell recognition and to analyze the performance.

## Problem Statement and Dataset

Creating a neural network to detect malaria-infected cells involves training it on a dataset containing images of both infected and uninfected cells. The network learns to differentiate between the two types of cells, aiding in malaria diagnosis and treatment.

## Neural Network Model

![image](https://github.com/poojaanbu0/malaria-cell-recognition/assets/119390329/238b2a89-7070-4552-85ff-b74e10cb99b8)


## DESIGN STEPS

### STEP 1: Libraries are imported, including TensorFlow.

### STEP 2: TensorFlow is configured for GPU acceleration.

### STEP 3: Data augmentation enhances model generalization.

### STEP 4: A CNN architecture is designed with convolutional layers.

### STEP 5: The model is trained using the training data.

### STEP 6: Model performance is evaluated using testing data.

## PROGRAM

### Name:POOJA A

### Register Number:212222240072

```python
import tensorflow as tf
# to share the GPU resources for multiple sessions
from tensorflow.compat.v1.keras.backend import set_session
config = tf.compat.v1.ConfigProto()
config.gpu_options.allow_growth = True # dynamically grow the memory used on the GPU
config.log_device_placement = True # to log device placement (on which device the operation ran)
sess = tf.compat.v1.Session(config=config)
set_session(sess)
%pip install matplotlib

%matplotlib inline
!pip install seabornimport os
import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt
from matplotlib.image import imread
from tensorflow.keras.preprocessing.image import ImageDataGenerator
from tensorflow import keras
from tensorflow.keras import layers
from tensorflow.keras import utils
from tensorflow.keras import models
from sklearn.metrics import classification_report,confusion_matrix


my_data_dir = 'dataset/cell_images'os.listdir(my_data_dir)
test_path = my_data_dir+'/test/'
train_path = my_data_dir+'/train/'
os.listdir(train_path)
len(os.listdir(train_path+'/uninfected/'))
len(os.listdir(train_path+'/parasitized/'))
os.listdir(train_path+'/parasitized')[0]
para_img= imread(train_path+
                 '/parasitized/'+
                 os.listdir(train_path+'/parasitized')[0])
print("POOJA A")
print("212222240072")
plt.imshow(para_img)
# Checking the image dimensions
dim1 = []
dim2 = []
for image_filename in os.listdir(test_path+'/uninfected'):
    img = imread(test_path+'/uninfected'+'/'+image_filename)
    d1,d2,colors = img.shape
    dim1.append(d1)
    dim2.append(d2)
sns.jointplot(x=dim1,y=dim2)
image_shape = (130,130,3)
help(ImageDataGenerator)
image_gen = ImageDataGenerator(rotation_range=20, # rotate the image 20 degrees
                               width_shift_range=0.10, # Shift the pic width by a max of 5%
                               height_shift_range=0.10, # Shift the pic height by a max of 5%
                               rescale=1/255, # Rescale the image by normalzing it.
                               shear_range=0.1, # Shear means cutting away part of the image (max 10%)
                               zoom_range=0.1, # Zoom in by 10% max
                               horizontal_flip=True, # Allo horizontal flipping
                               fill_mode='nearest' # Fill in missing pixels with the nearest filled value
                              )
image_gen.flow_from_directory(train_path)
image_gen.flow_from_directory(test_path)
image_gen.flow_from_directory(train_path)
image_gen.flow_from_directory(test_path)

model = models.Sequential()
model.add(keras.Input(shape=(image_shape)))
model.add(layers.Conv2D(filters=32,kernel_size=(3,3),activation='relu',))
model.add(layers.MaxPooling2D(pool_size=(2, 2)))
model.add(layers.Conv2D(filters=32, kernel_size=(3,3), activation='relu',))
model.add(layers.MaxPooling2D(pool_size=(2, 2)))
model.add(layers.Conv2D(filters=32, kernel_size=(3,3), activation='relu',))
model.add(layers.MaxPooling2D(pool_size=(2, 2)))
model.add(layers.Flatten())
model.add(layers.Dense(128))
model.add(layers.Dense(64,activation='relu'))
model.add(layers.Dropout(0.5))
model.add(layers.Dense(1,activation='sigmoid'))
model.compile(loss='binary_crossentropy',optimizer='adam',metrics=['accuracy'])

model.summary()

batch_size = 16
help(image_gen.flow_from_directory)
train_image_gen = image_gen.flow_from_directory(train_path,
                                               target_size=image_shape[:2],
                                                color_mode='rgb',
                                               batch_size=batch_size,
                                               class_mode='binary')
train_image_gen.batch_size
len(train_image_gen.classes)
train_image_gen.total_batches_seen
test_image_gen = image_gen.flow_from_directory(test_path,
                                               target_size=image_shape[:2],
                                               color_mode='rgb',
                                               batch_size=batch_size,
                                               class_mode='binary',shuffle=False)
train_image_gen.class_indices
results = model.fit(train_image_gen,epochs=2,
                              validation_data=test_image_gen
                             )
model.save('cell_model.h5')
losses = pd.DataFrame(model.history.history)
print("POOJA A")
print("212222240072")
losses[['loss','val_loss']].plot()
model.metrics_names
print("POOJA A")
print("212222240072")
model.evaluate(test_image_gen)
pred_probabilities = model.predict(test_image_gen)
test_image_gen.classes
predictions = pred_probabilities > 0.5
print("POOJA A")
print("212222240072")
print(classification_report(test_image_gen.classes,predictions))
print("POOJA A")
print("212222240072")
confusion_matrix(test_image_gen.classes,predictions)
```

## OUTPUT

### Training Loss, Validation Loss Vs Iteration Plot

![image](https://github.com/poojaanbu0/malaria-cell-recognition/assets/119390329/cb2a2d20-7ee5-4dbb-b039-c7cf2e25d5b3)


### Classification Report

![image](https://github.com/poojaanbu0/malaria-cell-recognition/assets/119390329/9c60a26e-4a9f-43aa-8a3e-4128da607db8)


### Confusion Matrix

![image](https://github.com/poojaanbu0/malaria-cell-recognition/assets/119390329/d878e07e-33aa-4f5b-87fd-318aac6559bd)


### New Sample Data Prediction

![image](https://github.com/poojaanbu0/malaria-cell-recognition/assets/119390329/d4ac47ee-3137-4fbe-9edb-6ec6a4b6ea6f)


## RESULT
Thus a deep neural network for Malaria infected cell recognition is successfully developed and the performance is analyzed.
