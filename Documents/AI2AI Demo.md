### `Agent` Class:

- **Attributes:**
    
    - `conversation_history: list` - Stores the history of the conversation as a list of strings.
    - `name: str` - The name of the agent.
    - `resume: dict` - The resume information for the agent, passed as a dictionary.
    - `knowledge: dict` - Extracted knowledge from the resume, also stored as a dictionary.
- **Methods:**
    
    - `__init__(name: str, resume: dict)` - The constructor method initializes the attributes and extracts knowledge from the resume.
    - `get_name() -> str` - Returns the name of the agent.
    - `load_resume_ocr(resume: dict) -> dict` - Extracts knowledge from the resume. 
    - extract_knowledge
    - This method is supposed to use an OCR service for processing but currently returns the resume as is.
    - `generate_response(prompt: str) -> str` - Generates a response based on the given prompt by interacting with the GPT-4o-mini model.
    - `update_knowledge(new_info: dict)` - Updates the agent's knowledge with new information extracted during the conversation.

### `Conversation` Class:

- **Attributes:**
    
    - `agent1: Agent` - The first agent involved in the conversation.
    - `agent2: Agent` - The second agent involved in the conversation.
    - `topics: Topics` - The topics that guide the conversation.
- **Methods:**
    
    - `__init__(agent1: Agent, agent2: Agent, topics: Topics)` - Initializes the conversation with two agents and a set of topics.
    - `start_conversation(iterations: int) -> list` - Runs the conversation between the two agents for a specified number of iterations and returns the conversation history.
    - `log_conversation(conversation_history: list)` - Logs the conversation history to a file.

### `Topics` Class:

- **Attributes:**
    
    - `topics_list: list` - A list of topics derived from the agents' resumes.
    - `agents: list[Agent]` - A list of agents for which topics are being extracted.
- **Methods:**
    
    - `__init__(agents: list[Agent])` - Initializes the topics class with a list of agents.
    - `extract_topics() -> list` - Extracts relevant topics from the agents' resumes based on their knowledge and experiences.
    - `get_next_topic() -> str` - Retrieves the next topic for discussion based on the conversation's flow.

### `Run` Class:

- **Attributes:**
    - `conversation: Conversation` - The conversation object that manages the interaction between agents.
- **Methods:**
    - `__init__(conversation: Conversation)` - Initializes the run with a conversation.
    - `execute()` - Executes the conversation, possibly with pre-processing and post-processing steps.
    - `analyze_results()` - Analyzes the outcomes of the conversation, potentially updating agents' knowledge or extracting insights.




### `Topics` Class:

- **Attributes:**
    
    - `agent1_knowledge: dict` - The knowledge and experience information for agent 1.
    - `agent2_knowledge: dict` - The knowledge and experience information for agent 2.
- **Methods:**
    
    - `__init__(agent1_knowledge: dict, agent2_knowledge: dict)` - The constructor method initializes the `Topics` class with the knowledge and experience of two agents.
        
    - `find_commonalities() -> list[str]` - Identifies commonalities between the two agents' knowledge and experience. Returns a list of these commonalities.
        
    - `extract_topics_from_commonalities(commonalities: list[str]) -> list[str]` - Uses OpenAI to extract and refine conversation topics from the identified commonalities. Returns a list of refined conversation topics.


\

- Commonalities List.example:
1. Skills:
    - Proficiency in Python and data analysis.
    - Experience with machine learning frameworks like TensorFlow.
    - Cloud platform expertise (AWS, Google Cloud).
2. Experience:
    - background in AI and ML projects.
    - Leadership in cross-functional tech teams.
    - Mentoring and training experience in tech roles.
3. Education:
    - Degrees in computer science or engineering.
    - Focus on AI and machine learning during academic careers.
    - Participation in tech competitions or hackathons.
4. Projects:
    - **Infrastructure Modernization**: Each has spearheaded large-scale infrastructure projects for financial institutions, focusing on migrating legacy systems to cloud environments. These efforts resulted in enhanced system reliability, reduced costs, and more efficient operations.
    - **Healthcare Systems Integration**: Both have developed platforms to integrate complex healthcare data across different facilities, ensuring compliance with industry regulations and enhancing the sharing of medical records, which improved patient care.
5. Certifications:
    - AI and data science certifications from recognized institutions.
    - Awards or honors in tech innovation.
