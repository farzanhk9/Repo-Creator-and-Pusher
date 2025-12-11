import requests
import os
import subprocess

# GitHub API URL to create a new repository
GITHUB_API_URL = "https://api.github.com/user/repos"

# GitHub authentication token (replace with your personal access token)
GITHUB_TOKEN = "your_personal_access_token"

# GitHub username (replace with your username)
GITHUB_USER = "your_github_username"

# Function to create a repository on GitHub
def create_github_repo(repo_name):
    headers = {
        "Authorization": f"token {GITHUB_TOKEN}",
        "Accept": "application/vnd.github.v3+json"
    }
    data = {
        "name": repo_name,
        "description": "This is a new repository created via script.",
        "private": False  # Set to True for private repository
    }

    response = requests.post(GITHUB_API_URL, json=data, headers=headers)
    
    if response.status_code == 201:
        print(f"Repository '{repo_name}' created successfully on GitHub.")
        return response.json()['html_url']
    else:
        print(f"Failed to create repository: {response.json()['message']}")
        return None

# Function to initialize a local Git repository and push to GitHub
def initialize_and_push_to_github(repo_name, github_url):
    # Initialize the Git repository
    subprocess.run(["git", "init"], check=True)
    
    # Add all files to the staging area
    subprocess.run(["git", "add", "."], check=True)
    
    # Commit the changes
    subprocess.run(['git', 'commit', '-m', '"Initial commit"'], check=True)
    
    # Add the remote GitHub URL
    subprocess.run(["git", "remote", "add", "origin", github_url], check=True)
    
    # Push to GitHub
    subprocess.run(["git", "push", "-u", "origin", "master"], check=True)
    
    print(f"Code pushed to repository: {github_url}")

# Main Program
if __name__ == "__main__":
    repo_name = "new-repo-from-script"  # Name of your new repository
    
    # Create the repository on GitHub
    github_url = create_github_repo(repo_name)
    
    if github_url:
        # Initialize local Git repo and push to GitHub
        initialize_and_push_to_github(repo_name, github_url)

