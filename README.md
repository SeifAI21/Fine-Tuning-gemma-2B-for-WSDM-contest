# Fine-Tuning-gemma-2B-for-WSDM-contest
Problem Description

Large Language Models (LLMs) are rapidly integrating into various aspects of our lives, yet ensuring their responses align with user preferences remains a critical challenge. This competition addresses this gap, leveraging real-world data to enhance LLMs' ability to resonate with human preferences.

Competition Details

Participants were tasked to predict which response a user would prefer in head-to-head LLM comparisons. The dataset was derived from Chatbot Arena, where users interacted with two anonymous LLMs and selected their preferred response. The goal was to bridge the gap between LLM capabilities and human preferences by creating a predictive model that captures user inclinations.

This challenge aligns with the concept of "reward models" or "preference models" in Reinforcement Learning from Human Feedback (RLHF). Directly prompting LLMs for preference predictions has limitations, including:

Position Bias: Favoring responses presented first.

Verbosity Bias: Preferring longer responses.

Self-Enhancement Bias: Favoring self-promoting responses.

Participants were encouraged to explore diverse machine-learning techniques to predict user preferences effectively, contributing to the development of more user-friendly AI conversation systems.

#Challenges Faced and Solutions

1. Inference Notebook Without Internet Access

The inference environment was restricted from internet access, leading to initial inference results being limited to the baseline score. Upon investigation, I hypothesized that using Hugging Face during training and running inference on Kaggle might cause discrepancies in the model's performance. This was due to potential differences in the environments, leading to checkpoint incompatibility. Adjusting the environment to align the training and inference settings resolved the issue.

2. Inference Time Constraint (4.7 Hours)

The competition imposed a maximum inference time of 4.7 hours. Initially, the inference notebook exceeded this limit. Monitoring revealed irregular GPU usage: the GPU worked efficiently for short intervals but frequently dropped, leading to extended CPU usage and delays.

Solution:
The bottleneck was identified as the data loader within the training loop. By removing the data loader from the loop, the inference process became GPU-dominant, reducing the total runtime to 2.5 hours, well within the limit.

3. Tokenization Challenges

The dataset consisted of the following structure:

Prompt

Response A

Response B

Winner

Initially, the maximum token length was set to 1024. However, exploratory data analysis (EDA) revealed that some prompts were large, leading to truncation of responses A and B, or both. This truncation negatively affected model accuracy.

Solution:
To ensure all components (prompt, response A, and response B) were represented, tokenization was adjusted as follows:

Assign 1024/3 tokens to each component.

This guaranteed that all three were fully tokenized in every data instance.

This adjustment significantly improved the model's accuracy.
