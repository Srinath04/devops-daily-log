
Today’s CKA learning focused on **Kubernetes Storage** concepts

### **Git Note – `git reset --soft`**

`git reset --soft <commit>` moves the **HEAD pointer** to a specified commit **without touching my working directory or staging area**.

🔹 **Effect:**

- Keeps changes **staged** (in index).
- Only updates the commit history (HEAD).
- Useful for **undoing commits** while preserving staged work.

🧭 **Example:**

`git reset --soft HEAD~1`

This undoes the **last commit** but keeps all changes staged — so that i can recommit with a new message or additional edits.

🪶 **Tip:**  
Used `--soft` As realizing the commit message was okay but accidentally removed previous file.

