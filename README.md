import tkinter as tk
from tkinter import filedialog, messagebox
import pygame
import os

# Initialize the main app window
root = tk.Tk()
root.title("Music Player")
root.geometry("500x400")

# Initialize pygame mixer
pygame.mixer.init()

# Playlist and current track index
playlist = []
current_track_index = 0

# Functions to manage playback
def play_music():
    if playlist:
        pygame.mixer.music.load(playlist[current_track_index])
        pygame.mixer.music.play()
    else:
        messagebox.showwarning("No Music", "The playlist is empty!")

def pause_music():
    pygame.mixer.music.pause()

def stop_music():
    pygame.mixer.music.stop()

def next_music():
    global current_track_index
    current_track_index = (current_track_index + 1) % len(playlist)
    play_music()

def prev_music():
    global current_track_index
    current_track_index = (current_track_index - 1) % len(playlist)
    play_music()

def set_volume(val):
    volume = float(val) / 100
    pygame.mixer.music.set_volume(volume)

# Functions to manage playlist
def add_music():
    files = filedialog.askopenfilenames(filetypes=[("Music Files", "*.mp3;*.wav")])
    for file in files:
        playlist.append(file)
        listbox.insert(tk.END, os.path.basename(file))

def remove_music():
    selected_items = listbox.curselection()
    if selected_items:
        for index in selected_items[::-1]:
            listbox.delete(index)
            playlist.pop(index)
    else:
        messagebox.showwarning("Selection Error", "No file selected to remove.")

# GUI Layout
# Playback controls
control_frame = tk.Frame(root)
control_frame.pack(pady=20)

play_button = tk.Button(control_frame, text="Play", command=play_music)
play_button.grid(row=0, column=0, padx=10)

pause_button = tk.Button(control_frame, text="Pause", command=pause_music)
pause_button.grid(row=0, column=1, padx=10)

stop_button = tk.Button(control_frame, text="Stop", command=stop_music)
stop_button.grid(row=0, column=2, padx=10)

prev_button = tk.Button(control_frame, text="Previous", command=prev_music)
prev_button.grid(row=0, column=3, padx=10)

next_button = tk.Button(control_frame, text="Next", command=next_music)
next_button.grid(row=0, column=4, padx=10)

# Playlist display
listbox = tk.Listbox(root, selectmode=tk.SINGLE)
listbox.pack(pady=10, fill=tk.BOTH, expand=True)

# Volume control
volume_scale = tk.Scale(root, from_=0, to=100, orient=tk.HORIZONTAL, command=set_volume)
volume_scale.set(50)
volume_scale.pack(pady=20)

# Playlist management buttons
add_button = tk.Button(root, text="Add", command=add_music)
add_button.pack(side=tk.LEFT, padx=20)

remove_button = tk.Button(root, text="Remove", command=remove_music)
remove_button.pack(side=tk.RIGHT, padx=20)

# Start the Tkinter event loop
root.mainloop()
