# AI Detection in Code

## Description

This project aims to detect the usage of AI in coding responses by predicting an AI-detected score. The model compares candidate answers with responses generated by various AI models (GPT-4, GPT-4 Turbo, and GPT-3.5 Turbo) to determine the likelihood that a response was AI-generated.

## Requirements

To set up and run this project, follow these steps:

1. **Python**: Ensure you have Python 3.9.10 installed (or a compatible version).

2. **Create a Virtual Environment** (recommended):
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows use `venv\Scripts\activate`

3. **Install Dependencies:**
    ```bash
    pip install -r requirements.txt

## Notebooks

The following Jupyter Notebooks are included for reference and are optional to run since the training has been completed and models have been pushed to the hub:

- **data_preparation.ipynb**: Prepares the dataset for model training.
- **finetune_LLM.ipynb**: Fine-tunes the language model.
- **finetune_similarity.ipynb**: Fine-tunes the similarity model.
- **finetune_regression_model.ipynb**: Fine-tunes the regression model.

## Testing the Model

To test the fine-tuned model, use the provided notebook:

- **deployment.ipynb**: Tests the fine-tuned models to validate the performance.

## Files

- **data.csv**: The dataset used for training the models.
- **vector_db.pt**: A mini vector database for similarity model inference.

## Documentation

For a detailed explanation of the process, methodologies, and choices made, please refer to the [documentation.md](documentation.md) file.


## Contact

- Email: ikram.djeghali@gmail.com
- HuggingFace : https://huggingface.co/wasabibish
