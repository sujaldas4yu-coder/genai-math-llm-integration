## Integration of a Mathematical Calulations with a Chat Completion System using LLM Function-Calling

### AIM:
To design and implement a Python function for calculating the volume of a cylinder, integrate it with a chat completion system utilizing the function-calling feature of a large language model (LLM).

### PROBLEM STATEMENT:
Design and implement a system where a user can input integers and perform multiplication,
and the system calculates its volume by invoking a Python function using the function-calling capabilities of an LLM.

### DESIGN STEPS:

#### Step 1: Create the function and describe it to the model

Write a normal Python function, multiply(a, b), that multiplies two numbers and returns the result in JSON form.
Write a JSON schema that tells the model about this function. It gives the function name (multiply), a short description (“Multiply two numbers”), the two parameters (a and b, both numbers) and marks both as required.

#### Step 2: Send the user’s question to the model and run the function

Send the user’s prompt (e.g. “Multiply 125 and 8”) to the Chat Completions API along with the function schema.
The model doesn’t do the multiplication. It replies with a function call, containing the function name (multiply) and the arguments it picked (a = 125, b = 8).
Read these arguments from the reply, convert them from a JSON string using json.loads(), and pass them to your own multiply function. Your function returns the result (1000).

#### Step 3: Show the final answer

Convert the function’s JSON result back into a normal Python value using json.loads().
Print the result: “Result of multiplication: 1000”.
Optionally, to get a friendly sentence from the model, add the function’s result to the conversation using the function role and call the model a second time. It then replies with something like “125 multiplied by 8 is 1000.”

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
