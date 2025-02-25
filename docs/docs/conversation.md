## ClassDef SeparatorStyle
**SeparatorStyle**: The function of SeparatorStyle is to define different styles of separators used in conversation formatting.

**attributes**: The attributes of this Class.
· SINGLE: Represents a single separator style.
· TWO: Represents a two-separator style.
· MPT: Represents a multi-part separator style.
· PLAIN: Represents a plain text separator style.
· LLAMA_2: Represents a separator style specific to the LLAMA 2 model.
· MISTRAL: Represents a separator style specific to the Mistral model.

**Code Description**: The SeparatorStyle class is an enumeration that encapsulates various styles of separators that can be used in formatting conversation prompts. Each member of the enum corresponds to a specific style that can be utilized by other components in the system, particularly in the Conversation class. The Conversation class utilizes the SeparatorStyle to determine how to format the conversation history when generating prompts for model inference.

In the Conversation class, the sep_style attribute is defined as an instance of SeparatorStyle, with a default value of SeparatorStyle.SINGLE. This attribute is crucial as it dictates how the messages are concatenated and presented in the final output. The get_prompt method within the Conversation class checks the value of sep_style to format the conversation accordingly. For example, if sep_style is set to SeparatorStyle.SINGLE, the method will concatenate messages with a single separator, while if it is set to SeparatorStyle.TWO, it will alternate between two different separators.

The various styles defined in SeparatorStyle allow for flexibility in how conversations are structured and presented, catering to different model requirements and user preferences. This is particularly important in applications where the format of the input can significantly affect the performance and output of the underlying models.

**Note**: When using the SeparatorStyle enumeration, it is essential to ensure that the chosen style aligns with the expected input format of the model being utilized. Each style may have specific implications on how the conversation history is interpreted by the model, thus affecting the quality of the generated responses.
## ClassDef Conversation
**Conversation**: The function of Conversation is to manage and format the history of interactions in a conversation, including messages and roles.

**attributes**: The attributes of this Class.
· system: A string representing the system message that sets the context for the conversation.
· roles: A list of strings defining the roles involved in the conversation (e.g., user, assistant).
· messages: A list of lists, where each inner list contains a role and a corresponding message.
· offset: An integer indicating the starting point for message retrieval.
· sep_style: An instance of SeparatorStyle that determines the formatting style for separators in the conversation.
· sep: A string representing the primary separator used in formatting messages.
· sep2: A string representing a secondary separator used in formatting messages, if applicable.
· version: A string indicating the version of the conversation format.
· skip_next: A boolean flag that indicates whether to skip the next message in the conversation.

**Code Description**: The Conversation class is designed to encapsulate the entire history of a conversation, allowing for the storage, retrieval, and formatting of messages exchanged between different roles. It maintains a structured format for messages, which can include both text and images, and provides various methods to manipulate and retrieve this data.

The get_prompt method is a key function within the class, responsible for generating a formatted string representation of the conversation history based on the specified separator style (sep_style). It handles different formatting styles defined in the SeparatorStyle enumeration, such as SINGLE, TWO, MPT, PLAIN, LLAMA_2, and MISTRAL. Each style dictates how messages are concatenated and presented, ensuring flexibility in how the conversation is structured for different model requirements.

The append_message method allows for the addition of new messages to the conversation, while the get_images method extracts images from the messages, processing them according to specified modes (e.g., resizing, padding). The to_gradio_chatbot method formats the conversation for use with Gradio, a popular library for creating user interfaces for machine learning models.

Additionally, the copy method creates a duplicate of the current Conversation instance, preserving its state, and the dict method returns a dictionary representation of the conversation, which can be useful for serialization or further processing.

The relationship with the SeparatorStyle class is crucial, as it provides the necessary formatting options that the Conversation class relies on to present messages in a coherent manner. The choice of separator style can significantly impact how the conversation is interpreted by downstream models, making it essential for developers to select the appropriate style based on their specific use case.

**Note**: When utilizing the Conversation class, it is important to ensure that the messages are structured correctly and that the chosen separator style aligns with the expected input format of any models that will process the conversation. This alignment is vital for maintaining the quality and relevance of the generated responses.

