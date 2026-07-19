# MM-FinEval: A Multi-Task Multimodal Benchmark for Real-World Financial Forecasting

This repository provides a foundational template for iterating over a structured multimodal dataset containing text transcripts, PDF slides, and audio files. It is designed to help you quickly load your data year-by-year, resolve the associated media file paths, and structure the data for your own custom processing pipelines

## Usage Example
Below is the boilerplate Python code. All Large Language Model (LLM) initialization, base64 encoding, and batching logic have been removed and replaced with placeholders so you can integrate your own tools seamlessly.

``` python
import json
import os

# --- Configuration & Paths ---
YEARS = [2019, 2020, 2021, 2022]
TASK = "car" # You can choose any task: [car, ret_eom, ret_ldm, ret_l5m, vol_fut_3d, vol_fut_7d, vol_fut_15d, vol_fut_30d, vol_past_3d, vol_past_7d, vol_past_15d, vol_past_30d]

# ==========================================
# TODO: INITIALIZE YOUR CUSTOM MODEL HERE
# Example: model = MyCustomProcessor()
# ==========================================

def process_multimodal_data():
    # Iterate through the data year by year
    for YEAR in YEARS:
        print(f"\n{'='*40}")
        print(f"📅 Processing Year: {YEAR}")
        print(f"{'='*40}")

        # Define dataset paths
        JSON_FILE = f"...your_path/data/labeled_transcripts_{YEAR}.json"
        PDF_FOLDER = f"...your_path/data/pt_{YEAR}"
        AUDIO_FOLDER = f"...your_path/data/mp3_{YEAR}"
        
        # Load the JSON data mapping
        try:
            with open(JSON_FILE, 'r', encoding='utf-8') as f:
                data = json.load(f)
            print(f"✅ Successfully loaded {len(data)} entries from {JSON_FILE}")
        except FileNotFoundError:
            print(f"❌ Error: '{JSON_FILE}' not found. Skipping year {YEAR}.")
            continue

        # Process each call entry
        for call_id, call_data in data.items():
            
            # 1. Extract metadata and text
            text_transcript = call_data.get("input")
            ppt_id = call_data.get("ppt_id")
            mp3_id = call_data.get("mp3_id")
            ground_truth = call_data.get(TASK) 
            
            # 2. Construct media paths
            pdf_folder_path = os.path.join(PDF_FOLDER, f"{ppt_id}.pdf")
            audio_file_path = os.path.join(AUDIO_FOLDER, f"{mp3_id}.mp3")
            
            # =========================================================
            # TODO: DO WHATEVER YOU WANT WITH THE EXTRACTED DATA HERE
            # =========================================================
            
            # Example Data Execution Placeholder:
            print(f"\nProcessing Call {call_id}:")
            print(f" - Target Variable ({TASK}): {ground_truth}")
            print(f" - Transcript Length: {len(text_transcript) if text_transcript else 0} chars")
            print(f" - Audio: ({audio_file_path})")
            print(f" - PDF: ({pdf_folder_path})")
            
            # =========================================================

if __name__ == "__main__":
    process_multimodal_data()
```
