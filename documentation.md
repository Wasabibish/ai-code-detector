
# AI-Detected Score Prediction Model

## Task Overview

This project aims to develop a machine learning model that predicts AI-detected scores based on the similarity between candidate answers and AI-generated responses. The dataset consists of coding questions paired with candidate answers, with two corresponding AI-generated responses from GPT-4, GPT-4 Turbo, and GPT-3.5 Turbo. Each entry includes an AI-detected score, ranging from 0 (no AI detected) to 1 (high AI detection).

---

## Methodology

## 1. Data Understanding

The dataset provided contains coding questions paired with candidate answers and responses from GPT-4, GPT-4 Turbo, and GPT-3.5 Turbo. 
Each example also includes an AI-detected score that ranges from 0 (no AI detected) to 1 (high AI detection). This score is calculated based on the similarity between the candidate's answer and the AI-generated responses.

### Dataset Structure
The dataset has the following fields:
- **coding_problem_id**: Unique identifier for the coding question.
- **llm_answer_id**: Unique identifier for the large language model (LLM) answer.
- **plagiarism_score**: A score representing the AI-detected similarity between the candidate's response and the LLM's output.
- **json_content**: Raw JSON data provided in the dataset.
- **llm_content**: The AI-generated answer from models like GPT-4, GPT-4 Turbo, or GPT-3.5 Turbo.
- **human_content**: The candidate's response to the coding question.
- **question**: The coding problem or question extracted from the JSON content.

## 2. Data Preparation

The first step I took was to inspect the data folder and explore its contents to gain an understanding of the structure. After examining a few examples, I prepared a dataframe with the following columns:
- **coding_problem_id**: Unique identifier for each coding problem.
- **llm_answer_id**: Unique identifier for each LLM answer.
- **plagiarism_score**: AI-detected score between 0 and 1 indicating the similarity of the candidate's answer to the LLM answer.
- **json_content**: Raw JSON content that was parsed from the dataset.
- **llm_content**: LLM-generated answers.
- **human_content**: Candidate's answers to coding questions.
- **question**: The coding question extracted from the JSON file.

This dataframe serves as the core structure for both model training and fine-tuning. I selected the relevant information necessary for this task, especially focusing on text-based data and the corresponding AI-detected plagiarism scores.

## Approach 1: Fine-tuning a Similarity Model

### Data Preparation

I prepared the dataset with the following structure:
- **llm code**: The code generated by an LLM (Large Language Model)
- **given code**: The actual candidate's code
- **score**: The AI-detected similarity score

This dataset will be used to fine-tune a similarity model for the task.

### Model Fine-tuning

I fine-tuned the model based on sentence similarity using the data prepared earlier. The training focused on improving the model's ability to accurately predict the similarity between the LLM-generated code and the candidate's code.

### Evaluation

The model was evaluated using the following metrics:
- **Pearson cosine**
- **Spearman cosine**
- **Pearson Manhattan**
- **Spearman Manhattan**
- **Pearson Euclidean**
- **Spearman Euclidean**
- **Pearson Dot**
- **Spearman Dot**
- **Mean Squared Error (MSE)**
- **R-squared (R²)**

These metrics indicate how well the model predicts similarity and overall regression performance.

### Inference Strategy

The inference process consists of the following steps:
1. **Vector Database Creation**: Create a vector database containing the embeddings of the test set, along with the metadata of their corresponding scores.
2. **Encoding**: For a given code and coding problem, the model encodes it to generate an embedding.
3. **Similarity Calculation**: Calculate the similarity (cosine similarity) between the given code and all vectors in the database.
4. **Retrieve Most Similar**: Identify the top **n** most similar embeddings.
5. **Calculate Weighted Score**: The final AI-detected score is computed by averaging the similarity-weighted real scores.

---

## Approach 2: Fine-tuning a Text Classification Model for Regression


### Data Preparation

In this approach, a text classification model was fine-tuned to handle a regression problem by configuring the model to have a single label for regression purposes. The data preparation involved the following steps:

