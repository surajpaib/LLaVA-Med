## ClassDef LLM
**LLM**: The function of LLM is to serve as an abstract base class for language model implementations.

**attributes**: The attributes of this Class.
· prompt_percent: A class-level attribute that defines the percentage of the prompt length to be utilized in the model's input.

**Code Description**: The LLM class is an abstract base class that establishes a blueprint for any language model implementation that inherits from it. It defines three abstract methods that must be implemented by any subclass: 

1. **__init__**: This method is intended for initializing instances of the subclass. It raises a NotImplementedError, indicating that subclasses must provide their own initialization logic.

2. **infer**: This method is designed to process input prompts and generate outputs based on the model's inference capabilities. Like the constructor, it raises a NotImplementedError, requiring subclasses to define how inference is performed.

3. **split_input**: This method is responsible for dividing input data into manageable segments based on specified parameters. It also raises a NotImplementedError, compelling subclasses to implement their own logic for input splitting.

The LLM class sets a foundational structure for language models, ensuring that any derived class adheres to a consistent interface. The prompt_percent attribute can be overridden in subclasses to adjust the proportion of the prompt used during inference.

The LLM class is utilized by the GPT class, which inherits from it. The GPT class implements the abstract methods defined in LLM, providing specific functionality for interacting with the OpenAI API. The GPT class sets its own prompt_percent value and includes additional attributes and methods tailored to its operational context, such as managing API connections and handling message formatting for the model's input and output.

**Note**: It is essential for any subclass of LLM to implement all abstract methods to ensure proper functionality. Failure to do so will result in a NotImplementedError when attempting to instantiate the subclass.
### FunctionDef __init__(self)
**__init__**: The function of __init__ is to serve as an initializer for subclasses of the class in which it is defined.

**parameters**: The parameters of this Function.
· There are no parameters for this function.

**Code Description**: The __init__ function is designed to be an abstract initializer for a class. It raises a NotImplementedError, indicating that this function is intended to be overridden by subclasses. This is a common pattern in object-oriented programming, particularly when defining abstract base classes. By raising this exception, the code enforces that any subclass must provide its own implementation of the __init__ method. This ensures that the base class cannot be instantiated directly, which is a crucial aspect of maintaining a clear and structured class hierarchy.

**Note**: It is important for developers to remember that any subclass inheriting from this class must implement its own version of the __init__ method to avoid runtime errors. This design pattern promotes code extensibility and enforces a contract for subclass behavior.
***
### FunctionDef infer(self, prompts)
**infer**: The function of infer is to define an interface for subclasses to implement their own inference logic.

**parameters**: The parameters of this Function.
· prompts: This parameter represents the input data or queries that will be processed by the inference logic implemented in subclasses.

**Code Description**: The infer function is an abstract method that raises a NotImplementedError. This indicates that the function is intended to be overridden by subclasses that inherit from the class containing this method. The purpose of this function is to provide a standard interface for performing inference based on the provided prompts. Since it raises an exception, it enforces that any subclass must implement its own version of the infer method, thereby ensuring that the specific inference logic is defined according to the needs of that subclass. This design pattern is commonly used in object-oriented programming to promote code extensibility and maintainability.

**Note**: It is crucial for developers implementing subclasses to provide a concrete implementation of the infer method. Failure to do so will result in a runtime error when the method is called, as the NotImplementedError will be raised. This method serves as a contract that guarantees that any subclass will have its own inference mechanism tailored to its functionality.
***
### FunctionDef split_input(self, fixed_instruction, few_shot_examples, splittable_input, input_header, output_header)
**split_input**: The function of split_input is to define a method for splitting input data into specified components, which is intended to be implemented by subclasses.

**parameters**: The parameters of this Function.
· fixed_instruction: This parameter is expected to hold a fixed instruction that guides the processing of the input data.
· few_shot_examples: This parameter is intended to contain examples that provide context or guidance for the input processing.
· splittable_input: This parameter represents the actual input data that needs to be split into different components.
· input_header: This parameter is meant to define the header or label for the input data.
· output_header: This parameter is intended to define the header or label for the output data.

