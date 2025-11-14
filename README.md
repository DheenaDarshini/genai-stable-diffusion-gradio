## Prototype Development for Image Generation Using the Stable Diffusion Model and Gradio Framework
## Name: Dheena Darshini Karthik Dheepan
## Regno: 212223240030

### AIM:
To design and deploy a prototype application for image generation utilizing the Stable Diffusion model, integrated with the Gradio UI framework for interactive user engagement and evaluation.

### PROBLEM STATEMENT:
Generating high-quality and creative images from text descriptions is a challenging task. This project aims to create an easy-to-use application that can turn simple prompts into realistic images using the Stable Diffusion model. It also provides an interactive interface for users to try and evaluate the results.

### DESIGN STEPS:

#### STEP 1:
Get the API key and set up access to the image generation model.

#### STEP 2:
Write a function that takes a prompt and gets the generated image from the model.

#### STEP 3:
Build a simple user interface using Gradio where users can type a prompt and see the generated image.

### PROGRAM:
```python
import os
import io
import IPython.display
from PIL import Image
import base64 
from dotenv import load_dotenv, find_dotenv
_ = load_dotenv(find_dotenv()) # read local .env file
hf_api_key = os.environ['HF_API_KEY']

# Helper function
import requests, json

#Text-to-image endpoint
def get_completion(inputs, parameters=None, ENDPOINT_URL=os.environ['HF_API_TTI_BASE']):
    headers = {
      "Authorization": f"Bearer {hf_api_key}",
      "Content-Type": "application/json"
    }   
    data = { "inputs": inputs }
    if parameters is not None:
        data.update({"parameters": parameters})
    response = requests.request("POST",
                                ENDPOINT_URL,
                                headers=headers,
                                data=json.dumps(data))
    return json.loads(response.content.decode("utf-8"))

import gradio as gr 

#A helper function to convert the PIL image to base64
#so you can send it to the API
def base64_to_pil(img_base64):
    base64_decoded = base64.b64decode(img_base64)
    byte_stream = io.BytesIO(base64_decoded)
    pil_image = Image.open(byte_stream)
    return pil_image

def generate(prompt):
    output = get_completion(prompt)
    result_image = base64_to_pil(output)
    return result_image

gr.close_all()
demo = gr.Interface(fn=generate,
                    inputs=[gr.Textbox(label="Your prompt")],
                    outputs=[gr.Image(label="Result")],
                    title="Image Generation with Stable Diffusion",
                    description="Generate any image with Stable Diffusion",
                    allow_flagging="never",
                    examples=["the spirit of a tamagotchi wandering in the city of Vienna","a mecha robot in a favela"])

demo.launch(share=True, server_port=int(os.environ['PORT1']))
```
### OUTPUT:
<img width="1025" height="1037" alt="Screenshot 2025-11-14 114144" src="https://github.com/user-attachments/assets/47292bf3-f68e-414e-8df0-b9301951fdb9" />

<img width="1845" height="995" alt="Screenshot 2025-11-14 114214" src="https://github.com/user-attachments/assets/8ad35c1a-47a5-4af1-91c6-f68a32d15c0f" />


### RESULT:
A prototype application for image generation was successfully designed and deployed using the Stable Diffusion model. The application was integrated with the Gradio UI framework, enabling interactive user engagement and effective evaluation of the model’s output.
