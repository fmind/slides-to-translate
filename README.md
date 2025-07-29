# Google Slides Translator with Vertex AI and Gemini

This project demonstrates how to automatically translate the text content of a Google Slides presentation into a target language using Vertex AI and the Gemini family of models.

The notebook is designed to be run in a Google Colab environment and leverages Google's APIs for Slides, Drive, and Vertex AI to perform the translation.

## How it Works

The process is broken down into the following steps:

1. **Copy Presentation**: A copy of the original Google Slides presentation is created in your Google Drive to avoid modifying the original file. This new presentation is titled with the original title and the target language for easy identification.
    
2. **Extract Text**: The notebook then extracts all individual text elements from every slide in the newly created presentation. It intelligently filters out empty strings and non-textual elements to create a list of unique text runs that need translation.
    
3. **Translate Text**: Using the specified Gemini model via the Vertex AI API, each unique text run is translated into the target language. The notebook uses a `ThreadPoolExecutor` to handle multiple translation requests concurrently, which significantly speeds up the process. An optional `EXTRA_PROMPT` can be provided to give the model more context about the presentation's topic, improving the accuracy of the translation.
    
4. **Replace Text**: Once the translations are complete, the notebook creates a batch of "replace all text" requests for the Google Slides API. These requests replace the original text in the copied presentation with their translated counterparts. The requests are sorted from the longest text to the shortest to prevent issues where a shorter piece of text might be a substring of a longer one.
    
5. **Report Cost**: Finally, the notebook calculates and reports the total number of input and output tokens used for the translation and provides an estimated cost based on the pricing for the selected Gemini model.
    

## Features

- **Multiple Language Support**: Translate your slides to any language supported by the Gemini models.
    
- **Choice of Models**: Select from different Gemini models available on Vertex AI, such as `gemini-1.5-flash` and `gemini-1.5-pro`, to balance cost and translation quality.
    
- **Contextual Translation**: Use the `EXTRA_PROMPT` to provide context to the model, leading to more accurate and relevant translations.
    
- **Concurrent Processing**: Leverages multithreading to perform translations in parallel, making the process faster for presentations with a lot of text.
    
- **Cost Estimation**: Get a clear estimate of the cost of the translation based on token usage.
    
- **Non-destructive**: The original presentation is never modified. All translations are performed on a copy.
    

## How to Use

To use this project, you will need:

- A Google Cloud Platform (GCP) project with the Vertex AI API enabled.
    
- Permissions to use Google Drive, Google Slides, and Vertex AI.
    

1. **Open the Notebook**: The project is contained within a single Jupyter Notebook (`Slides-to-Translate - Translate Google Slides Automatically with Gemini on Vertex AI.ipynb`). Open it in Google Colab.
    
2. **Configure User Settings**: In the "User Configuration" cell, you will need to provide:
    
    - Your GCP **`PROJECT_ID`**.
        
    - The **`PRESENTATION_ID`** of the Google Slides presentation you want to translate.
        
    - The **`TARGET_LANGUAGE`** for the translation.
        
    - The desired **`MODEL_NAME`**.
        
    - The Vertex AI **`LOCATION`**.
        
    - An optional **`EXTRA_PROMPT`** to guide the model.
        
3. **Run the Notebook**: Execute all the cells in the notebook in sequence. You will be prompted to authenticate with your Google account to grant the necessary permissions.
    
4. **Access Translated Slides**: Once the notebook has finished running, a link to the translated copy of your presentation will be provided in the output of the final cells.
