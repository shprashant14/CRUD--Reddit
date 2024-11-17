# Reddit CRUD Operations using Python and PRAW

This project demonstrates how to perform **CRUD (Create, Read, Update, Delete)** operations on **Reddit** using **Python** and the **PRAW (Python Reddit API Wrapper)** library. The code is implemented in a **Google Colab Notebook** to interact with the Reddit API seamlessly.

## Table of Contents
- [Features](#features)
- [Prerequisites](#prerequisites)
- [Setup Instructions](#setup-instructions)
- [CRUD Operations](#crud-operations)
  - [1. Create a New Post](#1-create-a-new-post)
  - [2. Read Recent Posts](#2-read-recent-posts)
  - [3. Update an Existing Post](#3-update-an-existing-post)
  - [4. Delete a Post](#4-delete-a-post)
- [Notes](#notes)
- [License](#license)

## Features
- **Create** a new Reddit post in a specified subreddit.
- **Read** the latest posts from a specified subreddit.
- **Update** the content of an existing Reddit post.
- **Delete** an existing Reddit post.
- Uses **PRAW** for easy interaction with the Reddit API.
- The entire project is implemented in **Google Colab** for ease of use.

## Prerequisites
Before running this project, ensure you have the following:
- A **Reddit Account** (If you don’t have one, you can create it [here](https://www.reddit.com/register)).
- A **Reddit Application** registered to obtain the necessary API credentials. 

### How to Register a Reddit Application:
1. **Login to your Reddit account**.
2. Go to [Reddit App Preferences](https://www.reddit.com/prefs/apps).
3. Click on **Create App**.
4. Fill out the form:
   - **Name**: (e.g., `RedditCRUDApp`)
   - **App type**: Select **script**.
   - **Redirect URI**: Use `http://localhost:8080`
5. Click **Create App**.
6. Note down the following credentials:
   - **Client ID**
   - **Client Secret**
   - **User Agent** (e.g., `"myRedditApp/0.1 by yourusername"`)

## Setup Instructions
1. **Clone the repository**:
   ```bash
   git clone https://github.com/yourusername/reddit-crud.git
   cd reddit-crud
   ```

2. **Open Google Colab** and upload the notebook or create a new one.

3. **Install the `praw` library** by running:
   ```python
   !pip install praw
   ```

4. **Authenticate with Reddit API** by configuring your credentials:
   ```python
   import praw

   # Set up Reddit API client
   reddit = praw.Reddit(
       client_id="YOUR_CLIENT_ID",
       client_secret="YOUR_CLIENT_SECRET",
       user_agent="YOUR_USER_AGENT",
       username="YOUR_USERNAME",
       password="YOUR_PASSWORD"
   )

   # Test the connection
   try:
       print(f"Authenticated as: {reddit.user.me()}")
   except Exception as e:
       print("Failed to authenticate:", e)
   ```

## CRUD Operations

### 1. Create a New Post
Create a new post in a subreddit.

```python
def create_post(subreddit_name, title, content):
    subreddit = reddit.subreddit(subreddit_name)
    post = subreddit.submit(title, selftext=content)
    print(f"Created post: {post.title} (ID: {post.id})")
    
create_post("test", "Hello Reddit from Python!", "This is a post created using PRAW in Google Colab.")
```

### 2. Read Recent Posts
Fetch recent posts from a subreddit.

```python
def read_posts(subreddit_name, limit=5):
    subreddit = reddit.subreddit(subreddit_name)
    for submission in subreddit.new(limit=limit):
        print(f"Title: {submission.title}\nScore: {submission.score}\nID: {submission.id}\n")

read_posts("test", limit=2)
```

### 3. Update an Existing Post
Edit the content of an existing Reddit post.

```python
def update_post(post_id, new_content):
    submission = reddit.submission(id=post_id)
    submission.edit(new_content)
    print(f"Updated post with ID {post_id}: {submission.title}")

update_post("POST_ID_HERE", "Updated content for my post.")
```

### 4. Delete a Post
Delete an existing post from Reddit.

```python
def delete_post(post_id):
    submission = reddit.submission(id=post_id)
    submission.delete()
    print(f"Deleted post with ID: {post_id}")

delete_post("POST_ID_HERE")
```

## Notes
- Ensure your **client ID**, **client secret**, and **password** are kept private.
- Avoid violating Reddit’s [API rules](https://github.com/reddit-archive/reddit/wiki/API) to prevent your account from being banned.
- Be mindful of Reddit’s rate limits when using the API to avoid being temporarily blocked.

## License
This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
