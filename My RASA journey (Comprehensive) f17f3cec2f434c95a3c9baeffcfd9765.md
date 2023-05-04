# My RASA journey (Comprehensive)

Documenting my journey with RASA

Notes:

[https://rasa.com/docs/rasa/](https://rasa.com/docs/rasa/)

1. Setting up virtual environment
    
    (more about venv: [https://docs.python.org/3/library/venv.html](https://docs.python.org/3/library/venv.html))
    
    Basic guide: [https://magbanum.com/blog/a-virtual-environment-in-powershell](https://magbanum.com/blog/a-virtual-environment-in-powershell) 
    
    ```java
    C:\Users\saian> python -m venv ./venv
    ```
    
2. Activating the virtual environment
    
    ```java
    C:\Users\saian> .\venv\Scripts\activate
    
    .\venv\Scripts\activate : File C:\Users\saian\venv\Scripts\Activate.ps1 cannot be loaded because running scripts is
    disabled on this system. For more information, see about_Execution_Policies at
    https:/go.microsoft.com/fwlink/?LinkID=135170.
    At line:1 char:1
    + .\venv\Scripts\activate
    + ~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : SecurityError: (:) [], PSSecurityException
        + FullyQualifiedErrorId : UnauthorizedAccess
    ```
    
    Solving this error: [https://tecadmin.net/powershell-running-scripts-is-disabled-system/](https://tecadmin.net/powershell-running-scripts-is-disabled-system/)
    
    i.e.,
    
    ```java
    // Run powershell as administrator.
    // Get-ExecutionPolicy
    // Set-ExecutionPolicy RemoteSigned 
    ```
    
    ![Untitled](My%20RASA%20journey%20(Comprehensive)%20f17f3cec2f434c95a3c9baeffcfd9765/Untitled.png)
    
    “Y” and continue.
    
    ```java
    PS C:\Users\saian> .\venv\Scripts\activate
    (venv) PS C:\Users\saian>
    ```
    
    2.1. Re-activating the virtual environment:
    
    - [https://youtu.be/ebeebaumL3M](https://youtu.be/ebeebaumL3M) → Video guide
    
    ![Untitled](My%20RASA%20journey%20(Comprehensive)%20f17f3cec2f434c95a3c9baeffcfd9765/Untitled%201.png)
    
3. Installing RASA OpenSource
    
    ```java
    pip3 install rasa
    
    // to make sure rasa is up to date:
    pip3 install --upgrade rasa
    ```
    
    It will take a few minutes to download and install all packages. (Longer than usual…)
    
4. Building the assistant
    
    ```java
    rasa init
    ```
    
    Creates a new project with example training data, actions, and config files.
    
    Project created in the current directory.
    
    ![Untitled](My%20RASA%20journey%20(Comprehensive)%20f17f3cec2f434c95a3c9baeffcfd9765/Untitled%202.png)
    
5. Training (initial) the model using rasa demo files:
    
    ![Untitled](My%20RASA%20journey%20(Comprehensive)%20f17f3cec2f434c95a3c9baeffcfd9765/Untitled%203.png)
    
6. Minimum files reqd. to make RASA run (Overview):
    1. **domain.yml → All of the things your assistant knows about;**
        
        (Directory of everything your assistant “knows”.)
        
        - Responses: Things that assistant says to the users.
            
            ![Untitled](My%20RASA%20journey%20(Comprehensive)%20f17f3cec2f434c95a3c9baeffcfd9765/Untitled%204.png)
            
            The above 5 things are the only things the assistant says (assuming no natural language generation -nlg is used)
            
            - multiple texts for a specific utterance → any one response will be randomly selected.
                
                ![Untitled](My%20RASA%20journey%20(Comprehensive)%20f17f3cec2f434c95a3c9baeffcfd9765/Untitled%205.png)
                
            - Can use **slots** in curly brackets too, like in the above image.
                - Slots are assistant’s memory:
                    
                    ![Untitled](My%20RASA%20journey%20(Comprehensive)%20f17f3cec2f434c95a3c9baeffcfd9765/Untitled%206.png)
                    
                    ![Untitled](My%20RASA%20journey%20(Comprehensive)%20f17f3cec2f434c95a3c9baeffcfd9765/Untitled%207.png)
                    
                - Settings slots using NLU:
                    
                    ![Untitled](My%20RASA%20journey%20(Comprehensive)%20f17f3cec2f434c95a3c9baeffcfd9765/Untitled%208.png)
                    
                - Settings slots using custom actions.
                - Slots can be configured to influence the conversation. (**influence_conversation: true**)
                    - influence_conversation = true configuration defines that the slot
                    will influence how the dialogue management model makes
                    the prediction for the next action. Depending on the type of the slot,
                    the flow can be influenced by the value of the slot or whether
                    the value of this slot is present.
                        
                        ![Untitled](My%20RASA%20journey%20(Comprehensive)%20f17f3cec2f434c95a3c9baeffcfd9765/Untitled%209.png)
                        
                    - How and when this should happen, depends on the type of slot.
                    
                    ![Untitled](My%20RASA%20journey%20(Comprehensive)%20f17f3cec2f434c95a3c9baeffcfd9765/Untitled%2010.png)
                    
            - Can also add Buttons and Images:
                
                ![Untitled](My%20RASA%20journey%20(Comprehensive)%20f17f3cec2f434c95a3c9baeffcfd9765/Untitled%2011.png)
                
            - Channel specific responses are possible
            - Custom output payloads:
                
                ![Untitled](My%20RASA%20journey%20(Comprehensive)%20f17f3cec2f434c95a3c9baeffcfd9765/Untitled%2012.png)
                
        
        - Intents: Categories of things that users say to the assistant that might warrant a response.
            - Starting, keep as many less intents as possible.
            - Training data provided at a separate place for ways that users might express that intent, nlu will take in that training data and map it to one of these intents.
            - These are classes for multi-class classification.
            
            ![Untitled](My%20RASA%20journey%20(Comprehensive)%20f17f3cec2f434c95a3c9baeffcfd9765/Untitled%2013.png)
            
        - Slots: Variables that are stored over the course of the conversation.
        - Entities: Pieces of information extracted form the raw user input text. (Eg. numbers, dates, country names, etc.)
            - “Book a flight to Sydney” → Entity: Destination; Value: Sydney;
            - 
        - Forms & Actions: To extend what the assistant can do and add some logic.
    2. config.yml → sets out the nlu pipeline (default one is already set up by RASA, no worries for now)
    3. At least one data file:
        1. **nlu.yml** → different examples of ways that people can express a specific intent.
            1. intents: Training data includes examples of ways to say things.
            2. If you have data, perform modified content analysis (Qualitative analysis)
            3. [https://youtu.be/8gvGh6H2NO0?list=PL75e0qA87dlEjGAc9j9v3a5h1mxI2Z9fi&t=812](https://youtu.be/8gvGh6H2NO0?list=PL75e0qA87dlEjGAc9j9v3a5h1mxI2Z9fi&t=812) → WHY FEWER INTENTS.
                
                ![Untitled](My%20RASA%20journey%20(Comprehensive)%20f17f3cec2f434c95a3c9baeffcfd9765/Untitled%2014.png)
                
            4. Training data for entity extraction should be included in this nlu.yml file.
                - **My account number is [Entity value](entity label):**
                
                ![Untitled](My%20RASA%20journey%20(Comprehensive)%20f17f3cec2f434c95a3c9baeffcfd9765/Untitled%2015.png)
                
            5. Entity Extraction methods:
                - Using pre-built models: (Components: DucklingHttpExtractor, SpacyEntityExtractor)
                    - Duckling - Extract numbers, dates, email addresses, url, etc.
                    - SpaCy - names, product names, locations, etc.
                - Using Regex: (for entities that match a specific pattern) eg: phone numbers, pin codes.
                    
                    ![Untitled](My%20RASA%20journey%20(Comprehensive)%20f17f3cec2f434c95a3c9baeffcfd9765/Untitled%2016.png)
                    
                - Machine Learning models
                    - For extracting custom entities
                    - Training data reqd.
                    - DIETclassifier in RASA
                
                Output: JSON format - Entity label, entity value, confidence values, component used for entity extraction.
                
                ![Untitled](My%20RASA%20journey%20(Comprehensive)%20f17f3cec2f434c95a3c9baeffcfd9765/Untitled%2017.png)
                
            - Synonyms - Mapping extracted value to another different one. (Eg: Credit card Account and Credit Account → Credit)
                - Method #1
                
                ![Untitled](My%20RASA%20journey%20(Comprehensive)%20f17f3cec2f434c95a3c9baeffcfd9765/Untitled%2018.png)
                
                - Method #2
                
                ![Untitled](My%20RASA%20journey%20(Comprehensive)%20f17f3cec2f434c95a3c9baeffcfd9765/Untitled%2019.png)
                
            - Lookup tables: list of words used to generate case-sensitive regular expression patterns:
                
                ![Untitled](My%20RASA%20journey%20(Comprehensive)%20f17f3cec2f434c95a3c9baeffcfd9765/Untitled%2020.png)
                
            - Entity roles and groups allow you to add more value to your entities.
                - “Book a flight from New York to Boston”
                    
                    ![Untitled](My%20RASA%20journey%20(Comprehensive)%20f17f3cec2f434c95a3c9baeffcfd9765/Untitled%2021.png)
                    
                    ![Untitled](My%20RASA%20journey%20(Comprehensive)%20f17f3cec2f434c95a3c9baeffcfd9765/Untitled%2022.png)
                    
            - Entity groups - allow you to put extracted entities under a specific group.
                
                ![Untitled](My%20RASA%20journey%20(Comprehensive)%20f17f3cec2f434c95a3c9baeffcfd9765/Untitled%2023.png)
                
                Need different examples for the assistant to learn.
                
        2. **rules.yml** → defines rules; conditional stuff; 
            1. Short pieces of conversation that always go the same way.
            2. Do use rules for single turn (at max 2 turn conversations) AVOID multiturn conversations using rules. 
        3. **stories.yml** → Potential conversation flows (**longer conversations, for machine learning to learn from)**; intents: categories of things said by users and detected by the nlu; actions: things the assistant does : “utter” tied with domain files; custom utterances can be generated using custom actions
            1. Training data to teach your assistant what it should do next.
            2. The `MemoizationPolicy` remembers the stories from your
            training data. It checks if the current conversation matches the stories in your
            `stories.yml` file. If so, it will predict the next action from the matching
            stories of your training data.
                1. If a new conversation is suspected, TED policy will guess what is the most likely thing to be said next. If it’s not sure enough (confidence value < threshold → performs fallback. 
                    
                    (The Transformer Embedding Dialogue (TED) Policy is a multi-task architecture for next action prediction and entity recognition.)
                    
                
                Interactive learning → Happy paths, common flows
                
            3. OR statements: “afffirm” or “thanks” intents will trigger “action_signup_newsletter” action, check below.
                
                ![Untitled](My%20RASA%20journey%20(Comprehensive)%20f17f3cec2f434c95a3c9baeffcfd9765/Untitled%2024.png)
                
            4. CHECKPOINTS! We can use checkpoints and form another story from there (as given in the following second pic).
                
                ![Untitled](My%20RASA%20journey%20(Comprehensive)%20f17f3cec2f434c95a3c9baeffcfd9765/Untitled%2025.png)
                
                ![Untitled](My%20RASA%20journey%20(Comprehensive)%20f17f3cec2f434c95a3c9baeffcfd9765/Untitled%2026.png)
                
7. Rasa commands:

```java
rasa -h //help, what all can the assistant do
rasa train //trains a new model and stores in the marked directory
rasa shell //talk to assistant in the shell 
rasa shell --debug //use this flag to extract more information

```

1. Training data and rules:
    - “data”?
        - text data to pre-train any models or features you’re using  in the nlu pipeline; (eg; language models, word embeddings)
        - User-generated text (+ mapping onto a single intent category)
        - Patterns of conversation (general flow)
        
        “USER CONVERSATIONS WITH THE ASSISTANT!!” 
        
    - DOs: ✅
        - Use actual stories as usual conversations
        - Use small stories that aren’t full conversations
        - Use rules only for single turn conversations
    - DON’Ts ❌
        - Rules for multi-turn conversations.
        - Overuse OR statements, checkpointing.
        - Write out all possible conversation flows.
        - Delay user testing.
        
2. Rollling back to RASA 2.X for better compatibility (Am I?)
    
    ```java
    pip install rasa == 2.8.1
    ```
    
    OOPS! ERROR!
    
    ![Untitled](My%20RASA%20journey%20(Comprehensive)%20f17f3cec2f434c95a3c9baeffcfd9765/Untitled%2027.png)