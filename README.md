# Amazon_Lex_V2_Walktrough_and_Navigation
This walkthrough will show you how to create a bot using V2 of Amazon Lex. It will also show you how to navigate the Amazon Lex V2 console once the bot is created.
This will be a walk through for building an Amazon Lex powered Chat bot using AWS management console. This walk through uses V2 of the Amazon Lex. I am writing this document with my personal experience using V2 of Amazon Lex for an Intern project after encountering differences in V1 and V2 that resulted in confusion. There already exists a Amazon V1 Walk though at the following link: https://github.com/aws-samples/amazon-ai-building-better-bots

## Table of contents:

    Navigation
        Creating Chat Bot for the first time
        Resuming Chat Bot / After creating Chat Bot
    Creating your new Chat Bot
        Important constructs
        Creating a Chat Bot
        Configuring Intent
        Configuring Slot Types
        Add Slots to the intent
        Set the Confirmation Prompt
        Set Closing response
        Optional - Set Code Hooks (Using Lambda function)
        Configure Fallback Intent
        Build and Test

# Navigation

This section shows how to navigate the V2 of Amazon Lex

## 1) Creating Chat Bot for the first time

When creating the chat bot for the first time, Go into AWS management console and search for Amazon Lex in the search bar. Once you click on Amazon Lex, You should see a window like this:

;;;;;;;;;

To start creating a chat bot we click on the Create Bot button. Jump to creating a new chat bot section of this page to start creating your own chat bot.

## 2) Resuming Chat Bot / After Creating Chat Bot

It can be difficult to navigate the console to resume the chat bot in V2 of Amazon Lex.

#### 1) You should see your chat bot existing in the "bots" Page of your Amazon Lex Console:

hope page with chat bot

#### 2) If you don't see the chat bot you created, try changing regions on your AWS management console on the top right corner under the regions drop down window.

Screen Shot 2021-09-09 at 11.12.53 AM.png

#### 3) Click on the chat bot you created which will be shown on the 'bots' page and you should see a window like this:

Screen Shot 2021-09-09 at 11.24.39 AM.png

#### 4) Click on Bot Versions:

Screen Shot 2021-09-09 at 11.24.09 AM.png

#### 5) You should see the versions of your Chat bot appear on the screen:

Screen Shot 2021-09-09 at 11.27.44 AM.png

When you have created the bot for the first time you will only see a Draft version which created by default when you create the bot. If you want to add more versions to the chat bot you can do so by clicking on create versions.

#### 6) Click on Draft version and a window will open up when you can start editing the chat bot.

Screen Shot 2021-09-09 at 11.31.48 AM.png

#### 7) You should see the languages you configured for the chat bot

We can edit the chat bot with Intents and Slot types which you can see in the left side of the screen. Each language will have separate intents and slot types as shown in the picture above.

#### 8) Now, you can configure the intents and slot types to create your own chat bot.

# Creating your new Chat bot

## Let's create a Coffee Chat bot

For creating chat bot in V1 of Amazon Lex, please visit https://github.com/aws-samples/amazon-ai-building-better-bots

## 1) Important constructs:

Before we start making a Chat Bot, lets understand what Intents, Slots, Utteranaces, and Fulfillment are. Each of these constructs serve a different purpose in allowing Lex to understand how to interact with a user. This diagram helps understand each of the 4 constructs.

Diagrams_lex_bookhotel.26784edb7206413e2046d5a00b411589d56a6621.png

## 2) Creating a Chat Bot

Click on the Create Bot button on the home page of Amazon Lex console. you should see a window like this:

Screen Shot 2021-09-09 at 11.48.37 AM.png

The first option is to select a creation method: You can select Create to create your own custom chat bot or you can select Start with and example to launch a preconfigured chat bot and make edits to edit. we will select Create for our use case.

Fill out the configuration column as follows:
|Setting	             |  Value                                             |
| -------------------- | -------------------------------------------------- |
|Bot name	             |  CoffeeBot                                         |
|Description	         |  Chat bot to order coffee                          |
|IAM permissions       |  Create a role with basic Amazon Lex permissions.  | 
|COPPA	               |  No                                                |
|Idle session timeout	 |  5 min                                             |

Click next and proceed to the add languages section
| Settings | Value |
| --------------------------------------------------------------------  |
|Select Language |	English  |
|Voice interaction	|  Choose any that you like  |
|Intent classification confidence score threshold  | 	Default (0.40)  |

3) Configuring Intent

After configuring bot settings click on create bot and an intent screen will open up which looks like this:

Screen Shot 2021-09-09 at 1.58.29 PM.png
| Settings | Value |
| --------------------------------------------------------------------  |
| Intent Name |	cafeOrderBeverageIntent |
| Description	| This intent is to recognize a drink order at the coffee shop. |
|Sample utterances	| I would like a {BeverageSize} {BeverageType} \n
Can I get a {BeverageType}\n
May I have a {BeverageSize} {Creamer} {BeverageType}\n
Can I get a {BeverageSize} {BeverageTemp} {Creamer} {BeverageType}\n
Let me get a {BeverageSize} {Creamer} {BeverageType} |

