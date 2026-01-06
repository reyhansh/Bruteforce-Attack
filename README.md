# Bruteforce-Attack
Bruteforce Attack GUI Tool Using Python
import tkinter as tk
from tkinter import scrolledtext, messagebox
import threading
import time

# ---------------------------------------------------------
# Brute-force simulation (safe)
# ---------------------------------------------------------
def brute_force_simulator():
    output_box.delete(1.0, tk.END)

    target_password = password_entry.get().strip()
    if not target_password:
        messagebox.showerror("Error", "Enter a target password to simulate.")
        return

    # Simple dictionary for simulation
    dictionary = ["1234", "password", "admin", "test", "letmein", "qwerty"]

    output_box.insert(tk.END, "[*] Starting brute-force simulation...\n\n")

    # ---------------------------------------------------------
    # 1. DICTIONARY ATTACK SIMULATION
    # ---------------------------------------------------------
    output_box.insert(tk.END, "[*] Running Dictionary Attack...\n")
    for word in dictionary:
        output_box.insert(tk.END, f"Trying: {word}\n")
        output_box.see(tk.END)
        time.sleep(0.3)  # Simulate delay

        if word == target_password:
            output_box.insert(tk.END, f"\n[+] Password Found (Dictionary): {word}\n")
            return

    output_box.insert(tk.END, "\n[-] Dictionary attack failed. Switching to brute force...\n\n")

    # ---------------------------------------------------------
    # 2. BRUTE FORCE ATTACK SIMULATION
    # ---------------------------------------------------------
    charset = "adm123"  # very small for demo
    max_length = 6       # keeps simulation fast

    def generate_strings():
        """Generator function to produce combinations (safe small scope)."""
        from itertools import product
        for length in range(1, max_length + 1):
            for combo in product(charset, repeat=length):
                yield "".join(combo)

    for guess in generate_strings():
        output_box.insert(tk.END, f"Brute Force Trying: {guess}\n")
        output_box.see(tk.END)
        time.sleep(0.05)  # simulate time

        if guess == target_password:
            output_box.insert(tk.END, f"\n[+] Password Found (Brute Force): {guess}\n")
            return

    output_box.insert(tk.END, "\n[!] Password NOT found within simulation limits.\n")

# ---------------------------------------------------------
# Thread wrapper (keeps GUI responsive)
# ---------------------------------------------------------
def start_simulation():
    t = threading.Thread(target=brute_force_simulator)
    t.start()

# ---------------------------------------------------------
# GUI SETUP
# ---------------------------------------------------------
root = tk.Tk()
root.title("PyCyberSuite Brute Force Attack Simulator")
NEON = "#26ff00"
BG = "#000"
root.configure(bg=BG)

tk.Label(root,fg=NEON, bg=BG,text="Enter Target Password (simulation only):").pack(pady=5)
password_entry = tk.Entry(root, width=40, show="*")
password_entry.pack(pady=5)

tk.Button(root, fg=NEON, bg=BG,text="Start Simulation", command=start_simulation).pack(pady=10)

output_box = scrolledtext.ScrolledText(root,fg=NEON, bg=BG, width=60, height=20)
output_box.pack(pady=5)

root.mainloop()
