## Exploring Visual ChatGPT In GPU & CPU
Recently ChatGPT is the hot trending topic in the world of Artificial Intelligence. When the world is trying to digest it and exploring its capabilities Microsoft has come up with yet an another surprise!!!
**The Visual ChatGPT**

The Visual ChatGPT combines ChatGPT and a series of Visual Foundation Models to send and receive images during textual conversation. In this blog we will see how we can quickly explore the visual ChatGPT in both GPU and CPU. Please follow the steps below to run Visual ChatGPT in your local environment.

### Step 1: Download the code from git
```bash
git clone https://github.com/microsoft/visual-chatgpt.git

cd visual-chatgpt
```
### Step 2: Create conda environment and activate it
```bash
# create a new environment
conda create -n visgpt python=3.8

# activate the new environment
conda activate visgpt
```

### Step 3: Install the Pip Requirements
```bash
#  prepare the basic environments
pip install -r requirement.txt
```

Note:

If you face any error with opencv-contrib-python you can update it to the latest version in requiremens.txt as below:

```bash
opencv-contrib-python==4.7.0.72
```

If found any error related to rust compiler not found you need to install rust. On MacOS you can use the brew command or On Linux you can use the apt:

```bash
#MacOS
brew install rust 
#Linux
sudo apt install rustc
```

### Step 4: Download visual foundation models
```bash
bash download.sh
```
### Step 5: Obtain your api key from openai and export it
```bash
# prepare your private openAI private key
export OPENAI_API_KEY={Your_Private_Openai_Key}
```
Navigate to https://platform.openai.com/docs/quickstart/build-your-application and create your secret key if you have not created before.

![image](https://user-images.githubusercontent.com/7569031/228010198-c22c3e22-eab1-4430-a2d9-4a583cc1a6fe.png)
> Secret Key for Open AI API

### Step 6: Create a folder to save your images
```bash
# create a folder to save images
mkdir ./image
```
### Step 7: Run Visual ChatGPT (GPU)
```bash
# Start Visual ChatGPT !
python visual_chatgpt.py
```
Please go through steps below to run Visual ChatGPT in CPU.

### Step 8: Open http://0.0.0.0:7860/ in your browser. Voila !!!
![image](https://user-images.githubusercontent.com/7569031/228010619-bb32355b-44da-4a44-86d7-978679005bc0.png)
> Visual ChatGPT in local browser

In order to save some memory you can select only the tools that you wish to use it. You can do so by modifying self.tools as follows:

![image](https://user-images.githubusercontent.com/7569031/228010914-6e7df6a6-ff0e-401e-85d7-19fccec41d51.png)
> Modification of self.tools to save some memory

Modify the following in visual_chatgpt.py to run in CPU:

1. Change the device to CPU.
![image](https://user-images.githubusercontent.com/7569031/228011293-364e5c7d-82fe-4929-9871-d234b925dca2.png)
> Modify the device to CPU to run ML Models in CPU

2. Not all Models are cpu supported so I have ignored few models.

3. “torch_dtype=torch.float16” is not supported on CPU so we have to remove it at class Pix2Pix and class T2I init methods when declaring the pipe.

![image](https://user-images.githubusercontent.com/7569031/228011463-418b9bf0-f92f-4936-90ac-06732b8fd2ad.png)
> Ignoring the torch dtype on Pix2Pix

![image](https://user-images.githubusercontent.com/7569031/228011534-4cbf8bdf-3d42-409e-a98d-e4575acb6f8d.png)
> Ignoring the torch dtype on T2I

Now you are ready to run visual chatGPT in CPU mode. Perform Step 7 and Step 8 mentioned above.

Imported from my article [Medium: Exploring Visual ChatGPT In GPU & CPU](https://medium.com/design-bootcamp/exploring-visual-chatgpt-in-gpu-cpu-6d24f3f8ea34)

