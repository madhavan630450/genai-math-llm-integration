# Experiment 1: Integration of Mathematical Calculations with Gemini Function-Calling

## Aim
To design and implement a Python function for calculating the volume of a cylinder, integrate it with a chat completion system utilizing the function-calling feature of a large language model (LLM).

## Theory
The system combines:
1. **Mathematical Component**: Uses the formula V = πr²h for cylinder volume calculation
2. **AI Component**: Utilizes Google's Gemini model for natural language processing
3. **Integration Layer**: Connects the AI understanding with mathematical computation

## Algorithm
1. Initialize Gemini AI and configure API key
2. Accept natural language input from user
3. Process query through Gemini to extract parameters
4. Calculate cylinder volume using extracted parameters
5. Format and display results
6. Handle any errors gracefully

## Program
```python
import os
import openai
from dotenv import load_dotenv, find_dotenv
_ = load_dotenv(find_dotenv()) # read local .env file
openai.api_key = os.environ['OPENAI_API_KEY']

import json
import math
# Function to calculate volume of a cylinder
def calculate_cylinder_volume(radius, height):
    if radius <= 0 or height <= 0:
        return "Enter a valid value"
    
    volume = math.pi * (radius ** 2) * height
    
    return json.dumps({
        "radius": radius,
        "height": height,
        "volume": round(volume, 2),
        "unit": "cubic units"
    })
# define a function
functions = [
    {
        "name": "calculate_cylinder_volume",
        "description": "Calculate the volume of a cylinder using radius and height",
        "parameters": {
            "type": "object",
            "properties": {
                "radius": {
                    "type": "number",
                    "description": "Radius of the cylinder"
                },
                "height": {
                    "type": "number",
                    "description": "Height of the cylinder"
                }
            },
            "required": ["radius", "height"],
        },
    }
]
messages = [
    {
        "role": "user",
        "content": "Find the volume of a cylinder where radius is 7 and height is 12"
    }
]
import openai
# Call the ChatCompletion endpoint
response = openai.ChatCompletion.create(
    # OpenAI Updates: As of June 2024, we are now using the GPT-3.5-Turbo model
    model="gpt-3.5-turbo",
    messages=messages,
    functions=functions
)
print(response)
response_message = response["choices"][0]["message"]
response_message
response_message["content"]
response_message["function_call"]
json.loads(response_message["function_call"]["arguments"])
args = json.loads(response_message["function_call"]["arguments"])
calculate_cylinder_volume(122,133)
```

## Output

<img width="877" height="478" alt="image" src="https://github.com/user-attachments/assets/7c52822e-0907-423f-9ab2-8312436bed8b" />


## Result
The experiment successfully demonstrated:
- Integration of Gemini AI with mathematical calculations
- Accurate parameter extraction from natural language
- Precise volume calculations with appropriate unit handling
- Robust error management
- Interactive user interface
