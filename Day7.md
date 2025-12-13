### Day-7 Linux SSH Authentication
A secure way to log in to a server remotely using either passwords or SSH key pairs (public/private keys).

SSH key authentication is more secure, convenient, and reliable than password-based login. It’s the standard for Linux servers, DevOps environments, and cloud infrastructure.

# Generate SSH Key-Pair (on your local/jumphost machine)
ssh-keygen                          # Generate SSH key pair

When prompted for file name, press Enter to use the default: ~/.ssh/id_rsa.
When prompted for passphrase, press Enter twice (no password).

ssh-copy-id tony@stapp01 # Automatically copies id_rsa.pub to server's authorized_keys.
After this, login does not require a password.

ls -lah ~/.ssh/      # List ~/.ssh folder to verify keys (id_rsa + id_rsa.pub)
id_rsa → Private key (keep secret!)
id_rsa.pub → Public key (share with servers)

### ==================================================================================================

ssh-keygen                                     # Generate SSH key pair (private + public)
ls -lah ~/.ssh/                                # List ~/.ssh folder to verify keys (id_rsa + id_rsa.pub)

ssh tony@stapp01                               # SSH login to server using password first time
ls -lah ~/.ssh/                                # Check if .ssh folder exists on server
mkdir -p ~/.ssh                                # Create .ssh folder if it doesn't exist
chmod 700 ~/.ssh                               # Set correct permissions for .ssh folder

cat ~/.ssh/id_rsa.pub                           # Display local public key
nano ~/.ssh/authorized_keys                      # Paste public key into server's authorized_keys
chmod 600 ~/.ssh/authorized_keys                # Set correct permissions for authorized_keys

ssh-copy-id tony@stapp01                        # Automatically copy public key to server
ssh tony@stapp01                                # Test SSH login without password

ls -lah ~/.ssh/                                  # Verify .ssh folder contents on server
cat ~/.ssh/authorized_keys                       # Verify your public key exists in authorized_keys

cat ~/.ssh/id_rsa.pub                            # Compare local public key with server authorized_keys

### ==================================================================================================

| Command                            | Purpose                                 |
| ---------------------------------- | --------------------------------------- |
| `ssh-keygen`                       | Generate SSH key pair                   |
| `ls -lah ~/.ssh/`                  | List SSH folder contents                |
| `ssh tony@stapp01`                 | SSH login (password or key-based)       |
| `cat ~/.ssh/id_rsa.pub`            | Display local public key                |
| `nano ~/.ssh/authorized_keys`      | Add public key manually on server       |
| `chmod 600 ~/.ssh/authorized_keys` | Set correct permissions                 |
| `ssh-copy-id tony@stapp01`         | Copy public key to server automatically |
| `cat ~/.ssh/authorized_keys`       | Verify server authorized keys           |

### ==================================================================================================

# What is Linux SSH Authentication?

SSH (Secure Shell) authentication is the process by which a user proves their identity to a Linux server (or any remote machine) in order to securely log in and perform commands.

It is a secure way to access a server remotely instead of using plain-text passwords.

There are two main methods:

1. Password-based Authentication

You log in by typing your username and password.

Less secure because passwords can be guessed, intercepted, or brute-forced.

2. Key-based Authentication (SSH Keys)

Uses a public-private key pair:

Private key → stays on your local machine (secret).

Public key → copied to the server.

When you connect, the server checks if your private key matches the public key.

No password is needed if the key is correct.

# Why Use SSH Key Authentication?

| Reason             | Explanation                                                                     |
| ------------------ | ------------------------------------------------------------------------------- |
| **Security**       | Keys are much harder to guess than passwords; prevents brute-force attacks.     |
| **Convenience**    | Once set up, you can log in without typing a password every time.               |
| **Automation**     | Essential for automated scripts, CI/CD pipelines, and remote server management. |
| **Encryption**     | All communication is encrypted, protecting sensitive data over networks.        |
| **Access Control** | You can easily add or remove user keys without changing passwords.              |


### ==================================================================================================

# Linux SSH Authentication:
A secure way to log in to a server remotely using either passwords or SSH key pairs (public/private keys).

Why use SSH keys:

1. More secure than passwords (harder to guess or brute-force).

2. Convenient: login without typing a password.

3. Supports automation for scripts and server management.