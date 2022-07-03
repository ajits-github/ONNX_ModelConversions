# ONNX_ModelConversions
Converting one model to another via ONNX.

**Objective**

The aim of the project is to convert one model type to another using ONNX as the central format.

---

**Overview**

The below conversions are working for the standard models like ResNet, DenseNet, Mobilenet etc.

* TensorFlow => PyTorch, Tflite
* Keras => PyTorch, TensorFlow
* PyTorch => Coreml, TensorFlow, Tflite, Keras, TorchScript

Most of the above conversions use ONNX at the center and hence, the below conversions are also being performed (to and from):

* TensorFlow, PyTorch, Keras, Coreml <=> ONNX

---

**Execution**

* Make sure you have all the packages installed from requirements.txt. It includes some specific commits to be installed rather than the master lib of the package.
* There are two required parsers:

  * `--saved-model`
  * `--output`
* Both --saved-model and --output should be provided with extensions just like source and destination; It can be along with directories or just the model/weight names.
* Run the file from command line as below:

  * `python model_conv.py --saved-model /path/to/model.pb --output model.onnx`
* Valid formats are (.onnx, .mlmodel(coreml), .pt(torch), .pth(torch), .pb(tf), .tflite, .h5(keras)).
* The execution log will tell everything about all the conversions involved and the location of the intermediate and final models which is a temporary folder called 'model_conv'.

---

**Docker**

The docker image size is approximately 7Gb and can be used to run in the respective container with the proper arguments as discussed above. Below is the command to run the docker image `onnx:img` with proper arguments:

* `docker run -v <abspath/on/hostmachine/model.pt>:root/Documents/model.pt onnx:img --saved-model root/Dcouments/model.pt --output converted_model.onnx`
* Copy the folder `model_conv` from container to your host:
  * `docker cp <container_id>:model_conv <targetlocation/on/hostmachine>`

---

**Note**

* The conversion has not been tested with YOLOv3 weights though it may or may not work. It was not working for YOLOv5 PyTorch model as it needed complete architectural information about the model, making it more specific.
* Also, try to avoid .pth weights which doesn't work most of the time. Instead use more stable .pt weights for any PyTorch conversion.
* All the conversions have been working smoothly with opset version as 13.
* If you feel inference is not good, try converting model with a sample image (--sample-file parser).
* For keras to onnx conversion, you may need to enable certain lines of code to pass the custom objects. For e.g there are custom layers and losses in RetinaNet and hence those must be passed in the arguments.
* TensorFlow <=> CoreML is still in progress.

---

**References:**

[https://hub.docker.com/r/onnx/onnx-ecosystem](https://hub.docker.com/r/onnx/onnx-ecosystem "Follow link")

[https://github.com/onnx/onnx](https://github.com/onnx/onnx "Follow link")

[https://github.com/onnx/tutorials](https://github.com/onnx/tutorials "Follow link")
