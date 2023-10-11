# AWS-AIChatbot
## Overview
This chatbot is powered by Amazon Lex, an advanced AI service designed for building conversational interfaces. It uses deep learning to provide automatic speech recognition (ASR) and natural language understanding (NLU), enabling the bot to engage in natural dialogues with users. Integrated with AWS Lambda, a serverless compute service, the system can execute code in response to events, extending the chatbot's capabilities beyond simple interactions. This combination allows for real-time data processing, complex decision-making, and seamless user engagement without the need for extensive infrastructure management.Together, Amazon Lex and AWS Lambda form a robust, scalable platform that supports everything from basic Q&A chatbots to complex enterprise solutions, delivering responsive, intuitive, and interactive experiences to end-users.
## Architecture 
![AI Chatbot drawio](https://github.com/Shereefo/AWS-AIChatbot/assets/137960467/cf6c10ac-5e71-4486-9cfa-9b54188f3d88)

## Project Introduction 
For this project I utilized Amazon Lex for the purpose/scenario of facillitating user ice cream orders. The secondary AWS service I used was Lambda to add specific configurations according to user inputs and requests. Initially I edited the configuration using the Lex console before using the Lambda function to set up the basis of the chatbot. 
This required the creation of: 
1. Intents: Represents what the user wants to achieve when they interact with a chatbot. It captures the purpose or goal of the user's input. 
2. Utterances: Phrases that users might say to invoke the intent.
3. Slots: Data points the bot needs to fulfill the intent.
I am going to walk you through these building blocks of the chatbot and their specific configurations.
## Create the Bot and add a Welcome Intent
 <img width="1116" alt="Screenshot 2023-10-09 at 2 11 51 PM" src="https://github.com/Shereefo/AWS-AIChatbot/assets/137960467/6513012a-86ad-4914-aacb-63022a4822d0">
After creating the bot we are going to create the first intent, this will be called the welcome intent. 

<img width="1412" alt="Screenshot 2023-10-09 at 2 16 58 PM" src="https://github.com/Shereefo/AWS-AIChatbot/assets/137960467/15fbecd0-c151-4403-9c21-aa91f653054e">

Then input some sample utterances to this intent. Try to think what the user would say to initiate a conversation with the bot. 

<img width="797" alt="Screenshot 2023-10-09 at 7 13 31 PM" src="https://github.com/Shereefo/AWS-AIChatbot/assets/137960467/8db52e7d-5109-452f-a1a4-02527f58a57d">

Then below in message group you can define a text message group to respond. You can input something like "okay" in the message box. Then proceed to advanced options. In the top right corner click add, then add a card group. A card group is a set of response cards shown to users. Each card can display information like a title, image, and buttons. For example, we are using this card group to present buttons that offer you a choice. It offers a visual and interactive way for users to engage with the chatbot's responses. 

<img width="1104" alt="Screenshot 2023-10-09 at 2 26 13 PM" src="https://github.com/Shereefo/AWS-AIChatbot/assets/137960467/74cb4c90-b201-4928-837b-66520b74010a">

This is what it should look in the chat after configured. (Remember to build the bot then click test once configured) 

<img width="1127" alt="Screenshot 2023-10-09 at 2 25 30 PM" src="https://github.com/Shereefo/AWS-AIChatbot/assets/137960467/ac6282bd-521c-4a40-87ee-fc9a5ac4f845">

After creating the welcome intent for the first contact with the chatbot create a new intent called the "create order intent", which is used for creating the ice cream order. 
1. Just like the first intent (welcome intent) you will want to add more sample utterances. Again, think what would the user say if they wanted to create an order.
2. Add a response for the chatbot under message group like "sure."
3. Now you will want to add a slot which is used to capture information from the user to fulfill the intent
4. Configure the slot to say, "what is your name" for example
5. Then identify a slot type, in this case you would select AMAZON.FirstName
6. Create another slot named "flavor" for the flavor of ice cream, the slot type should be AMAZON.AlphaNumeric. You can include a prompt like "what flavor?" 

<img width="793" alt="Screenshot 2023-10-09 at 2 36 37 PM" src="https://github.com/Shereefo/AWS-AIChatbot/assets/137960467/dd764024-ced2-47cf-897c-941b628ca9ee">

This slot will also have a card group for the selection of an ice cream flavor:   

<img width="824" alt="Screenshot 2023-10-09 at 2 41 31 PM" src="https://github.com/Shereefo/AWS-AIChatbot/assets/137960467/3f16abd9-a525-4053-9aea-ec6431e7c313">

After creating card group and a button for each flavor, now the user can pick and choose what they want, it is time to configure the desired size of the ice cream by adding slot type.  Navigate to slot types on the tabs in the left hand side, it is below intents. There you can add slot values, which will be the sizes of the ice cream ordered. 

<img width="810" alt="Screenshot 2023-10-09 at 2 51 22 PM" src="https://github.com/Shereefo/AWS-AIChatbot/assets/137960467/a940c1aa-0312-4daf-a557-7cc45b1cca95">

1. After creating the slot type you can then add a new slot to the create order intent, called "size".
2. when editing the slot settings, change the name to "size" and the slot type to the one we just created - SizeSlotType
3. For the prompt, that the chatbot replies to the user can be something like "What size - Small/Medium/Large?"

Alternativley you can provide the user a choice to confirm or decline using a **confirmation** prompt. 

<img width="801" alt="Screenshot 2023-10-09 at 3 00 55 PM" src="https://github.com/Shereefo/AWS-AIChatbot/assets/137960467/29db7b43-4567-452f-9016-fbbade77aa1f">

Next you will need to configure a **fulfillment** this is needed to inform the user when the status, when the order is complete. So for example you can say something like "Order successfully placed" after confirming the order. Alternativley in case of a failure or error processing you can say something like, "Oops! Something went wrong. Please try again later." This can be configured using a Lambda function which will be shown later. 

After you should add a **closing response** for the bot to define the response when closing the intent. 

<img width="797" alt="Screenshot 2023-10-09 at 3 44 45 PM" src="https://github.com/Shereefo/AWS-AIChatbot/assets/137960467/8fdcef1a-f932-40bf-ba17-4aa777f4dd9c">

After making these configuration changes you can test the bot. Click build, then test. You will see the changes made to your chat:  

<img width="357" alt="Screenshot 2023-10-09 at 3 46 52 PM" src="https://github.com/Shereefo/AWS-AIChatbot/assets/137960467/91bfc335-ff0d-4dbc-b5dd-0e15d0517d3b">

# Cancel Order Intent 
1. Next is to create a new intent, to cancel an order in case the user changes their mind after making an order. Add utterances pertaining to what a user might say if they wanted to cancel an order for example: "I would like to cancel my order"
2. Next create a slot for this intent, name it "OrderNo"
3. Select the AMAZON.AlphaNumeric slot type
4. Under prompts you can say something like: "Please provide the order number you would like to cancel
After configuring this slot type you can add it to an utterance, shown below: 

<img width="808" alt="Screenshot 2023-10-09 at 3 52 57 PM" src="https://github.com/Shereefo/AWS-AIChatbot/assets/137960467/9638b334-1c0d-4364-b56f-cca80deb5a86">

Now you can test this intent after inputting responses to the intent's confirmation and fulfillment. This is what it should look like in the chat: 

<img width="1137" alt="Screenshot 2023-10-09 at 3 58 17 PM" src="https://github.com/Shereefo/AWS-AIChatbot/assets/137960467/c8c19de6-d617-4f1a-bf87-3476a816d3ef">


# Lambda Integration 

Amazon Lex provides a robust platform for building conversational interfaces, but to truly unlock its potential, it often requires dynamic data processing and decision-making. This is where AWS Lambda comes into play. By integrating Lambda functions with Amazon Lex, developers can execute custom code in response to user inputs, allowing for real-time data retrieval, complex computations, or even database interactions. This seamless integration ensures that the chatbot can provide dynamic, context-aware responses, elevating the user experience from a simple Q&A interaction to a more personalized and intelligent conversation. In essence, using Lambda with Amazon Lex bridges the gap between static chatbot responses and dynamic, adaptable conversations.

First you will want to configure this change under fulfillment when you are switching to Lambda, for both the create order and cancel order intent: 

<img width="841" alt="Screenshot 2023-10-09 at 5 10 20 PM" src="https://github.com/Shereefo/AWS-AIChatbot/assets/137960467/f5a52032-e6c9-478b-bc20-694d802c68b8">

Using this Lambda function to integrate with Lex: 

<img width="1392" alt="Screenshot 2023-10-09 at 8 49 18 PM" src="https://github.com/Shereefo/AWS-AIChatbot/assets/137960467/3166ed78-b9e4-45b4-8328-1370e2ad0dec">

Below is a video showing the process: 


https://github.com/Shereefo/AWS-AIChatbot/assets/137960467/81dc5d27-4b3b-477b-afb3-5607a0f2477b















