# M.A.R.S. (Model Assisted Review System) - Web User Interface

![React](https://img.shields.io/badge/react-%2320232a.svg?style=for-the-badge&logo=react&logoColor=%2361DAFB)
![Flask](https://img.shields.io/badge/flask-%23000.svg?style=for-the-badge&logo=flask&logoColor=white)
![OpenAI](https://img.shields.io/badge/OpenAI-%23412991.svg?style=for-the-badge&logo=openai&logoColor=white)

**M.A.R.S.** is a full-stack web application designed to streamline the evaluation of biomedical research papers using Artificial Intelligence. It automates the screening phase of Systematic Literature Reviews (SLRs) by extracting text, analyzing parameters, and classifying papers as "Good" (Relevant) or "Bad" (Irrelevant) based on a specific topic.

This repository contains the **Frontend (React)** and **Backend (Flask)** code for the interface.

## Features

* **Topic Selection:** Autocomplete functionality for selecting biomedical topics or entering custom queries.
* **Bulk Upload:** Drag-and-drop support for uploading up to 1000 PDF files at once.
* **AI-Powered Extraction:** Utilizes **GPT-4o-mini** to extract key parameters (Title, Abstract, Study Type, Population Size, Reference Count) from raw PDF text.
* **Local LLM Inference:** Connects to a fine-tuned **Llama 3.2 3B** model running locally to classify the papers based on the extracted parameters.
* **Results Visualization:** Interactive Pie charts and detailed lists showing the classification results.
* **Automated Organization:** Automatically sorts files and generates a downloadable ZIP archive containing separated folders for 'Good' and 'Bad' papers.

## Architecture

The system follows a modern client-server architecture:

1.  **Frontend:** Built with **React** and **TypeScript**. It manages file selection, displays processing states, and visualizes results.
2.  **Backend:** Built with **Python Flask**.
    * **Text Extraction:** Uses `pdfplumber` to parse text from uploaded PDFs.
    * **Parameter Extraction:** Sends truncated text to OpenAI's `GPT-4o-mini` API to structure data.
    * **Classification:** Uses `unsloth` and `FastLanguageModel` to run inference on a local Llama 3.2 model.
    * **File Handling:** Manages temporary storage and ZIP creation for downloads.

## Tech Stack

* **Frontend:** React.js, TypeScript, Vite, React Router, Chart.js, React Dropzone.
* **Backend:** Flask, Flask-CORS, Python.
* **AI/ML:**
    * **Unsloth:** For efficient loading and inference of the fine-tuned Llama model.
    * **PyTorch:** Underlying framework for the LLM.
    * **OpenAI API:** For parameter extraction.

## Prerequisites

* **Node.js** (v16+ recommended)
* **Python** (v3.10+ recommended)
* **GPU:** NVIDIA GPU required for `unsloth` and local Llama inference (CUDA support).
* **OpenAI API Key:** Required for the parameter extraction step.
* **OS:** Linux required for CUDA support.

## Installation

### 1. Clone the Repository
```bash
git clone https://github.com/ashwin-v1/MARSWebUI.git
cd MARSWebUI
```

### 2. Backend Setup
Navigate to the backend directory and set up the Python environment.

```bash
cd backend
# Create a virtual environment
python -m venv venv
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt
# Note: You may need to update unsloth installation depending on your CUDA version.
```

**Configuration:**
1.  Create a file named `key.env` in the root of the `backend` folder.
2.  Add your OpenAI API key:
```text
OPENAI_API_KEY=my-precious-key-here
HUGGINGFACE_API_KEY=key-here-if-pulling
```

**Model Setup:**
The application expects a locally saved model or a HuggingFace model path. By default, `app.py` looks for:
`model_name = "llama3.2_3B_fullParamDataset_3epoch"`

Ensure you have downloaded the fine-tuned model or updated the `model_name` in `app.py` to point to the correct path/HuggingFace repo.

### 3. Frontend Setup
Navigate to the frontend directory and install dependencies.

```bash
cd ../frontend
npm install
```

## Usage

### Start the Backend Server
In the `backend` terminal (with virtual environment active):
```bash
python app.py
```
*The server will start on `http://0.0.0.0:5000`.*

### Start the Frontend Client
In the `frontend` terminal:
```bash
npm run dev
```
*Open your browser to the local URL provided (usually `http://localhost:5173`).*

### Workflow
1.  Enter a research **Topic** (e.g., "Clinical Outcomes of COVID-19").
2.  Drag and drop your PDF files into the upload zone.
3.  Wait for the processing (Loading spinner will appear).
4.  View the results page for the breakdown of Good vs. Bad papers.
5.  Click **Download ZIP** to get your organized files.

## The Language Model

The intelligence behind the classification is handled by a separate core component.

**M.A.R.S. LM (Model Assisted Review System Language Model)** is an AI-powered LLM tool designed to automate the screening phase of Biomedical Systematic Literature Reviews (SLRs). It utilizes a Llama 3.2 3B Instruct model to categorize research papers as Good (Relevant) or Bad (Irrelevant) for a given biomedical topic.

For details on the model training, dataset, and weights, please visit the LM repository:
👉 **[StudyScreeningLanguageModel](https://github.com/Harish25/StudyScreeningLanguageModel)**

## Authors

* Ashwin Vasantharasan
* Harish Umapathithasan
* Nathes Mehanathan
* Nirmalram Kannan

**Supervised by:**
* Dr. Faezeh Ensan

*Developed as part of Final Year Capstone Design Project.*  
*Toronto Metropolitan University, Department of Electrical, Computer, & Biomedical Engineering (2025).*