**Output Example**: An example output from the get_prompt method might look like this:

```
System: Welcome to the chat!
User: Hello, how can I help you today?###
Assistant: I'm looking for information on your services.###
User: Can you provide details about pricing?###
Assistant: Sure! Our pricing starts at $99 per month.###
```
### FunctionDef get_prompt(self)
**get_prompt**: The function of get_prompt is to generate a formatted conversation prompt based on the stored messages and the specified separator style.

**parameters**: The parameters of this Function.
· None

**Code Description**: The get_prompt method is a crucial component of the Conversation class, responsible for constructing a formatted string that represents the conversation history. This method retrieves the messages stored in the instance and formats them according to the specified separator style, which is defined by the SeparatorStyle enumeration.

The method begins by checking if there are any messages in the conversation. If the first message is a tuple, it processes the initial message to remove any image tags and prepares it for formatting. Depending on the version of the conversation, it may prepend an image placeholder or a confirmation message.

The formatting of the messages is determined by the sep_style attribute, which can take on various values from the SeparatorStyle enumeration, including SINGLE, TWO, MPT, PLAIN, LLAMA_2, and MISTRAL. Each style dictates a different way of concatenating the messages:

- **SINGLE**: Concatenates messages using a single separator.
- **TWO**: Alternates between two different separators for each message.
- **MPT**: Similar to SINGLE but may have additional formatting considerations.
- **LLAMA_2**: Formats messages specifically for the LLAMA 2 model, wrapping system and instruction messages in specific tags.
- **MISTRAL**: Formats messages for the Mistral model, ensuring the first message is treated as a system message and subsequent messages are wrapped appropriately.

The method also includes error handling to raise a ValueError if an invalid separator style is encountered. The final output is a string that represents the entire conversation history, formatted according to the specified style.

The get_prompt method is called within the main function of the llava/serve/cli.py module. In this context, it is used to generate the prompt that will be fed into the model for inference. After user input is collected and appended to the conversation, get_prompt is invoked to create the formatted prompt, which is then tokenized and processed by the model. This integration highlights the method's role in bridging user interactions with the underlying model's processing capabilities.

**Note**: When utilizing the get_prompt method, it is essential to ensure that the messages are structured correctly and that the sep_style is set appropriately to match the expected input format of the model being used.

**Output Example**: An example output of the get_prompt method might look like this:
```
<<SYS>>
This is the system message.
<</SYS>>

[INST] User: Hello, how are you? [/INST]
 Assistant: I'm doing well, thank you!
```
***
### FunctionDef append_message(self, role, message)
**append_message**: The function of append_message is to add a new message to the conversation history along with the associated role of the message sender.

**parameters**: The parameters of this Function.
· parameter1: role - A string indicating the role of the message sender (e.g., 'user' or 'assistant').
· parameter2: message - A string containing the content of the message to be appended.

**Code Description**: The append_message function is a method of the Conversation class that is responsible for updating the conversation history by appending a new message. It takes two parameters: 'role', which specifies the sender of the message, and 'message', which contains the actual text of the message. The function appends a list containing the role and message to the 'messages' attribute, which is expected to be a list that maintains the sequence of messages exchanged during the conversation.

This function is called within the main function of the llava/serve/cli.py file. In this context, it is used to record both user inputs and assistant responses during an interactive conversation session. When the user provides input, the append_message function is invoked to store the user's message along with the role 'user'. Subsequently, after the assistant generates a response, the function is called again to append the assistant's role and its corresponding message (which may initially be set to None until the output is generated). This ensures that the conversation history is accurately maintained and can be referenced later for generating prompts or responses.

**Note**: It is important to ensure that the 'messages' attribute is properly initialized as a list before calling this function. Additionally, the role and message parameters should be validated to prevent any inconsistencies in the conversation history.
***
### FunctionDef get_images(self, return_pil)
**get_images**: The function of get_images is to retrieve and process images from the conversation messages.

**parameters**: The parameters of this Function.
· return_pil: A boolean flag indicating whether to return the images as PIL Image objects (True) or as base64 encoded strings (False).

