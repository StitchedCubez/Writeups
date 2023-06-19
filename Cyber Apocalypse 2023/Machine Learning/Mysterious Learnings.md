Description: 
One day the archeologist came across a strange metal plate covered in uncommon hieroglyphics. It looked like blueprints for some kind of alien technology. "What kind of magic is this?" He studied the plate more closely and was amazed by the advanced technology and incredible engineering they were using at a time like this. This could only lead him in him wanting to learn more…

Downloaded ml_mysterious_learnings.zip file
- unzip ml_mysterious_learnings.zip
	- contains 'alien.h5' file

H5 file contains multidimensional arrays of scientific data

Download link: https://www.hdfgroup.org/downloads/

Installed HDFView to open .h5 file

alien.h5 file in HDFView shows 2 objects "model_weights" and "optimizer_weights"
	
  - each object has 'subfolders' with data in each folder
	
  - tried plotting each set of data to graph for flag > did not work like 1st ML challenge lol

Found this 'uZDNyc3Q0bmR9' string to be unique and stood out (looks like base64)
	
  - tried decrypting this string (echo 'uZDNyc3Q0bmR9' |  base64 -d) > returns non-ASCII text

Viewed object info for 'model_weights' > shows backend and keras version info
	
  - backend is using tensorflow with keras version 2.11.0

Researched more about TensorFlow and Keras
	
  - TensorFlow is an end-to-end machine learning platform that is used within Python
	
  - Keras is an API designed to simplify use of TensorFlow in Python

REF: https://www.tensorflow.org/api_docs/python/tf/keras

Tried installing TensorFlow into Kali Linux VM
	- pip install tensorflow

Tried importing tensorflow into python to start analyzing this file > error: "illegal hardware instructions"

Some articles online advised creating a virtual environment within Python

```
pip install venv
python3 -m venv env
source env/bin/activate
```

Installed tensorflow through pip again in venv

Tried importing tensorflow into python again > same error as above

Found more info on TensorFlow
	
  - supports Python versions 3.7-3.10

Installed Python 3.9.16 directly onto Kali Linux VM

Setup venv in python 3.9.16 again and imported tensorflow > error: "TensorFlow library was compiled to use AVX instructions, but these aren't available on your machine"


- did not find any info on enabling AVX on CPU for VM

- only option I found was to install tensorflow 2.8.0 wheel available on GitHub repo

Setup TensorFlow again from 2.8.0 wheel with no AVX > this now works in Python

```
import h5py
import tensorflow as tf
from tensorflow import keras
import dis

file = h5py.File('alien.h5')
model = tf.keras.models.load_model(file, compile=False)
model.summary() #this prints out a summary of the model

#found lambda layer 'uZDNyc3Q0bmR9'
#lambda layers in keras are used to carry out custom non built-in functions on input data

#tried to view function of 'uZDNyc3Q0bmR9' layer

uZDNyc3Q0bmR9 = model.layers[5]
function = uZDNyc3Q0bmR9.function
print(function) #prints object at memory address in Python

bytecode = dis.Bytecode(function)
for i in bytecode:
	print(i.opname)
	
#returns LOAD FAST, RETURN VALUE
#this function basically takes in the input data and returns it to the output without changing it
```

Found this 'uZDNyc3Q0bmR9' string to be unique and stood out (looks like base64)
	
  - tried decrypting this string (echo 'uZDNyc3Q0bmR9' |  base64 -d) > returns non-ASCII text

DID NOT FIND FLAG

===================================================================================

AFTER PARTY

Reviewed write-up for this challenge

REF: https://github.com/Hansuuuuuuuuuu/CTF_Writeups/blob/master/Cyber-Apocalypse-CTF-2023/ml_mysterious_learnings.md

They found 3 separate base64 encoded strings when loading this model into TensorFlow Keras
	
  - first string ('SFRCe24wdF9zb') appears when loading model into TensorFlow Keras
	
  - second string ('19oNHJkX3RvX3V') appears as the model name when viewing model.summary()
	
  - third string ('uZDNyc3Q0bmR9') is the lambda layer that I tried decrypting

They combined all 3 strings and decrypted via base64 > this gave the flag

echo 'SFRCe24wdF9zb19oNHJkX3RvX3VuZDNyc3Q0bmR9' | base64 -d

FLAG NOW FOUND

