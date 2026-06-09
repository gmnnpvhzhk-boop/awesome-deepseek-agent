        self.start_time = time.time()
        logging.info(f"AlienSystemMonitor initialized for volume: {self.target_volume}")

    def generate_file_hash(self, file_path: str, block_size: int = 65536) -> str:
        """Generates a SHA-256 cryptographic hash for a given file to ensure data integrity."""
        sha256_hash = hashlib.sha256()
        try:
            with open(file_path, "rb") as f:
                for byte_block in iter(lambda: f.read(block_size), b""):
                    sha256_hash.update(byte_block)
            return sha256_hash.hexdigest()
        except FileNotFoundError:
            logging.error(f"File not found for hashing: {file_path}")
            return ""
        except PermissionError:
            logging.error(f"Permission denied while accessing: {file_path}")
            return ""

    def scan_target_volume(self) -> Dict[str, Any]:
        """Scans the target volume, registers files, and records their integrity hashes."""
        logging.info(f"Starting volume scan on: {self.target_volume}")
        if not os.path.exists(self.target_volume):
            logging.error(f"Target volume does not exist: {self.target_volume}")
            return self.registry

        for root, _, files in os.walk(self.target_volume):
            for file in files:
                full_path = os.path.join(root, file)
                file_hash = self.generate_file_hash(full_path)
                if file_hash:
                    self.registry[full_path] = {
                        "hash": file_hash,
                        "timestamp": time.time(),
                        "size": os.path.getsize(full_path)
                    }
        
        logging.info(f"Scan complete. Registered {len(self.registry)} files.")
        return self.registry
python editor.py
import platform
import sys

print("--- System Info ---")
print(f"Python Version: {platform.python_version()}")
print(f"OS Platform: {platform.system()} {platform.release()}")

print("\n--- Loaded Modules ---")
# Lists common modules if they are ready to use
modules = ['tkinter', 'customtkinter', 'json', 'os']
for mod in modules:
    try:
        __import__(mod)
        print(f"  [✓] {mod} is available")
    except ImportError:
        print(f"  [ ] {mod} is NOT installed")

import sys
import subprocess
import tkinter as tk
from tkinter import filedialog, messagebox
import customtkinter as ctk

# Set modern UI theme
ctk.set_appearance_mode("Dark")
ctk.set_default_color_theme("blue")

