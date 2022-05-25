# Kaggle
<br>

## Kaggle 01：[Heart Disease UCI](https://www.kaggle.com/c/heart-disease-uci/data)
* 使用模型：Support Vector Machine (SVM) Algorithm
* 分析結果：[]()
<br>


## Kaggle 02：[Dogs vs. Cats](https://www.kaggle.com/c/dogs-vs-cats)
* 使用模型：ResNet-50
* 分析結果：[]()
<br>


## TensorFlow
```
!pip install tensorflow==2.0.0-alpha0 
```
### DNN
* Sequential: That defines a SEQUENCE of layers in the neural network
* Flatten: Remember earlier where our images were a square, when you printed them out? Flatten just takes that square and turns it into a 1 dimensional set.
* Dense: Adds a layer of neurons

* Each layer of neurons need an activation function to tell them what to do. There's lots of options, but just use these for now.
  * Relu effectively means "If X>0 return X, else return 0" -- so what it does it it only passes values 0 or greater to the next layer in the network.
  * Softmax takes a set of values, and effectively picks the biggest one, so, for example, if the output of the last layer looks like [0.1, 0.1, 0.05, 0.1, 9.5, 0.1, 0.05, 0.05, 0.05], it saves you from fishing through it looking for the biggest value, and turns it into [0,0,0,0,1,0,0,0,0] -- The goal is to save a lot of coding!
```
import tensorflow as tf
print(tf.__version__)

class myCallback(tf.keras.callbacks.Callback):
  def on_epoch_end(self, epoch, logs={}):
    if(logs.get('loss')<0.4):
      print("\nReached 60% accuracy so cancelling training!")
      self.model.stop_training = True

callbacks = myCallback()
mnist = tf.keras.datasets.fashion_mnist
(train_images, train_labels), (test_images, test_labels) = mnist.load_data()

# Look
import numpy as np
np.set_printoptions(linewidth=200)
import matplotlib.pyplot as plt
plt.imshow(training_images[0])
print(training_labels[0])
print(training_images[0])

# Normalizing
train_images = train_images/255.0
test_images = test_images/255.0
model = tf.keras.models.Sequential([
                 tf.keras.layers.Flatten(),
                 #tf.keras.layers.Flatten(input_shape=(28, 28)),
                 tf.keras.layers.Dense(512, activation=tf.nn.relu),
                 tf.keras.layers.Dense(10, activation=tf.nn.softmax)
        ])
   
# we have to specify 2 functions, a loss and an optimizer
model.compile(optimizer='adam', loss='sparse_categorical_crossentropy')
#model.compile(optimizer=tf.optimizers.Adam(),
#              loss='sparse_categorical_crossentropy',
#              metrics=['accuracy'])
#model.compile(optimizer='sgd', loss='mean_squared_error')
model.fit(train_images, train_labels, epochs=5, callbacks=[callbacks])
model.evaluate(test_images, test_labels)
```
```
# Normalizing
train_images = train_images.reshape(60000, 28, 28, 1)
train_images = train_images/255.0
test_images = test_images.reshape(10000, 28, 28, 1)
test_images = test_images/255.0

model = tf.keras.models.Sequential([
  tf.keras.layers.Conv2D(64, (3,3), activation='relu', input_shape=(28, 28, 1)),
  tf.keras.layers.MaxPooling2D(2, 2),
  tf.keras.layers.Conv2D(64, (3,3), activation='relu'),
  tf.keras.layers.MaxPooling2D(2,2),
  tf.keras.layers.Flatten(),
  tf.keras.layers.Dense(128, activation='relu'),
  tf.keras.layers.Dense(10, activation='softmax')
])
model.compile(optimizer='adam', loss='sparse_categorical_crossentropy', metrics=['accuracy'])
model.summary()
model.fit(train_images, train_labels, epochs=5)
test_loss = model.evaluate(test_images, test_labels)
```
<br>