**Code Description**: The split_input function is an abstract method that raises a NotImplementedError, indicating that it is designed to be overridden by subclasses. This method serves as a template for subclasses to implement their own logic for splitting input data into various components based on the provided parameters. The parameters suggest that the function is meant to handle structured input, where fixed instructions and examples can guide the processing of the splittable input. The presence of input and output headers indicates that the function may be part of a larger framework that deals with structured data processing, possibly in the context of machine learning or data evaluation.

**Note**: It is important to note that since this function raises a NotImplementedError, it cannot be called directly. Developers must create a subclass that implements this method to provide the specific logic required for their application.
***
## ClassDef GPT
**GPT**: The function of GPT is to provide an implementation of a language model that interacts with the OpenAI API for generating text-based responses.

**attributes**: The attributes of this Class.
· prompt_percent: A class-level attribute that defines the percentage of the prompt length to be utilized in the model's input, set to 0.8.

· openai_cxn_dict: A dictionary containing the connection details for the OpenAI API, including the endpoint and API key.

· deployment_max_length_dict: A dictionary that maps different model IDs to their respective maximum token lengths for input.

· temperature: A parameter that controls the randomness of the model's output, initialized to 0.0.

· top_k: A parameter that determines the number of highest probability vocabulary tokens to keep for sampling, initialized to 1.

· encoding: An encoding object that is used to convert text into token IDs based on the specified model ID.

· openai_api: A string that specifies the API connection to be used, defaulting to 'default'.

· model_id: A string representing the specific model identifier being used.

· max_length: An integer that defines the maximum length of the input based on the model ID.

· client: An instance of the AsyncAzureOpenAI client used to make asynchronous calls to the OpenAI API.

**Code Description**: The GPT class inherits from the LLM abstract base class and implements its abstract methods to provide specific functionality for interacting with the OpenAI API. Upon initialization, the class sets various parameters such as temperature, top_k, and encoding based on the provided model ID. It also establishes a connection to the OpenAI API using the specified endpoint and API key.

The class contains several key methods:

1. **gen_messages**: This method generates a structured list of messages formatted for the OpenAI API based on fixed instructions, few-shot examples, and user input. It organizes the messages into a format that the API expects, including roles for system, user, and assistant.

2. **make_api_call_to_gpt**: This coroutine method makes an asynchronous call to the OpenAI API using the client instance. It handles rate limit errors with a backoff strategy and returns the content of the assistant's response.

3. **dispatch_openai_requests**: This method takes a list of message sets and asynchronously calls the API for each set of messages, gathering the results concurrently.

4. **infer**: This method serves as a synchronous interface for users to invoke the model's inference capabilities. It runs the asynchronous request dispatching method and returns the results.

5. **split_input**: This method is responsible for tokenizing the input and splitting it into manageable segments based on the maximum token length allowed for the specified model. It ensures that the input does not exceed the model's constraints while maintaining the integrity of the message structure.

The GPT class is utilized in the `infer` function found in the `llava/eval/eval_multimodal_chat_gpt_score.py` file. In this context, an instance of the GPT class is created with a specific model ID (e.g., "gpt-4-0314"). The function processes a list of samples, generating input messages for each sample and invoking the GPT model's inference method to obtain evaluation results. The results are then compiled and returned, demonstrating the practical application of the GPT class in a scoring evaluation scenario.

**Note**: It is essential to provide valid Azure OpenAI endpoint and API key in the `openai_cxn_dict` for the class to function correctly. Additionally, users should ensure that the model ID used is supported and that the input does not exceed the defined maximum token length.

**Output Example**: A possible return value from the `infer` method could be a list of strings, each representing the assistant's response to the input prompts, such as:
```
[
    "The figure illustrates the relationship between X and Y, showing that as X increases, Y tends to decrease.",
    "In summary, the analysis indicates a strong correlation between the two variables."
]
```
### FunctionDef __init__(self, model_id)
**__init__**: The function of __init__ is to initialize an instance of the GPT class with specific model parameters and configurations.

