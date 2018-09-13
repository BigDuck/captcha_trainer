# Attention
1. Choose your version:
    This project uses GPU for training by default.
    You can use the CPU version by replacing ```tensorflow-gpu==1.9.0``` in the requirements.txt file with ```tensorflow==1.9.0```
    
# Ready to work
   If you want to use the GPU for training, you must first install CUDA and cuDNN:
   https://www.tensorflow.org/install/install_sources#tested_source_configurations
   Please open the above link to view the version of CUDA and cuDNN corresponding to the current TensorFlow version.
   
# Start
1. Install the python 3.6 environment (with pip)
2. Install virtualenv ```pip3 install virtualenv```
3. Create a separate virtual environment for the project:
    ```bash
    virtualenv -p /usr/bin/python3 venv # venv is the name of the virtual environment.
    cd venv/ # venv is the name of the virtual environment.
    source bin/activate # to activate the current virtual environment.
    cd captcha_platform # captcha_platform is the project path.
    ```
4. ```pip install -r requirements.txt```


# Configuration
1. config.yaml - System Config
    ```yaml
    # Device: [gpu:0, cpu:0] The default device is GPU.
    # - If you use the GPU version, you need to install some additional applications.
    # TrainRegex and TestRegex: Default matching apple_20181010121212.jpg file.
    # TrainsPath and TestPath: The local path of your training and testing set.
    System:
      NeuralNet: 'CNNNet'
      Device: 'gpu:0'
      TrainsPath: 'E:\Task\Trains\YourModelName'
      TrainRegex: '.*?(?=_.*\.)'
      TestPath: 'E:\Task\TestGroup\YourModelName'
      TestRegex: '.*?(?=_.*\.)'
    
    # SavedStep: A Session.run() execution is called a Step,
    # - Used to save training progress, Default value is 100.
    # TestNum: The number of samples for each test batch.
    # - A test for every saved steps.
    # CompileAcc: When the accuracy reaches the set threshold,
    # - the model will be compiled together each time it is archived.
    # - Available for specific usage scenarios.
    # EndAcc: Finish the training when the accuracy reaches [EndAcc*100]%.
    # EndStep: Finish the training when the step is greater than the [-1: Off, EndStep >0: On] step.
    # LearningRate: Find the fastest relationship between the loss decline and the learning rate.
    Trains:
      SavedStep: 100
      TestNum: 500
      CompileAcc: 0.8
      EndAcc: 0.95
      EndStep: -1
      LearningRate: 0.001
    ```
    There are several common examples of TrainRegex:
    i. apple_20181010121212.jpg
    ```
    .*?(?=_.*\.)
    ```
    ii apple.png
    ```
    .*?(?=\.)
    ```
    
1. model.yaml  - Model Config
    ```yaml
    # Convolution: The number of layers is at least 3.
    # - The number below corresponds to the size of each layer of convolution.
    DenseNet:
      Filters: 24
    
    # Provide flexible neural network construction,
    # Adjust the neural network structure that suits you best
    # [Convolution, Pool, Optimization: {Dropout}]
    CNNNet:
      Layer:
        - Convolution: 32
        - Pool: [1, 2, 2, 1]
        - Optimization: Dropout
        - Convolution: 64
        - Pool: [1, 2, 2, 1]
        - Optimization: Dropout
        - Convolution: 64
        - Pool: [1, 2, 2, 1]
        - Optimization: Dropout
      ConvCoreSize: 3
      FullConnect: 1024
    
    # ModelName: Corresponding to the model file in the model directory,
    # - such as YourModelName.pb, fill in YourModelName here.
    # CharSet: Provides a default optional built-in solution:
    # - [ALPHANUMERIC, ALPHANUMERIC_LOWER, ALPHANUMERIC_UPPER, NUMERIC]
    # - Or you can use your own customized character set like: ['a', '1', '2'].
    # ImageChannel: [1 - Gray Scale, 3 - RGB].
    # CharLength: Captcha Length.
    Model:
      ModelName: YourModelName
      ImageChannel: 1
      CharLength: 4
      CharSet: ALPHANUMERIC_LOWER
    
    # Magnification: [ x2 -> from size(50, 50) to size(100,100)].
    # OriginalColor: [false - Gray Scale, true - RGB].
    # Binaryzation: [-1: Off, >0 and < 255: On].
    # Smoothing: [-1: Off, >0: On].
    # Blur: [-1: Off, >0: On].
    # Resize: [WIDTH, HEIGHT].
    Pretreatment:
      Magnification: 0
      OriginalColor: false
      Binaryzation: 240
      Smoothing: 3
      Invert: false
      Blur: 5
    #  Resize: [160, 60]
    ```
    

# Run
1. ```python trains.py```

# INTRODUCTION
https://www.jianshu.com/p/b1a5427db6e2
