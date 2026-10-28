## Integration of a Mathematical Calulations with a Chat Completion System using LLM Function-Calling

### AIM:
To design and implement a Python function for calculating the volume of a cylinder, integrate it with a chat completion system utilizing the function-calling feature of a large language model (LLM).

### PROBLEM STATEMENT:
Design and implement a system where a user can input integers and perform multiplication,
and the system calculates its volume by invoking a Python function using the function-calling capabilities of an LLM.

### DESIGN STEPS:

#### STEP 1:
Write a normal Python function, calculate_cylinder_volume(radius, height), that uses the math library to compute volume = π × radius² × height.

Write a JSON schema that tells the model about this function. It gives the function name, a short description, the two parameters (radius and height, both numbers) and marks both as required.

#### STEP 2:
Send the user’s prompt (e.g. “Find the volume of a cylinder with radius 5 and height 10”) to the Chat Completions API along with the function schema.

The model doesn’t calculate anything. It replies with a function call, containing the function name and the arguments it picked (radius = 5, height = 10).
Read these arguments from the reply, convert them from a JSON string using json.loads(), and pass them to your own function. Your function returns the volume (785.3982).

#### STEP 3:
Add the model’s function call and your function’s result to the conversation. The result goes in a message with the function role.

Call the model a second time with this updated conversation.
The model now writes a friendly final reply, e.g. “The volume of the cylinder is approximately 785.4 cubic units.”

### PROGRAM:
```
import os
import json
import openai
from dotenv import load_dotenv, find_dotenv

_ = load_dotenv(find_dotenv())
openai.api_key = os.environ['OPENAI_API_KEY']

def multiply(a, b):
    """Multiply two numbers"""
    result = a * b
    return json.dumps({"result": result})

functions = [
    {
        "name": "multiply",
        "description": "Multiply two numbers",
        "parameters": {
            "type": "object",
            "properties": {
                "a": {"type": "number", "description": "First number"},
                "b": {"type": "number", "description": "Second number"},
            },
            "required": ["a", "b"],
        },
    }
]

messages = [{"role": "user", "content": "Multiply 125 and 8"}]

response = openai.ChatCompletion.create(
    model="gpt-4o-mini",
    messages=messages,
    functions=functions
)

response_message = response["choices"][0]["message"]
print("LLM response:", response_message)

args = json.loads(response_message["function_call"]["arguments"])
print("Arguments:", args)

result = multiply(**args)
print("Function result:", result)

result = json.loads(result)
print("Result of multiplication:", result["result"])


```

### OUTPUT:
<img width="1607" height="1032" alt="image" src="https://github.com/user-attachments/assets/ebb106e9-703c-4421-8bb7-2ff6f65c6128" />


### RESULT:
Hence,the python program to design and implement a Python function for multiplying two integers,
integrating it with a chat completion system utilizing the function-calling feature of a large language model (LLM) is written successfully and executed.