**parameters**: The parameters of this Function.
· model_id: A string representing the identifier of the model to be used.

**Code Description**: The __init__ function is a constructor for the GPT class, which sets up the initial state of an instance when it is created. It takes a single parameter, model_id, which is used to configure various attributes of the instance. 

Upon invocation, the function initializes several instance variables:
- `self.temperature`: This is set to 0.0, which typically controls the randomness of the model's output. A lower value results in more deterministic outputs.
- `self.top_k`: This is set to 1, which is often used in sampling methods to limit the number of potential next tokens to consider, effectively making the model's predictions more focused.
- `self.encoding`: This variable is assigned the encoding for the specified model. It uses the `tiktoken.encoding_for_model` function, which processes the model_id to derive the appropriate encoding. The model_id is modified by replacing the first two segments with a hyphen and changing any '5' to '.5'.
- `self.openai_api`: This is set to 'default', indicating the default API configuration to be used for OpenAI services.
- `self.model_id`: This stores the model_id passed to the constructor for later reference within the instance.
- `self.max_length`: This retrieves the maximum length of the input that the model can handle, based on a predefined dictionary (`deployment_max_length_dict`) that maps model_ids to their respective maximum lengths.
- `self.client`: This initializes an asynchronous client for Azure OpenAI services using the `openai.AsyncAzureOpenAI` class. It requires an API key and endpoint, both of which are retrieved from a dictionary (`openai_cxn_dict`) that contains connection details for the specified API.

**Note**: It is important to ensure that the model_id provided is valid and corresponds to an entry in the deployment_max_length_dict and openai_cxn_dict to avoid runtime errors. Additionally, the temperature and top_k parameters can be adjusted based on the desired output characteristics of the model.
***
### FunctionDef gen_messages(self, fixed_instruction, few_shot_examples, input, input_header, output_header)
**gen_messages**: The function of gen_messages is to generate a structured list of messages formatted for a conversational AI model based on fixed instructions, few-shot examples, and user input.

**parameters**: The parameters of this Function.
· parameter1: fixed_instruction - A string that provides the system's fixed instruction for the conversation context.
· parameter2: few_shot_examples - A list of dictionaries containing example user-assistant interactions to guide the model's responses.
· parameter3: input - A string representing the user's input that needs to be processed.
· parameter4: input_header - A string that serves as a header for the user input in the generated messages.
· parameter5: output_header - A string that serves as a header for the expected output in the generated messages.

**Code Description**: The gen_messages function constructs a list of messages that are formatted for interaction with a conversational AI model. It begins by creating a message with the role of "system" that includes the provided fixed_instruction. Following this, it iterates through the few_shot_examples, appending a pair of messages for each example: one from the user that combines the input_header, the user content from the example, and the output_header, and another from the assistant containing the assistant's response from the example. After processing the few-shot examples, the function adds a final user message that incorporates the input provided by the user, along with the input_header and output_header.

This function is called by the split_input function, which is responsible for splitting a potentially long input into manageable segments. The split_input function first tokenizes the fixed_instruction and few_shot_examples to determine how much additional input can be accommodated based on the model's token limit. It then splits the user input into smaller segments and calls gen_messages for each segment, thereby generating a structured prompt for each piece of input. This relationship ensures that the generated messages are contextually relevant and formatted correctly for the conversational AI to process.

**Note**: When using this function, it is important to ensure that the fixed_instruction and few_shot_examples are well-defined to provide clear guidance to the AI model. The input_header and output_header should also be appropriately set to maintain clarity in the generated messages.

