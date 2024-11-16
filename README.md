# Model Conversion Workflow in Colab
This project demonstrates how to download a model from Hugging Face, convert it to GGUF format, and upload it back to Hugging Face using a Colab notebook.

## Table of Contents
1. Setup
2. Downloading the Model
3. Cloning llama.cpp Repository
4. Converting the Model
5. Uploading to Hugging Face Hub

## 1. Setup
- First, install the necessary libraries:

```bash
!pip install huggingface_hub
```

## 2. Downloading the Model
- Use the huggingface_hub library to download a model snapshot:

```bash
from huggingface_hub import snapshot_download

model_id = "Telugu-LLM-Labs/Indic-gemma-2b-finetuned-sft-Navarasa-2.0"
snapshot_download(repo_id=model_id, local_dir="Indic-gemma-2b-finetuned-sft-Navarasa",
                  local_dir_use_symlinks=False, revision="main")

!ls -lash Indic-gemma-2b-finetuned-sft-Navarasa
```

## 3. Cloning llama.cpp Repository
- Clone the llama.cpp repository and install its requirements to access conversion tools:

```bash
!git clone https://github.com/ggerganov/llama.cpp.git
!pip install -r llama.cpp/requirements.txt
```

## 4. Converting the Model
- Convert the downloaded Hugging Face model to GGUF format using the provided conversion script:

```bash
!python llama.cpp/convert-hf-to-gguf.py -h

!python llama.cpp/convert-hf-to-gguf.py Indic-gemma-2b-finetuned-sft-Navarasa \
  --outfile Indic-gemma-2b-finetuned-sft-Navarasa-2.0.gguf \
  --outtype q8_0

!ls -lash Indic-gemma-2b-finetuned-sft-Navarasa-2.0.gguf
```

### Explanation
- convert-hf-to-gguf.py -h: Displays the help message for the conversion script.
- convert-hf-to-gguf.py: Converts the specified Hugging Face model to GGUF format. Parameters:
 - --outfile: Specifies the output file name.
 - --outtype: Specifies the output type (e.g., q8_0).

## 5. Uploading to Hugging Face Hub
- Upload the converted model back to the Hugging Face Hub:
```bash
from huggingface_hub import HfApi
import os

# Set the token as an environment variable (recommended for security)
os.environ["HUGGING_FACE_HUB_TOKEN"] = "your_hugging_face_hub_token"

api = HfApi()
model_id = "your-username/Indic-gemma-2b-finetuned-sft-Navarasa-2.0.gguf"
api.create_repo(model_id, exist_ok=True, repo_type="model")  # Use the api object to create the repo
api.upload_file(
    path_or_fileobj="Indic-gemma-2b-finetuned-sft-Navarasa-2.0.gguf",
    path_in_repo="Indic-gemma-2b-finetuned-sft-Navarasa-2.0.gguf",
    repo_id=model_id,
)
```

## Conclusion
This notebook provides a step-by-step guide to convert a Hugging Face model to GGUF format and upload it back to the Hugging Face Hub. Modify the notebook as needed for your specific models and conversion requirements.

## License
This project is licensed under the MIT License. See the [LICENSE](LICENSE.txt) file for more details.

