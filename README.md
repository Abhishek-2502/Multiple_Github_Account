# Using Multiple GitHub Accounts on a Single PC (Windows)

This guide explains how to configure and manage multiple GitHub accounts (e.g., personal and work) on a single PC using SSH.

---

## ✅ **Step 1: Generate SSH Keys for Each Account**

1. Open **Git Bash** and generate an SSH key for your personal account:
    ```bash
    ssh-keygen -t ed25519 -C "abhishek25022004@gmail.com"
    ```
    - Save it as `~/.ssh/id_ed25519` (Inside .ssh folder).

2. Generate a second SSH key for your work account:
    ```bash
    ssh-keygen -t ed25519 -C "abhishekwork@gmail.com"
    ```
    - Save it as `~/.ssh/id_ed25519_work` (Inside .ssh folder).

---

## ✅ **Step 2: Start the SSH Agent and Add the Keys**

1. Start the SSH agent:
    ```bash
    eval $(ssh-agent -s)
    ```

2. Add your keys to the SSH agent:
    ```bash
    ssh-add ~/.ssh/id_ed25519
    ssh-add ~/.ssh/id_ed25519_work
    ```

3. Verify if the keys are added:
    ```bash
    ssh-add -l
    ```

---

## ✅ **Step 3: Configure SSH for GitHub**

1. Open your SSH config file:
    ```bash
    nano ~/.ssh/config
    ```

2. Add the following configuration:
    ```bash
    # Personal Account
    Host github.com-personal
        HostName github.com
        User git
        IdentityFile ~/.ssh/id_ed25519

    # Work Account
    Host github.com-work
        HostName github.com
        User git
        IdentityFile ~/.ssh/id_ed25519_work
    ```

3. Save and exit (`Ctrl + O`, then `Ctrl + X`).

---

## ✅ **Step 4: Add SSH Keys to GitHub**

1. Copy your public keys:
    ```bash
    cat ~/.ssh/id_ed25519.pub
    cat ~/.ssh/id_ed25519_work.pub
    ```
2. Go to GitHub → **Settings** → **SSH and GPG Keys** → **New SSH Key**.
3. Paste the respective keys:
    - **Personal Account** → Paste the `id_ed25519.pub` key.
    - **Work Account** → Paste the `id_ed25519_work.pub` key.

---

## ✅ **Step 5: Test SSH Connections**

Ensure the SSH connections work:

```bash
ssh -T git@github.com-personal
ssh -T git@github.com-work
```

You should see a message like:
> Hi username! You've successfully authenticated, but GitHub does not provide shell access.

---

## ✅ **Step 6: Clone Repositories**

- **For Work Account:**
    ```bash
    git clone git@github.com-work:Username/Repo_name.git
    ```
- **For Personal Account:**
    ```bash
    git clone git@github.com-personal:Username/Repo_name.git
    ```

---

## ✅ **Step 7: Configure Git for Multiple Accounts**

- Set your **personal account** as the global configuration:
    ```bash
    git config --global user.name "Abhishek-2502"
    git config --global user.email "abhishek25022004@gmail.com"
    ```

- When working inside a cloned repository for your **work account**, override the global configuration by executing these command inside cloned repo:
    ```bash
    git config user.name "Work_Username"
    git config user.email "abhishekwork@gmail.com"
    ```

- Verify the configuration:
    ```bash
    git config --get user.name
    git config --get user.email
    ```
    If executed inside a work repository, it will display your work credentials. Outside of any repository, it will show your personal (global) credentials.

---

## ✅ **Additional Step: Set Correct Permissions if Something Fails**

Ensure your SSH keys and config file have the correct permissions:

```bash
chmod 600 ~/.ssh/id_ed25519
chmod 600 ~/.ssh/id_ed25519_work
chmod 600 ~/.ssh/config
```

---

## 🎉 **Conclusion!**
You can now seamlessly manage multiple GitHub accounts using SSH. Let me know if you encounter any issues!

## Author
Abhishek Rajput
