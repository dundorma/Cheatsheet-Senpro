# Cheatsheet-Senpro

[SSH Keygen](#ssh-keygen)

# Content
<details>
<summary><h2>SSH Keygen</h2></summary>
  <div>

### Key Generation with `ssh-keygen`

`ssh-keygen` is a command-line tool that generates a new SSH key pair. Here's a basic guide on how it works:

1. **Open a terminal** and run the following command:
   ```bash
   ssh-keygen
   ```

2. **Follow the prompts**:
   - **Enter file in which to save the key**: You can press Enter to accept the default location (usually `~/.ssh/id_rsa`).
   - **Enter passphrase (empty for no passphrase)**: Adding a passphrase adds an extra layer of security, requiring you to enter the passphrase whenever you use the private key.
   - **Enter same passphrase again**: Confirm the passphrase.

3. **Completion**: After you complete these steps, you will have two new files:
   - `id_rsa`: This is your private key. Keep this file secure and never share it.
   - `id_rsa.pub`: This is your public key. You can share this file with any SSH server you want to connect to.

### Using SSH Keys for Authentication

1. **Copy the Public Key to the Server**:
   - You can use `ssh-copy-id` to copy your public key to a server:
     ```bash
     ssh-copy-id user@remote_host
     ```
   - Alternatively, you can manually append the contents of `id_rsa.pub` to the `~/.ssh/authorized_keys` file on the server.

2. **Connect to the Server**:
   - Once the public key is added to the server, you can connect to it without a password (or just with your passphrase if you set one):
     ```bash
     ssh user@remote_host
     ```

     ### Example

Here's a step-by-step example:

1. **Generate SSH Keys**:
   ```bash
   ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
   ```

2. **View the Public Key**:
   ```bash
   cat ~/.ssh/id_rsa.pub
   ```

3. **Copy the Public Key to a Server**:
   ```bash
   ssh-copy-id user@remote_host
   ```
   
4. **Connect to the Server**:
   ```bash
   ssh user@remote_host
   ```
   
  </div>
</details>
