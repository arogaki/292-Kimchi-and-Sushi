Below is a **step-by-step** guide (in **English**) on how to use **Git** (with GitHub) and **branches** to merge four separate Godot 4.3 “projects” into one unified project without significant merge conflicts. 

1. **Prerequisites** and Preparations  
2. **Initial Setup** (Creating a single repository)  
3. **Working with Branches** (One branch per team member/feature)  
4. **Merging Changes** (Pull Requests, conflict resolution, especially for `project.godot` and `.tscn` files)  
5. **Practical Tips** (Godot-specific .gitignore, best practices for scene files, etc.)  

---

## 1. Prerequisites and Preparations

1. **Install Git** on each team member’s machine.  
   - **Windows**: Install via [https://git-scm.com/](https://git-scm.com/). During installation, select “Use Git from the Windows Command Prompt” (or Git Bash, whichever you prefer).  
   - **macOS**: Git often comes pre-installed. If not, install via Homebrew (`brew install git`) or by installing Xcode Command Line Tools.

2. **Create GitHub accounts** for each team member (if not already done).

3. **Install Godot 4.3** on each machine and confirm everyone uses the **same minor version** to minimize project format differences.

4. **Establish a .gitignore** for Godot. Here’s a typical `.gitignore` snippet to place in your repo to avoid pushing unnecessary files:

   ```gitignore
   # Godot-specific
   .import/
   .godot/imported/
   # If you have C# scripts or use Mono
   .mono/
   # OS-specific
   .DS_Store
   Thumbs.db
   # ...
   ```

5. **Decide on a project structure**:
   - You should have **one** `project.godot` file (Godot 4.3 might name it `project.godot` or `project.godot.template`, but by default it’s `project.godot`).  
   - Inside, you’ll have folders like `scenes/`, `scripts/`, `assets/`, etc.
   - Each team member can work on their own scene files: 
     - **You**: Main Character scene (e.g., `scenes/Player.tscn`)  
     - **Team A**: Enemies (e.g., `scenes/Enemy.tscn`)  
     - **Team B**: Title scene/UI (e.g., `scenes/TitleScreen.tscn`)  
     - **Team C**: Background and stage (e.g., `scenes/Stage.tscn`, `scenes/Background.tscn`)  

This way, you minimize direct conflicts since each `.tscn` is separate.

---

## 2. Initial Setup (Creating a Single Repository)

1. **One person** (often the project lead) creates the **initial Godot project** on their local machine. 
   - For example, you create an empty Godot project with the following structure:
     ```
     MyGameProject/
       ├─ project.godot
       ├─ .gitignore
       ├─ scenes/
       ├─ scripts/
       ├─ assets/
       └─ (other folders as needed)
     ```

2. **Initialize Git** locally:

   **Windows (Command Prompt or PowerShell)**:
   ```bash
   cd path\to\MyGameProject
   git init
   git add .
   git commit -m "Initial commit of Godot 4.3 project."
   ```

   **macOS (Terminal)**:
   ```bash
   cd /path/to/MyGameProject
   git init
   git add .
   git commit -m "Initial commit of Godot 4.3 project."
   ```

3. **Create a GitHub repository** (private or public, depending on your needs).  
   - On GitHub, create a new repo named `MyGameProject` (or any name you prefer).

4. **Link local repo to GitHub**:

   **Windows**:
   ```bash
   git remote add origin https://github.com/YourGitHubAccount/MyGameProject.git
   git branch -M main
   git push -u origin main
   ```

   **macOS**:
   ```bash
   git remote add origin https://github.com/YourGitHubAccount/MyGameProject.git
   git branch -M main
   git push -u origin main
   ```

5. **Set up the default branch** to `main` on GitHub if needed (GitHub sometimes defaults to `main` automatically, but confirm in the GitHub repo settings).

---

## 3. Working with Branches

Now we have a single repository containing a single Godot project. Each team member will:

1. **Clone** the repository from GitHub.
2. **Create a branch** for their specific feature (e.g., `feature/player`, `feature/enemy`, etc.).
3. **Commit** and **push** changes to that branch regularly.
4. **Merge** changes via Pull Requests (PRs) on GitHub.

### Cloning the Repository

Each team member does:

**Windows**:
```bash
cd path\to\where\you\want\the\project
git clone https://github.com/YourGitHubAccount/MyGameProject.git
```

**macOS**:
```bash
cd /path/to/where/you/want/the/project
git clone https://github.com/YourGitHubAccount/MyGameProject.git
```

This creates a local folder `MyGameProject/` with the current `main` branch.

### Creating Your Branch

Inside the cloned folder:

**Windows** / **macOS** (Git commands are the same):
```bash
cd MyGameProject
git checkout -b feature/player
```
- `feature/player` is your branch name. Team A might do `git checkout -b feature/enemy`, etc.

### Making Changes and Committing

1. **Open Godot** and add your new scene, for example `scenes/Player.tscn`.
2. Save your scene and confirm it’s inside the `MyGameProject/scenes/` folder.
3. **Commit** your changes:

   ```bash
   git add scenes/Player.tscn
   git commit -m "Add Player scene"
   ```
4. **Push** your branch to GitHub:

   ```bash
   git push -u origin feature/player
   ```
   This uploads your `feature/player` branch to GitHub.

### Regular Workflow (Branch)

- Continue working: edit your scene, scripts, etc.
- `git add .`
- `git commit -m "Description of changes"`
- `git push`

### Staying Updated with `main`

- Occasionally, you want to **pull** updates from `main` (especially if other members merged changes) into your branch:

  ```bash
  # Make sure you're on your feature branch
  git checkout feature/player
  
  # Pull from main
  git pull origin main
  ```
  This merges the latest `main` branch changes into your current `feature/player` branch locally. Resolve any merge conflicts if they appear.

---

## 4. Merging Changes (Pull Requests, Conflict Resolution)

When your feature is ready (or partially ready but you want to share progress):

1. **Open a Pull Request (PR)** on GitHub from your `feature/*` branch **into `main`**.
2. Other team members can review and once approved, you (or the repository owner) **merge** the PR.
3. Now everyone can **pull** from `main` to get your newly added scene(s).

### Conflict Resolution (Especially `project.godot` and `.tscn`)

- **`project.godot`**: 
  - This file can change whenever you add new Input actions or project settings.  
  - Best practice: only one person modifies `project.godot` at a time, or do minimal changes. If conflicts arise:
    1. Open the conflict in a text editor.
    2. Look for `>>>>>>`, `======`, and `<<<<<<` lines.  
    3. Manually keep the correct lines from each version, remove the conflict markers, and save.
    4. `git add project.godot`
    5. `git commit`

- **`.tscn` files**:
  - Each `.tscn` is generally plain text, but conflicts can still occur if two people edit the **same** `.tscn` file.  
  - To avoid this, each team member should ideally work on **separate** scene files. 
  - If you must both edit the same scene, coordinate who merges first, or break it into subscenes. 
  - If a conflict happens, do the same manual text-based merge as above. Then open the `.tscn` in Godot to confirm it still loads correctly.

---

## 5. Practical Tips for Godot Collaboration

1. **Separate Scenes**: As stated, keep each collaborator’s work in separate `.tscn` files when possible. This drastically reduces merge conflicts.

2. **Use Subscenes**: If the main scene is large, break it into smaller subscenes so multiple people can work in parallel.

3. **Godot .gitignore**: Ensure the repository has a proper `.gitignore` so you’re not pushing local or temporary files (e.g., `.import/`, `.mono/` if you do C#, etc.).

4. **Pull Requests**: Always open a PR instead of directly pushing changes to `main`. This helps track changes and properly handle merges.

5. **Communication**: If you know you’ll edit the same file, communicate with your teammates. Minimize friction by merging smaller, more frequent changes.

6. **Periodic Sync**: Don’t wait too long to merge your branch into `main`. Large merges over many weeks can create big conflicts. Merge smaller changes more frequently.

---

## Putting It All Together (Example Flow)

1. **Project Lead**:
   - Initializes the project, sets up repository on GitHub, pushes `main`.
2. **All Team Members**:
   - Clones `main` branch onto their local machines.
   - Creates their own feature branch, e.g., `feature/player`, `feature/enemy`, `feature/title`, `feature/stage`.
3. **Work on Scenes**:
   - Each team member adds or modifies only their relevant `.tscn` files plus related scripts.
   - Commits and pushes to **their** feature branch.
4. **Pull Request**:
   - When a feature is done (or at a stable checkpoint), open a PR from `feature/*` to `main`.
   - Another team member reviews, merges if OK.
5. **Update**:
   - Everyone else pulls from `main` regularly to keep up with changes.
6. **Resolve Conflicts**:
   - If the same file was edited in two branches, handle text-based merges carefully. 
   - Reopen Godot to ensure everything runs properly.

---

## Command Summary (Windows and macOS)

Most Git commands are identical on both Windows and macOS; the only difference is the file path syntax:

- **Initialize Git**:
  ```bash
  git init
  git add .
  git commit -m "Initial commit"
  git remote add origin <URL>
  git push -u origin main
  ```
- **Create/checkout Branch**:
  ```bash
  git checkout -b feature/something
  ```
- **Check Current Branch**:
  ```bash
  git branch
  ```
- **Add/Commit/Pull/Push**:
  ```bash
  # Stage changes
  git add <file or folder> 
  # Commit
  git commit -m "Message"
  # Pull changes from main
  git pull origin main
  # Push your branch
  git push origin feature/something
  ```
- **Merging via GitHub**:
  - **On GitHub**: Create a Pull Request from your feature branch → main.
  - Merge once approved.

---
