## Integration of a Mathematical Calulations with a Chat Completion System using LLM Function-Calling

### AIM:
To design and implement a Python function for calculating the volume of a cylinder, integrate it with a chat completion system utilizing the function-calling feature of a large language model (LLM).

### PROBLEM STATEMENT:

### DESIGN STEPS:

#### STEP 1:

#### STEP 2:

#### STEP 3:

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