**Code Description**: The get_images function iterates through the messages stored in the Conversation object, starting from a specified offset. It processes messages that are tuples, which contain an image and its associated processing mode. The function handles various image processing modes, including "Pad," "Crop," "Resize," and "Default." 

For the "Pad" mode, the function expands the image to a square format while maintaining the aspect ratio and filling the background with a specified color. In the "Resize" mode, the image is resized to a fixed dimension of 336x336 pixels. The function also calculates the appropriate dimensions for the image based on its aspect ratio and ensures that the longest edge does not exceed a maximum length of 800 pixels or a minimum length of 400 pixels.

The processed images are collected in a list, which can either contain PIL Image objects or base64 encoded strings, depending on the value of the return_pil parameter. The function ultimately returns this list of images.

This function is called by other methods within the Conversation class, such as the dict method. The dict method checks if there are any images available by calling get_images. If images are present, it includes relevant information in the returned dictionary, which can be used for further processing or display in the application.

**Note**: It is important to ensure that the image processing mode provided in the messages is valid, as the function raises a ValueError for any invalid modes. Additionally, users should be aware of the potential memory implications when returning PIL Image objects, especially if many images are processed at once.

**Output Example**: An example return value when calling get_images with return_pil set to False might look like this:
```
[
    "iVBORw0KGgoAAAANSUhEUgAAAAUA...",
    "iVBORw0KGgoAAAANSUhEUgAAAAUA..."
]
```
If return_pil is set to True, the output would be a list of PIL Image objects instead.
#### FunctionDef expand2square(pil_img, background_color)
**expand2square**: The function of expand2square is to resize an image to a square format by adding a background color if necessary.

**parameters**: The parameters of this Function.
· parameter1: pil_img - This is a PIL (Python Imaging Library) image object that needs to be resized to a square format.
· parameter2: background_color - This is an optional parameter that defines the color of the background to be added when resizing the image. It defaults to a tuple representing an RGB color (122, 116, 104).

**Code Description**: The expand2square function takes a PIL image and checks its dimensions. If the image is already square (width equals height), it returns the original image without any modifications. If the image is wider than it is tall, the function creates a new square image with the same width as the original image, filling the background with the specified background color. The original image is then pasted onto this new image, centered vertically. Conversely, if the image is taller than it is wide, the function creates a new square image with the same height, again filling the background with the specified color. The original image is pasted onto this new image, centered horizontally. This ensures that the output image is always square, with the original image centered and the background color filling any extra space.

**Note**: When using this function, ensure that the input is a valid PIL image object. The background_color parameter can be customized to any RGB color, allowing for flexibility in the appearance of the output image.

**Output Example**: If the input image is a rectangle with dimensions 200x100 and the background color is set to white (255, 255, 255), the output will be a 200x200 image with the original image centered and the top and bottom areas filled with white.
***
***
### FunctionDef to_gradio_chatbot(self)
**to_gradio_chatbot**: The function of to_gradio_chatbot is to convert the conversation messages into a format suitable for display in a Gradio chatbot interface.

**parameters**: The parameters of this Function.
· None

**Code Description**: The to_gradio_chatbot function processes the messages stored in the conversation object, starting from a specified offset. It iterates through the messages, distinguishing between user and assistant messages based on their roles. For every user message (even indexed), if the message contains an image (indicated by being a tuple), it performs several operations to resize the image and convert it into a base64-encoded string. This string is then embedded in an HTML image tag, which is concatenated with the original message text (after removing the placeholder '<image>'). If the message does not contain an image, it simply appends the message text to the return list.

For every assistant message (odd indexed), it updates the last entry in the return list to include the assistant's response. The function ultimately returns a list of lists, where each inner list contains a message and its corresponding assistant response (or None if there is no response yet).

This function is called by several other functions in the project, specifically clear_history, add_text, and http_bot. Each of these functions utilizes to_gradio_chatbot to prepare the state of the conversation for display in the Gradio interface after modifying the conversation state. For instance, clear_history resets the conversation and returns the initial state along with the formatted messages for the chatbot. Similarly, add_text processes new user input and updates the conversation state before calling to_gradio_chatbot to reflect these changes in the chatbot interface. The http_bot function also uses this function to yield the current state of the conversation during an ongoing interaction with the model.

