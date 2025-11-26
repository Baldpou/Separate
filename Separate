import os
import math
from pydub import AudioSegment
import shutil

audio_files = [f for f in os.listdir('.') if os.path.isfile(f) and f.lower().endswith(('.mp3', '.wav', '.flac', '.ogg', '.m4a', '.aac', '.wma'))]

for i, file in enumerate(audio_files, 1):
    print(f"{i}. {file}")

file_choice = input("file: ")
selected_file = audio_files[int(file_choice) - 1]

base_name_input = input("name: ")
if not base_name_input.strip():
    base_name = os.path.splitext(selected_file)[0]
else:
    base_name = base_name_input

main_folder = base_name
if not os.path.exists(main_folder):
    os.makedirs(main_folder)

print("in progress...")
if selected_file.lower().endswith('.mp3'):
    audio = AudioSegment.from_mp3(selected_file)
else:
    audio = AudioSegment.from_file(selected_file)
    mp3_temp_file = os.path.join(main_folder, "temp_converted.mp3")
    audio.export(mp3_temp_file, format="mp3", bitrate="64k")

twenty_nine_minutes_ms = 29 * 60 * 1000
total_duration = len(audio)
num_chunks = math.ceil(total_duration / twenty_nine_minutes_ms)

for i in range(num_chunks):
    start_time = i * twenty_nine_minutes_ms
    end_time = min((i + 1) * twenty_nine_minutes_ms, total_duration)
    
    chunk = audio[start_time:end_time]
    
    low_quality_file = os.path.join(main_folder, f"{base_name}_{i + 1}.mp3")
    chunk.export(low_quality_file, format="mp3", bitrate="64k")
    
    txt_file = os.path.join(main_folder, f"{base_name}_{i + 1}.txt")
    open(txt_file, 'w').close()

original_name, ext = os.path.splitext(selected_file)
new_original_name = f"{base_name}_original{ext}"
shutil.move(selected_file, os.path.join(main_folder, new_original_name))

temp_file_path = os.path.join(main_folder, "temp_converted.mp3")
if os.path.exists(temp_file_path):
    os.remove(temp_file_path)

print("done")
