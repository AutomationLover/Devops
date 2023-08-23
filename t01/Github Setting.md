
1. **Generate a Personal Access Token on GitHub**:
    
    - Go to your GitHub account settings.
    - Click on "Developer settings" in the left sidebar.
    - Select "Personal access tokens" under "Access tokens."
    - Click the "Generate token" button.
    - Follow the prompts to configure the token's permissions. At the minimum, you'll need "repo" scope for private repositories or "public_repo" for public repositories.
    - Click the "Generate token" button at the bottom of the page.
    - Copy the generated token. This token is like a password, so keep it secure.
2. **Store Your GitHub Credentials in the macOS Keychain**:
    
    - Open your terminal.
        
    - Run the following command to configure Git to use the macOS Keychain for credentials storage:
        
        bashCopy code
        
        `git config --global credential.helper osxkeychain`
        
    - Then, use the following command to store your GitHub credentials in the Keychain. Replace `YOUR_USERNAME` and `YOUR_TOKEN` with your GitHub username and the personal access token you generated earlier:
        
        bashCopy code
        
        `printf "protocol=https\nhost=github.com\nusername=YOUR_USERNAME\npassword=YOUR_TOKEN\n" | git credential approve`
        
3. **Push to GitHub**:
    
    - Navigate to your local Git repository folder using the terminal.
        
    - Make sure you have committed your changes using `git commit`.
        
    - Use the `git push` command to push your changes to GitHub:
        
        bashCopy code
        
        `git push origin master`
        
        Replace `origin` with the remote repository name (you might have a different name) and `master` with the branch you want to push.
        

Now, both your GitHub username and personal access token should be securely stored in the macOS Keychain, and Git should use them for authentication when you push to GitHub without prompting you for your credentials.