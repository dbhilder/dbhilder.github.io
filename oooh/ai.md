---
layout: midnight-page
title:  "Generative AI @ Oooh"
description: |
    <i>Summary of how generative AI was integrated with the Oooh platform</i>
---
# Overview

When the Oooh platform pivoted to support a group chat application, generative AI had just come into the mainstream with OpenAI's ChatGPT,
and the technology seemed to be a natural fit for a chat-based application. Initial efforts at Oooh were focused on simple experiments with
prompt engineering, then later evolved into support for more robust, LLM-based applications. An extension framework was introduced to allow 
developers to quickly iterate on these LLM applications and integrate them with the platform. Work was also begun that made use of embeddings
generated from user generated content to drive recommendations and search. Other efforts were planned around fine-tuning models to improve
summarization and question and answer operations based on chat history, improve automated moderation of content based on feedback from human
moderators, and many other compelling features.

# Chat Bots

The first experiments with integrating generative AI into the Oooh platform took the form of chat bots. 

An ***@ai*** bot was created that would pass user messages posted to it to the OpenAI completion endpoint along with a prompt that had the 
local chat history injected into it for context. User's could converse with this chat bot similar to how one would with ChatGPT, but with 
the added context of the group chat in which the conversation was started.  

The next chat bot that was implemented was ***@aitrivia***, which passed a subject provided by a user into an LLM API with instructions to
generate the inputs for a Trivia oooh. The generated inputs were then used to configure and publish the oooh to the group chat where users 
could challenge their knowlege of the subject.

## Agents

The ***@ai*** chat bot was later extended to route user prompts to various chat bot agents that had been installed into the platform, similar 
to the tools functionality provided by OpenAI and Google Gemini. Work was underway to create agents that used RAG techniques to answer questions 
about the product and the company, though they were never completed. Additional agents were also planned that could allow the user to interact 
with the platform using natural language, such as publishing ooohs similar to ***@aitrivia***.

# Extension Framework

To support rapid development of more fully functional LLM-based applications (such as those built with LangChain) that could integrate with the
platform, an extension framework was developed. This framework would package up these LLM-based applications as serverless applications and 
install them into the platform. Clients could then trigger these extensions directly, or the extensions could respond to internal events in the
system, such as a message being posted or a user joining a group.  

This framework was used to create a chat summary extension that summarized recent chat history and also used the chat history to generate queries 
that could be passed to Bing News and Search to find articles relevant to the conversation in the group chat.

# UGC Embeddings

A platform service was added to generate embeddings from user generated content and store these in a vector database.
Clients could then use this service to perform similarity searches to find users and groups whose chat history matched search terms, or to
find groups and users that were similar to other groups and users.  

A variation of this feature was used to generate a "vibe match" amongst users to promote social connections.

# Model Fine-tuning

Most recently, efforts were underway to investigate opportunities that could result from fine-tuning models based on users' content.

***Efficient chat summarization and question and answering***  
A base LLM was fine-tuned using chat messages and other user generated content, allowing for chat histories to be summarized or questions
about chat histories to be answered without having to inject the chat history into a prompt.

***Improve moderation through human feedback***  
All user generated content published to the system was subject to moderation. The first step in this moderation process was to run inferences
on the content to generate classifications that could be used to determine its safety. A report was then put in a human monitored queue so the
content could be reviewed and released or rejected. The plan was to fine-tune the models with the decisions made by these human moderators, resulting
in safety scores that would reflect Oooh's values more and more over time.

***User-centric image generation***  
A feature was planned that would make use of the fine-tuning technique outlined by the DreamBooth paper to support generating custom images based
on a model trained on images users provided of themselves. This is similar to the Dreams feature provided by SnapChat.
