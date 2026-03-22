# 🔐 GitHub SSH + GPG Setup (Arch + Fish)

## 🧠 Goal

Secure GitHub workflow with:

- SSH authentication
    
- GPG signed commits
    
- Privacy email
    
- Clean Fish shell integration
    

---

# 1️⃣ Install Required Packages

```bash
sudo pacman -S git gnupg openssh fish pinentry github-cli
```

---

# 2️⃣ Setup SSH Key (GitHub Authentication)

## Generate SSH Key

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```

Press Enter for default location.

---

## Start SSH Agent (Fish)

```fish
eval (ssh-agent -c)
```

---

## Add SSH Key

```bash
ssh-add ~/.ssh/id_ed25519
```

---

## Copy Public Key

```bash
cat ~/.ssh/id_ed25519.pub
```

---

## Add to GitHub

GitHub → Settings → SSH Keys → New → Paste key

---

## Test Connection

```bash
ssh -T git@github.com
```

Expected:

```
Hi username! You've successfully authenticated.
```

---

# 3️⃣ Setup GPG Key (Commit Signing)

## Generate Key

```bash
gpg --full-generate-key
```

Choose:

- RSA and RSA
    
- 4096 bits
    
- Expiry: 1y or no expiry
    
- Name + Email
    

---

## List Keys

```bash
gpg --list-secret-keys --keyid-format LONG
```

Example:

```
sec rsa4096/ABC123456789
```

---

## Export Public Key

```bash
gpg --armor --export ABC123456789
```

---

## Add to GitHub

GitHub → Settings → GPG Keys → New → Paste key

---

# 4️⃣ Configure Git

```bash
git config --global user.name "0x0Axlw"
git config --global user.email " 238889558+0x0Axlw@users.noreply.github.com"
git config --global user.signingkey 9D427963653178D2
git config --global commit.gpgsign true
git config --global init.defaultBranch main
```



# 6️⃣ Fish Shell SSH Agent Auto Start

Edit:

```
~/.config/fish/config.fish
```

Add:

```fish
if not set -q SSH_AUTH_SOCK
    eval (ssh-agent -c) >/dev/null
    ssh-add ~/.ssh/id_ed25519 >/dev/null 2>&1
end
```

---

# 7️⃣ Verify GPG Signing

```bash
git commit -m "test"
git log --show-signature
```

Expected:

```
Good signature
```

---

# ✅ Final Setup

- SSH → Authentication ✔
    
- GPG → Verified commits ✔
    
- Fish → Auto agent ✔
    
- GitHub → Secure + private ✔
    

---

# ⚡ Pro Tips

- Use GitHub noreply email for privacy
    
- Always keep `commit.gpgsign = true`
    
- Backup your GPG key securely
    
- Never share private keys
    

---

# 🧠 Mental Model

```
SSH → Who are you (authentication)
GPG → Did you really write this (verification)
```

---

> “Security isn’t extra work. It’s disciplined setup done once.”
---
 
