# Cheatsheet-Senpro

# SSH
SSH (Secure Shell) is a protocol that provides a secure channel over an unsecured network by using a client-server architecture. SSH uses a pair of cryptographic keys—a public key and a private key—for authentication and encryption.

### Public and Private Keys

- **Private Key**: This is a secret key that should be kept secure and never shared with anyone. It is used to decrypt data that was encrypted with the corresponding public key.
- **Public Key**: This key can be shared freely and is used to encrypt data that only the private key can decrypt. 

When you connect to an SSH server, the server uses the public key to encrypt a challenge message, which the client must decrypt using the private key to prove its identity.

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

### Summary of SSH Key Benefits

- **Security**: SSH keys are more secure than password-based authentication. Even if an attacker intercepts the data, they cannot decrypt it without the private key.
- **Convenience**: With SSH keys, you can automate secure connections and avoid entering passwords for each connection.

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

By following these steps, you set up a secure and efficient way to authenticate to remote servers using SSH keys.