1. **Text Formatting**: The dataset was formatted by combining the coding question and the candidate's answer into a single text entry. The AI-detected plagiarism score was used as the label for each entry. This allowed the model to learn the relationship between the combined text input and the corresponding score.

### Model Training

The text classification model was trained with the formatted data, treating the problem as a regression task. This involved adjusting the model to predict a continuous score based on the input text.

### Evaluation

The model's performance was assessed using the following metrics:

- **Mean Absolute Error (MAE)**: Measures the average magnitude of errors in predictions, without considering their direction.
- **Mean Squared Error (MSE)**: Measures the average squared difference between the predicted and actual values.

These metrics provide insights into the accuracy and effectiveness of the model's predictions.

### Inference Strategy

For inference, the model predicts the AI-detected score based on a given problem and code snippet by following these steps:

1. **Input Preparation**: The input text, which includes the coding question and the candidate's answer, is prepared for the model.
2. **Model Prediction**: The model processes the input text to generate predictions.
3. **Score Extraction**: The predicted score is extracted from the model’s output, representing the model’s estimation of the AI-detected similarity.

---

## Approach 3: Fine-tuning an LLM (Gemma-2B-IT) for Score Prediction

### Data Preparation

In this approach, an open-source LLM, Gemma-2B-IT, was fine-tuned to predict AI-detected scores. The preparation involved the following steps:

1. **Input and Output Formatting**: The dataset was structured by combining the coding question and the candidate's answer into a single input entry. The AI-detected plagiarism score was used as the output label. Additionally, an instruction was included to guide the model in generating appropriate predictions. The instruction specified that the model should provide a score between 0 and 1, where 0 means not AI-generated and 1 means fully AI-generated.

2. **Prompt Generation**: A prompt was created to guide the model's response. The prompt provided context for the task, asking the model to assess whether the code is AI-generated and to provide a score accordingly. 

### Training

The training of Gemma-2B-IT involved using LoRA (Low-Rank Adaptation) and BitsAndBytesConfig for quantization. This setup allowed for efficient training while managing memory and computational resources. The model was fine-tuned on the prepared dataset, focusing on learning to predict the AI-detected score based on the provided inputs and instructions.

### Inference

For inference, the following process was used:

1. **Prompt Construction**: A prompt was constructed to include the coding problem and the candidate's answer. This prompt was designed to query the model about the AI-detection score.

2. **Model Generation**: The model processed the prompt to generate a completion, which included the predicted AI-detection score.

3. **Output Post-Processing**: The generated output was processed to extract the numerical score. Regular expressions were used to identify and convert the predicted score from the text into a float value.

### Evaluation

Due to the high resource requirements for generating predictions, the evaluation focused primarily on monitoring the training loss rather than conducting extensive evaluations. The training loss provided an indication of how well the model was learning from the data.

---

## Model Choice

For this project, two models were chosen to handle different aspects of the problem:

1. **DistilBERT for Similarity and regression**:
   - **Model**: DistilBERT (`distilbert-base-uncased-finetuned-sst-2-english`)
   - **Reason for Choice**: DistilBERT is a smaller, distilled version of BERT, which makes it more efficient in terms of computational resources while retaining much of the performance of its larger counterpart. The model is pre-trained on English data, making it well-suited for understanding and evaluating text similarity in English. Given that the coding problems, DistilBERT's English-language capability and its efficiency due to being a distilled model make it an ideal choice for this task.

2. **Gemma-2B-IT for LLM-based Prediction**:
   - **Model**: Gemma-2B-IT
   - **Reason for Choice**: Gemma-2B-IT was selected for its suitability in fine-tuning for specific tasks like predicting AI-detection scores. As an open-source model, it offers flexibility and customization for our needs. Its ability to handle complex text generation and its comparatively smaller size than other models like LLaMA 3 make it an efficient choice for predicting detailed outputs like AI-detection scores. 