**Note**: It is important to ensure that the messages are formatted correctly before calling this function, as it relies on the structure of the messages to function properly. Additionally, the function assumes that images are processed as PIL Image objects when present.

**Output Example**: An example of the return value from to_gradio_chatbot could look like this:
```
[
    ["Hello, how can I help you today?", None],
    ["I need assistance with my order.", "Sure, can you provide your order number?"],
    ["<img src='data:image/png;base64,...' alt='user upload image' /> I have attached an image for reference.", "Thank you for the image."]
]
```
***
### FunctionDef copy(self)
**copy**: The function of copy is to create a duplicate of the current Conversation object with all its properties.

**parameters**: The parameters of this Function.
· None

**Code Description**: The copy function is a method defined within the Conversation class. Its primary purpose is to return a new instance of the Conversation class that is a duplicate of the current instance. This is achieved by passing the current instance's attributes such as system, roles, messages, offset, sep_style, sep, sep2, and version to the constructor of the Conversation class.

The messages attribute is particularly noteworthy as it is transformed into a list of lists, where each inner list contains two elements from the original messages. This ensures that the structure of the messages is preserved in the copied instance. 

The copy function is utilized in various parts of the project, particularly in the context of conversation management. For instance, in the load_demo function, a copy of the default_conversation is created to initialize the state of the conversation when loading a demo. Similarly, in the load_demo_refresh_model_list and clear_history functions, the copy function is called to reset the conversation state to its default configuration. This indicates that the copy function plays a crucial role in maintaining the integrity of conversation states across different operations, allowing for a fresh start without altering the original conversation instance.

**Note**: It is important to ensure that the copy function is called when a new conversation state is needed, as it prevents unintended modifications to the original conversation instance.

**Output Example**: A possible appearance of the code's return value could be:
```
Conversation(
    system='default_system',
    roles=['user', 'assistant'],
    messages=[['Hello', 'Hi'], ['How can I help you?', None]],
    offset=0,
    sep_style='default_style',
    sep=':',
    sep2=';',
    version='1.0'
)
```
***
### FunctionDef dict(self)
**dict**: The function of dict is to generate a dictionary representation of the conversation state, including system information, roles, messages, and offsets.

**parameters**: The parameters of this Function.
· None

**Code Description**: The dict function constructs and returns a dictionary that encapsulates the current state of the conversation. It first checks if there are any images associated with the conversation by invoking the get_images method. If images are present, the function includes a modified version of the messages where each message is paired with its corresponding image, if applicable. The messages are formatted such that if a message is a tuple, the first element (the message itself) is retained, and the second element (the image) is included only if it is a tuple; otherwise, it is included as is.

The returned dictionary contains the following keys:
- "system": This holds the system information related to the conversation.
- "roles": This contains the roles defined in the conversation.
- "messages": This is a list of messages formatted as described above.
- "offset": This indicates the current offset in the conversation messages.
- "sep": This is a separator string used in the conversation.
- "sep2": This is an alternative separator string.

If no images are found, the function returns a simpler dictionary that includes the system information, roles, and messages without any modifications related to images.

The dict function is called by other components in the project, specifically within the http_bot function located in the gradio_web_server.py file. In this context, the dict function is used to log the state of the conversation, including the images processed during the interaction. This logging occurs after the conversation has been processed and is essential for maintaining a record of the conversation state, which can be useful for debugging and analysis.

**Note**: It is important to ensure that the messages are correctly formatted and that the get_images method is functioning as expected, as the output of dict relies on these components. Additionally, users should be aware that the structure of the messages may affect the output of the dict function.

**Output Example**: A possible return value of the dict function could look like this:
```
{
    "system": "Chatbot System",
    "roles": ["User", "Bot"],
    "messages": [["User", "Hello!"], ["Bot", "Hi there!"]],
    "offset": 0,
    "sep": "###",
    "sep2": "---"
}
```
***