**Output Example**: An example of the return value from gen_messages might look like this:
[
  {"role": "system", "content": "You are a helpful assistant."},
  {"role": "user", "content": "User input header\nWhat is the capital of France?\n\nExpected output header"},
  {"role": "assistant", "content": "The capital of France is Paris."},
  {"role": "user", "content": "User input header\nCan you tell me about the Eiffel Tower?\n\nExpected output header"}
]
***
### FunctionDef make_api_call_to_gpt(self, messages)
**make_api_call_to_gpt**: The function of make_api_call_to_gpt is to asynchronously send a chat completion request to the GPT model and return the generated response.

**parameters**: The parameters of this Function.
· messages: A list of message objects that represent the conversation history or prompts to be sent to the GPT model.

**Code Description**: The make_api_call_to_gpt function is designed to facilitate interaction with the GPT model by making an asynchronous API call. It takes a single parameter, messages, which is expected to be a list of message objects. This function utilizes the client object to send a request to the chat completions API of the specified model (identified by self.model_id). The temperature parameter, which influences the randomness of the model's responses, is also included in the request.

Upon execution, the function awaits the response from the API call. Once the response is received, it extracts the content of the first message from the choices provided in the response and returns it. This function is crucial for obtaining responses from the GPT model based on the provided conversation context.

The make_api_call_to_gpt function is called by the dispatch_openai_requests function. In dispatch_openai_requests, a list of message lists (messages_list) is processed, where each list of messages is passed to make_api_call_to_gpt. This allows for concurrent processing of multiple requests to the GPT model, enhancing efficiency and reducing wait times for responses. The results from all the API calls are gathered and returned as a list, making it suitable for handling multiple prompts in a single operation.

**Note**: It is important to ensure that the messages parameter is formatted correctly as expected by the GPT model. Additionally, the function is asynchronous, so it should be called within an async context to avoid runtime errors.

**Output Example**: A possible appearance of the code's return value could be:
"Hello! How can I assist you today?"
***
### FunctionDef dispatch_openai_requests(self, messages_list)
**dispatch_openai_requests**: The function of dispatch_openai_requests is to asynchronously process a list of message prompts and return the generated responses from the GPT model.

**parameters**: The parameters of this Function.
· messages_list: A list of lists, where each inner list contains message objects representing the conversation history or prompts to be sent to the GPT model.

**Code Description**: The dispatch_openai_requests function is designed to facilitate the concurrent processing of multiple requests to the GPT model. It takes a single parameter, messages_list, which is expected to be a list of lists, with each inner list containing messages that represent a conversation or prompt for the model.

Within the function, a list comprehension is used to create a list of tasks, where each task is an asynchronous call to the make_api_call_to_gpt function. This function is called with each inner list of messages from messages_list. The use of asynchronous programming allows for multiple API calls to be initiated without waiting for each to complete sequentially, thereby improving efficiency and reducing overall wait times for responses.

The asyncio.gather function is then employed to run all the tasks concurrently. This function collects the results of the asynchronous calls once they are completed and returns them as a list. The results correspond to the responses generated by the GPT model for each set of messages provided.

The dispatch_openai_requests function is called by the infer function. In this context, the infer function serves as a synchronous wrapper that allows users to call dispatch_openai_requests in a blocking manner by using asyncio.run. This means that when infer is invoked with a messages_list, it will execute dispatch_openai_requests, wait for all responses to be gathered, and return them to the caller.

**Note**: It is essential to ensure that the messages_list is structured correctly, with each inner list formatted as expected by the GPT model. Additionally, since dispatch_openai_requests is an asynchronous function, it must be called within an appropriate asynchronous context or through a synchronous wrapper like infer to avoid runtime errors.

**Output Example**: A possible appearance of the code's return value could be:
["Hello! How can I assist you today?", "What information are you looking for?"]
***
### FunctionDef infer(self, messages_list)
**infer**: The function of infer is to provide a synchronous interface for processing a list of message prompts and obtaining responses from the GPT model.

**parameters**: The parameters of this Function.
· messages_list: A list of lists, where each inner list contains message objects representing the conversation history or prompts to be sent to the GPT model.

