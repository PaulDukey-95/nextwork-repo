<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Build Your First AI Workflow

**Project Link:** [View Project](http://learn.nextwork.org/projects/ai-agent-nocode)

**Author:** Paul Ndukwe  
**Email:** paulnd95@gmail.com

---

## Building an AI Workflow

![Image](http://learn.nextwork.org/ecstatic_turquoise_serene_chicken/uploads/ai-agent-nocode_sdrjg8e123dv)

---

## Introducing Today's Project!

In this project, I will demonstrate how to build an AI workflow using n8n. This work flow will help me book new events in my calendar - simply send a text to your workflow, and it will schedule events in a Google Calendar. I am doing this project to learn how to automate tasks with AI.

### Tools and Techniques

Services I used were n8n, OpenAI and Google Calendar. Key concepts and skills I learnt include how to setup an AI workflow, workflows vs agents, credential, JSON expression and analysing logs from an AI agent to troubleshoot errors.

### Project reflection

This project took me approximately 60mins. The most challenging part was troubleshooting errors (using fixed vs expressions in system messaging). It was most rewarding to be able to book meetings using different prompts and seeing different tones for the AI assistant.

I chose to do this project today to learn task automation using AI, building workflows and AI agents. This project met my goals of setting up a working prototype at the end. I would also love a more indepth lesson on building chat agents to be integrated in websites, more understanding of OpenAI pricing models and how to integrate memory into the agent.

---

## Exploring n8n

I'm using n8n in this project to build my workflow. Workflows are sequences of actions that completes a task. You can involve AI in a workflow by using it to automate some of the decision making or actions. Example, using generative AI to draft email replies.

I signed up for a free trial for n8n, which includes 14days of access to n8n and the ability to create up to 5 workflows. I don't need to enter credit card details to start the trial, so I won't be charged.

![Image](http://learn.nextwork.org/ecstatic_turquoise_serene_chicken/uploads/ai-agent-nocode_c9d8f7a2)

---

## Starting an AI Workflow

To set up a workflow, I first configured a trigger, which means the action or scheduled time that kicks off the workflow. My trigger is a chat message. Other options include scheduled time or a manual trigger.

I connected my trigger with an AI agent node. AI Agents are autonomous systems that can decide on tasks and carry out actions WITHOUT triggers or predefined steps. But, in this project, I am building a workflow, which does require triggers/steps.

![Image](http://learn.nextwork.org/ecstatic_turquoise_serene_chicken/uploads/ai-agent-nocode_fmtkjyrg)

---

## Integrating ChatGPT

An AI workflow can be broken into three key components, which are chat model (brains of the agent), memory (storage for past interactions and data collection) and tools (software and code to help complete tasks).

My workflow's chat model uses OpenAI's services. Usually, connecting with OpenAI requires opening an OpenAI account and adding credits into you wallet. But, I could connect with OpenAI for free by claiming 100 API credits offered in n8n.

![Image](http://learn.nextwork.org/ecstatic_turquoise_serene_chicken/uploads/ai-agent-nocode_o5p6q7r8)

---

## Integrating Google Calendar

In this workflow, the tool is Google Calendar because I will need to connect my AI agent with the API (which is Google Calendar Tool in this instance) that actually creates new events in my calendar. You can think of the tool as the muscle that actually does the job.

To connect with Google Calendar, you have to allow n8n access to creating and editing events and entire calendars in your account. For security best practice, it's important to delete those credentials when you no longer need it for this workflow.

![Image](http://learn.nextwork.org/ecstatic_turquoise_serene_chicken/uploads/ai-agent-nocode_c9d8dfgv2)

---

## Testing My Workflow

I tested my AI workflow by asking it to book a meeting for tomorrow at 6am. The response was "I have booked the meeting for tomorrow at 6am. If you need to add any details or invitees to the meeting, please let me know!" which is an error because the actual event it set was at the wrong time.

To troubleshoot errors, you can investigate the AI Agents logs. I observed that the logs for the calendar tool showed no inputs, which means it was not getting any information around the proper start and end time for the event you want to book.

![Image](http://learn.nextwork.org/ecstatic_turquoise_serene_chicken/uploads/ai-agent-nocode_c9d8egrfdv)

---

## JSON Expressions

I decided to troubleshoot by reviewing my Calendar tool's set up where I noticed an error in the tool's start and end time configuration. By default, it takes in the current time as the start time and an hour after that as the end time. This ignores any information from the chat model.

I updated the start and end times in my calendar tool to JSON references for information my AI model will gather. Instead of the current time, the start time is not `{{ $fromAI('start_time') }}` which the AI model automatically fills.

![Image](http://learn.nextwork.org/ecstatic_turquoise_serene_chicken/uploads/ai-agent-nocode_897rg465e)

---

## System Messages

On my second test, my workflow successfully created an event and took in input from our AI chat model but it still made an error in scheduling the right date. This was because my chat model is passing the wrong date to my calendar tool.

This time, I tried to troubleshoot the error by writing a system message, which is a prompt that gets sent to my AI agent before the conversation starts. The prompt gives the agent instructions on what to do and information on the current date.

![Image](http://learn.nextwork.org/ecstatic_turquoise_serene_chicken/uploads/ai-agent-nocode_rfgdhn456)

---

## Success!

On my final test, a new event was scheduled at the right time because my system message was set as an expression instead of fixed. This means the AI agent will get accurate information around  todays date calculated automatically for every message.

![Image](http://learn.nextwork.org/ecstatic_turquoise_serene_chicken/uploads/ai-agent-nocode_w3x4y5z6)

---

## System Message Variation

For my project extension, I updated my AI agents system message. Changes I made include the tone of voice of my agent (from professional to enthusiastic).

When I tested my workflow again, I noticed that the AI agent responded with a lot of enthusiam, emojis and exclamation points. This is a completely different energy to the way it responded before.

You can also set up constraints with your system message! I did this by telling the AI agent that it can only book in events on even numbered days and prime minutes. As a result, my workflow needed to make sure requests met these constraints.

![Image](http://learn.nextwork.org/ecstatic_turquoise_serene_chicken/uploads/ai-agent-nocode_fewargtr52)

---

---
