# Simple AI APIs - Abhay G K

## How it works

This project implements three core AI APIs using FastAPI and OpenAI, along with a very basic HTML/CSS/JS frontend served directly by FastAPI.

* **Chat API (`/chat`):** Provides a direct interface to OpenAI's `gpt-4o-mini` model for general conversational AI.
* **Document Upload API (`/upload`):** Allows users to upload `.txt` or `.pdf` documents. The content is extracted, chunked, embedded using `text-embedding-3-small`, and then stored in a Qdrant vector database.
* **RAG Chat API (`/rag-chat`):** Combines the power of the `gpt-4o-mini` model with the uploaded document's context. When a user asks a question, relevant chunks are retrieved from Qdrant and provided to the OpenAI model as context to generate a more informed response.
* **Basic Chat UI:** A minimal web interface built with plain HTML, CSS, and JavaScript, served directly by the FastAPI application at the root URL (`/`).

## Tech Stack

* **Backend:** FastAPI (Python)
* **Frontend:** Plain HTML, CSS, JavaScript (served statically by FastAPI)
* **AI:** OpenAI API (`gpt-4o-mini`, `text-embedding-3-small`)
* **Vector Database:** Qdrant

## Setup Instructions

Follow these steps to set up and run the project locally.

### Prerequisites

* Python 3.9+
* **Docker Desktop** (essential for running Qdrant)
* An OpenAI API Key (currently hardcoded in `main.py` for simplicity of setup).

### Running the Application

1.  **Get the Code:**
    Ensure all project files (`main.py`, `requirements.txt`, `README.md`, and the `static` folder containing `index.html`) are in a single directory on your computer.

2.  **Install Python dependencies:**
    * Open your terminal or command prompt.
    * Navigate to your project's root directory (where `main.py` is located).
    * Run the following command to install the required Python libraries:
        ```bash
        pip install -r requirements.txt
        ```
    *(Note: This installs packages globally. For larger projects, using a Python [virtual environment](https://docs.python.com/3/library/venv.html) is typically recommended for better dependency management.)*

3.  **Start Qdrant (Vector Database):**
    * Open a **SECOND, NEW** terminal/command prompt window.
    * Ensure Docker Desktop is running in the background on your machine.
    * Run the Qdrant Docker container using this command:
        ```bash
        docker run -p 6333:6333 -p 6334:6334 -v "$(pwd)/qdrant_storage:/qdrant/storage" qdrant/qdrant
        ```
    * **Keep this second terminal window open.** It will display Qdrant's logs as it runs.

4.  **Start the FastAPI Application (Backend & Frontend Server):**
    * Go back to your **FIRST** terminal window (where you installed Python libraries).
    * Run your FastAPI application:
        ```bash
        uvicorn main:app --host 0.0.0.0 --port 8000 --reload
        ```
    * You should see messages indicating that the server has started successfully, typically saying `INFO: Uvicorn running on http://0.0.0.0:8000`. Keep this terminal window open.

5.  **Access the Web Interface:**
    * Open your web browser (Chrome, Firefox, Edge, etc.).
    * Go to the following address:
        ```
        http://localhost:8000/
        ```
    * The basic chat interface will load, and you can start typing messages to interact with the AI. You can also use tools like Postman or the `/docs` endpoint (`http://localhost:8000/docs`) to test the `/upload` and `/rag-chat` functionalities.

## API Endpoints

* **Endpoint:** `POST /chat`
    * **Description:** Sends a user message to OpenAI's `gpt-4o-mini` model and returns the AI's response.
    * **Frontend Interaction:** The chat UI directly communicates with this endpoint.

* **Endpoint:** `POST /upload`
    * **Description:** Accepts a `.txt` or `.pdf` file, extracts its content, chunks it, embeds the chunks, and stores them in Qdrant for RAG.
    * **Usage:** Requires sending a `multipart/form-data` request with the file.

* **Endpoint:** `POST /rag-chat`
    * **Description:** Answers questions based on the content of a previously uploaded document using Retrieval-Augmented Generation.
    * **Payload:** Requires both `message` (your question) and `document_id` (from a prior `/upload` call).

## For questions: connect@botifynow.ai