class PythonEditor(ctk.CTk):
    def __init__(self):
        super().__init__()
        self.title("RAMII Python Code Editor")
        self.geometry("900x700")
        
        self.current_file = None
        self.create_widgets()
        
    def create_widgets(self):
        # --- TOP MENU BAR ---
        self.menu_frame = ctk.CTkFrame(self, height=40, corner_radius=0)
        self.menu_frame.pack(side="top", fill="x")
        
        btn_open = ctk.CTkButton(self.menu_frame, text="Open", width=70, command=self.open_file)
        btn_open.pack(side="left", padx=5, pady=5)
        
        btn_save = ctk.CTkButton(self.menu_frame, text="Save", width=70, command=self.save_file)
        btn_save.pack(side="left", padx=5, pady=5)
        
        btn_run = ctk.CTkButton(self.menu_frame, text="▶ Run", width=70, fg_color="#2ecc71", hover_color="#27ae60", command=self.run_code)
        btn_run.pack(side="left", padx=5, pady=5)

        # --- MAIN PANELS (SPLIT SCREEN) ---
        # PanedWindow handles the adjustable boundary between editor and terminal
        self.paned_window = tk.PanedWindow(self, orient=tk.VERTICAL, bg="#1d1e22", bd=0, sashwidth=6)
        self.paned_window.pack(fill="both", expand=True)
        
        # --- EDITOR AREA ---
        self.editor_frame = ctk.CTkFrame(self.paned_window, corner_radius=0)
        self.text_area = tk.Text(
            self.editor_frame, 
            wrap="none", 
            background="#1e1e1e", 
            foreground="#d4d4d4", 
            insertbackground="white", # Cursor color
            font=("Consolas", 12),
            bd=0,
            highlightthickness=0
        )
        self.text_area.pack(side="left", fill="both", expand=True, padx=5, pady=5)
        
        # Add basic scrollbar to editor
        editor_scroll = ctk.CTkScrollbar(self.editor_frame, command=self.text_area.yview)
        editor_scroll.pack(side="right", fill="y")
        self.text_area.config(yscrollcommand=editor_scroll.set)
        
        # --- TERMINAL / OUTPUT AREA ---
        self.output_frame = ctk.CTkFrame(self.paned_window, corner_radius=0)
        self.output_area = tk.Text(
            self.output_frame, 
            background="#141414", 
            foreground="#85929e", 
            font=("Consolas", 11),
            bd=0,
            highlightthickness=0
        )
        self.output_area.pack(side="left", fill="both", expand=True, padx=5, pady=5)
        
        # Bind typing events to apply basic syntax highlighting formatting
        self.text_area.bind("<KeyRelease>", self.apply_highlighting)
        self.setup_tags()

        # Add components to split screen view
        self.paned_window.add(self.editor_frame, minsize=300)
        self.paned_window.add(self.output_frame, minsize=150)

    def setup_tags(self):
        # Visual color rules for keywords
        self.text_area.tag_config("keyword", foreground="#569cd6")
        self.text_area.tag_config("string", foreground="#ce9178")
        self.text_area.tag_config("comment", foreground="#6a9955")

    def apply_highlighting(self, event=None):
        # Simple string-matching syntax highlighting logic
        words = ["def", "class", "import", "from", "return", "if", "else", "elif", "for", "while", "print", "in", "True", "False"]
        
        # Clear existing highlighting tags
        for tag in ["keyword", "string", "comment"]:
            self.text_area.tag_remove(tag, "1.0", tk.END)
            
        content = self.text_area.get("1.0", tk.END)
        lines = content.split("\n")
        
        for idx, line in enumerate(lines):
            line_num = idx + 1
            
            # Highlight keywords
            for word in words:
                start_pos = 0
                while True:
                    start_pos = line.find(word, start_pos)
                    if start_pos == -1:
                        break
                    # Ensure it is a distinct word, not a substring
                    if (start_pos == 0 or not line[start_pos-1].isalnum()) and \
                       (start_pos + len(word) == len(line) or not line[start_pos + len(word)].isalnum()):
                        start = f"{line_num}.{start_pos}"
                        end = f"{line_num}.{start_pos + len(word)}"
                        self.text_area.tag_add("keyword", start, end)
                    start_pos += len(word)
                    
            # Basic comment highlighting (#)
            if "#" in line:
                comment_idx = line.find("#")
                self.text_area.tag_add("comment", f"{line_num}.{comment_idx}", f"{line_num}.{len(line)}")

    # --- FILE ACTIONS ---
    def open_file(self):
        file_path = filedialog.askopenfilename(filetypes=[("Python Files", "*.py"), ("All Files", "*.*")])
        if file_path:
            self.current_file = file_path
            self.text_area.delete("1.0", tk.END)
            with open(file_path, "r", encoding="utf-8") as file:
                self.text_area.insert("1.0", file.read())
            self.apply_highlighting()
            self.title(f"RAMII Python Code Editor - {file_path}")

    def save_file(self):
        if not self.current_file:
            self.current_file = filedialog.asksaveasfilename(defaultextension=".py", filetypes=[("Python Files", "*.py")])
        
        if self.current_file:
            with open(self.current_file, "w", encoding="utf-8") as file:
                file.write(self.text_area.get("1.0", tk.END))
            self.title(f"RAMII Python Code Editor - {self.current_file}")
            messagebox.showinfo("Success", "File saved successfully!")

    # --- CODE EXECUTION ---
    def run_code(self):
        # Auto-save changes into a temp file if no file path exists yet
        if not self.current_file:
            messagebox.showwarning("Save Required", "Please save your Python file before running.")
            self.save_file()
            if not self.current_file:
                return

        # Clear output pane
        self.output_area.delete("1.0", tk.END)
        self.output_area.insert("1.0", "Running script...\n" + "-"*50 + "\n")
        
        try:
            # Safely trigger native python execution framework
            process = subprocess.Popen(
                [sys.executable, self.current_file],
                stdout=subprocess.PIPE,
                stderr=subprocess.PIPE,
                text=True
            )
            stdout, stderr = process.communicate()
            
            if stdout:
                self.output_area.insert(tk.END, stdout)
            if stderr:
                self.output_area.insert(tk.END, f"\nERROR:\n{stderr}", "error")
                self.output_area.tag_config("error", foreground="#e74c3c")
        except Exception as e:
            self.output_area.insert(tk.END, f"Failed to execute program: {str(e)}")

if __name__ == "__main__":
    app = PythonEditor()
    app.mainloop()

pip install customtkinter
python editor.py

RAMII-2 Alien.py_v2 Alpha owl adam   
If your account is named RAMII, you likely experienced a data breach or unauthorized repository access resulting in your Python code being scraped or duplicated by ChatGPT or GitLab. This frequently occurs due to public repositories or leaked API keys. 

GitHub
 +1
You can protect your code and revoke unauthorized access through these immediate steps:
1. Revoke API Keys and Access
GitHub & GitLab: Go to your account settings to review your Personal Access Tokens or GitLab's Access Tokens. Delete any tokens you do not recognize.
ChatGPT: If you are using ChatGPT as an IDE assistant, ensure you have not publicly shared conversation links containing your codebase. 
2. Secure Your Account
Force a global sign-out across all sessions. On GitHub, you can terminate all other web sessions under Security Settings.
Enable Two-Factor Authentication (2FA) if it is not already on to prevent unauthorized logins. 
3. Change Visibility Settings
Review your repositories on both platforms. If you do not intend for your code to be public, toggle your repositories from Public to Private in their respective settings.
4. Intellectual Property & Code Theft
If you are dealing with copyright issues or unauthorized commercial use of your code, file a formal DMCA takedown notice. For unauthorized code hosting, submit a request via the GitHub DMCA Takedown Form. If the issue is related to your ChatGPT history being exposed, you can reach out via the OpenAI Help Center.
pip install python-gitlab import gitlab  # 1. Authenticate with your GitLab Personal Access Token gl = gitlab.Gitlab('https://gitlab.com', private_token='YOUR_GITLAB_TOKEN')  # 2. List all projects on your account projects = gl.projects.list(owned=True)  for project in projects:     print(f"Project Name: {project.name}")     print(f"Project URL: {project.web_url}")     print("-" * 20) 