# Audio-video-transcription-using-Whisper-AI
import whisper
import os
# add the env_path of ffmpeg in program
os.environ["PATH"] += os.pathsep + "C:\\ffmpeg\\bin"



# Step 1: receive user input as a file path
input_folder = input("Enter the path of the folder: ").strip()

# Step 2: Create a clone Folder (output folder)
output_folder = input_folder + "_transcriptions"
if not os.path.exists(output_folder):
    os.makedirs(output_folder)

# Step 3: Supported File Formats
audio_video_formats = (".mp3", ".wav", ".mp4", ".m4a", ".aac", ".ogg", ".flac")

# Step 4: loading the Whisper Model(tiny Model for Fast Processing)
model = whisper.load_model("tiny")

# Step 5: detects the folders inner audio/video files
for root, dirs, files in os.walk(input_folder):
    for file in files:
        if file.lower().endswith(audio_video_formats):
            file_path = os.path.join(root, file)
            
            print(f"Processing: {file_path}")

            # Step 6: Transcribe Audio/Video File
            result = model.transcribe(file_path)

            # Step 7:Save the output Text File
            text_filename = os.path.splitext(file)[0] + ".txt"
            output_text_path = os.path.join(output_folder, text_filename)

            with open(output_text_path, "w", encoding="utf-8") as f:
                f.write(result["text"])

            print(f"Transcription saved: {output_text_path}")

print("\n All files processed successfully! Check:",output_folder)
