# notes
- frontend sends the graph as json to backend
- don't create ui
    - just use swagger

# state
- current agent
- messages (chat history)
    - list of...
        - user msg
            - msg: str
            - intent of that msg: enum
        - agent msg
        - tool msg
- user info
    - address
    - phone number
    - email
    - name
    - user availability
- fix info
    - what to fix?
    - when to fix?
- buy info
    - room size
    - a/c type
    - budget
    - what to buy?
    - when to install?

# nodes
- global router agent
    - chat node
        - gets state (chat hist + current agent)
        - based on current agent...
            - if no agent: classify user intent and start a workflow
            - if there is agent, check chat hist
                - if user addresses why agent ended: go to that agent
                - if user talks about something else: classify user intent and start a workflow
- faq agent
- fix
    1. collect fix info agent
        - chat node
            - gets state: chat hist
            - extract fix info from chat hist
            - next
                - tool: if extracted fix info
                - end: if tool said bad fix info OR if need more fix info
                - next node: if all fix info is filled
        - tool node
            - available tools
                - verify given fix info
            - if fix info is good, update fix info in state
            - next: chat
    2. schedule fix agent
        - chat node
            - gets state
            - extract schedule info from chat hist
            - next
                - tool: 
        - tool node
            - available tools
                - get available time slots
                - check if given time slot is available
                - call schedule api with given time slot
            - 

# default workflow
1. classifier: classifies the intent of the user
    - model + reasoning effort
    - input
        - prompt: "classify"
        - chat history
    - processing: based on chat hist, decide the intent of lastest user msg
    - output
        - user msg
            - intent type
                - general questions
                - buy new ac
                - fix existing ac
    - next node
        - route based on intent type
2. general questions
    - model + reasoning effort
    - input
        - prompt: business details + db schema + faq
        - chat history
    - processing: 
    - output
        - agent msg
            - tool call
    - next node
        - tool OR end
3. buy new ac
    - todo
        - collect info: room size, address, etc.
        - suggest options: suggests different a/c and tradeoffs
        - process order: redirect to payment page
4. fix existing ac
    - todo
        - collect info: problem, address, etc.
        - schedule appointment: redirect to scheduling page
5. call tool
    - input
        - chat history (tool inputs)
    - output
        - tool msg
    - next node
        - node that called the tool?