Click on Save Intent . This is important!! don't forget to save.

Under left-hand menu click Back to Intents list . This brings us back to the main window for our chat bot.

4) Configuring Slot Types

Screen Shot 2021-09-09 at 2.12.16 PM.png

You will see two intents. One that you created just now and a FallbackIntent. A FallbackIntent is for a scenario when user types in something that does not match any Intent in our chat bot (A case where the chat bot does not understand what the user is talking about).

In the left-hand menu you can see Intents and Slot types. We will now configure the slot types for our cafeOrderBeverageIntent.

Click on Slot types .

Screen Shot 2021-09-09 at 2.16.54 PM.png

Click on Add slot type and then Click on Add blank slot type
Slot type name	cafeBeverageType
Description	This slot helps the intent detect the type of drink that is being ordered by the user.
Slot value resolution	Default (Expand values)
Values (add each individually and click on add value)	coffee
cappuccino
latte
mocha
chai
espresso
smoothie

Click on Save Slot type . on the left hand menu  Click on Slot types to go back to the main menu.

Similarly create 3 more slot types.
Slot type name	Description	Slot resolution	Values (each entry on a separate line)
cafeBeverageSize	 	default	kids
small
medium
large
extra large
six ounce
eight ounce
twelve ounce
sixteen ounce
twenty ounce
cafeCreamerType	 	default	

two percent
skim milk
soy
almond
whole
skim
half and half
cafeBeverageTemp	 	default	

kids
hot
ice

5) Add Slots to the intent

Go back to the Intents list and click on the cafeOrderBeverageIntent

under Slots section add the following slots:-

Note: The "required" field is a check box
Required	Name	Slot type	Prompt
Yes	BeverageType	cafeBeverageType	What kind of beverage would you like? For example, mocha, chai, etc.
Yes	BeverageSize	cafeBeverageSize	What size? small, medium, large?
 	Creamer	cafeCreamerType	What kind of milk or creamer?
 	BeverageTemp	cafeBeverageTemp	Would you like that iced or hot?
6) Set the Confirmation Prompt
Confirmation type	Description
Confirm	You'd like me to order a {BeverageSize} {BeverageType}. Is that right?
Cancel	Okay. Nothing to order this time. See you next time!

7) Set Closing response

Thank you. Your {BeverageType} has been ordered.

8) Optional - Set Code Hooks (Using Lambda function)

###########################################################

You have two options:

1) Use Lambda function for Initialization and validation

      -- Activating this would mean that at every step of the conversation, a Lambda function that you define will be invoked and executed. This option helps to initialize values and validate user input at

          every step of the conversation using a Lambda function.

2) Use a Lambda function for fulfillment

      -- Activating this would mean that the Lambda function will be invoked to fulfill this intent. When the user interacts with the chat bot and completes all the slots and even completes the confirmation

          prompt, this Lambda function will be called and the user input will be passed to the Lambda function.

For this walkthrough, we will not use either of these options. If you want to implement it, "Save Intent" and only read the sections "Create a Lambda function" and "Create a test event to validate the function" at https://github.com/aws-samples/amazon-ai-building-better-bots . Don't read the next step from that document as they are for V1 of Lex, follow the next steps here:

Once you have created the Lambda function, you would update the bot to use the Lambda function. To do this, on the bottom-right, there is a Build and a Test button.

Click Build first and once build is complete click Test . The following window will open up.

Screen Shot 2021-09-09 at 2.57.49 PM.png

Click on Setting Icon (Cogwheel).  The following window will appear:

Screen Shot 2021-09-09 at 2.59.17 PM.png

Under the Lambda function section -> under Source, add the Lambda function that you created.

###########################################################

9) Configure Fallback Intent

You might remember that Fallback Intent is invoked when the chat bot does not understand what the user means.

Click into Fallback Intent and fill in the following information.
Name	FallbackIntent
Description	Default intent when no other intent matches
Closing Response	I could not understand your order. Please see the cashier to order a drink.

10) Build and Test

On the bottom right section of the window, you will see a Build  and a Test button. First, click the Build button, then click the Test button in that order. A test window will open up like this:

Screen Shot 2021-09-09 at 2.57.49 PM.png

Type a message for example,

Can I get a coffee?

You will notice the chat bot respond.

Screen Shot 2021-09-09 at 3.20.24 PM.png

 Let's test if the fall back intent works. Type a message that would not match an intent, for example, The birds were flying high in the sky and you should get the response from the Fallback Intent

Screen Shot 2021-09-09 at 3.26.28 PM.png

Our Chat bot is complete and now you can build your own chat bots and customize them as you want with Amazon Lex V2.