**Code Description**: The infer function serves as a synchronous wrapper around the asynchronous dispatch_openai_requests function. It takes a single parameter, messages_list, which is expected to be structured as a list of lists. Each inner list should contain message objects that represent either the conversation history or specific prompts intended for the GPT model.

When the infer function is called, it utilizes asyncio.run to execute the dispatch_openai_requests function, effectively blocking the execution until all asynchronous tasks initiated by dispatch_openai_requests are completed. This allows the caller to receive the results in a straightforward manner, as if the function were synchronous.

The dispatch_openai_requests function, which is invoked within infer, is responsible for handling the actual asynchronous processing of the message prompts. It creates a list of tasks for each inner list of messages and uses asyncio.gather to run these tasks concurrently. The results returned from dispatch_openai_requests are then passed back to the caller of infer.

This design enables efficient handling of multiple requests to the GPT model without the need for the caller to manage asynchronous programming directly. By providing a synchronous interface, infer simplifies the interaction with the underlying asynchronous functionality, making it more accessible for users who may not be familiar with async programming patterns.

**Note**: It is crucial to ensure that the messages_list is formatted correctly, with each inner list containing the appropriate message objects. This structure is necessary for the successful processing of requests by the GPT model. Additionally, since infer is a synchronous function that wraps an asynchronous call, it should be used in contexts where blocking behavior is acceptable.

**Output Example**: A possible appearance of the code's return value could be:
["Hello! How can I assist you today?", "What information are you looking for?"]
***
### FunctionDef split_input(self, fixed_instruction, few_shot_examples, splittable_input, input_header, output_header)
**split_input**: The function of split_input is to divide a long user input into smaller segments while maintaining the context provided by fixed instructions and few-shot examples.

**parameters**: The parameters of this Function.
· parameter1: fixed_instruction - A string that provides the system's fixed instruction for the conversation context.  
· parameter2: few_shot_examples - A list of dictionaries containing example user-assistant interactions to guide the model's responses.  
· parameter3: splittable_input - A string representing the user's input that needs to be processed and potentially split into smaller segments.  
· parameter4: input_header - A string that serves as a header for the user input in the generated messages.  
· parameter5: output_header - A string that serves as a header for the expected output in the generated messages.  

**Code Description**: The split_input function is designed to manage the input length for a conversational AI model by splitting a potentially lengthy user input into manageable segments. It begins by tokenizing the fixed_instruction and the concatenated user-assistant pairs from the few_shot_examples. This tokenization helps determine how many tokens can be allocated to the user input based on the model's maximum token limit, which is adjusted by a specified percentage (prompt_percent). 

The function calculates the remaining token length available for the user input after accounting for the fixed_instruction and few-shot examples. It then tokenizes the splittable_input and divides it into smaller segments based on the calculated remaining token length. Each segment is further decoded back into a string format.

Subsequently, the function generates a list of structured prompt strings by calling the gen_messages function for each segment of the split input. This ensures that each segment is formatted correctly and retains the necessary context provided by the fixed_instruction and few-shot examples. The relationship between split_input and gen_messages is crucial, as split_input prepares the input for gen_messages, which formats it for the conversational AI model.

**Note**: When using this function, it is essential to ensure that the fixed_instruction and few_shot_examples are well-defined and relevant to the context of the conversation. The input_header and output_header should also be clearly articulated to maintain clarity in the generated messages.

**Output Example**: An example of the return value from split_input might look like this:
[
  [
    {"role": "system", "content": "You are a helpful assistant."},
    {"role": "user", "content": "User input header\nWhat is the capital of France?\n\nExpected output header"},
    {"role": "assistant", "content": "The capital of France is Paris."}
  ],
  [
    {"role": "system", "content": "You are a helpful assistant."},
    {"role": "user", "content": "User input header\nCan you tell me about the Eiffel Tower?\n\nExpected output header"},
    {"role": "assistant", "content": "The Eiffel Tower is located in Paris."}
  ]
]
***
