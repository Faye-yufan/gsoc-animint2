---
layout: post
title: animint2pages()
subtitle: New feature
gh-repo: animint/animint2
gh-badge: [star, fork, follow]
tags: [pr, contribution, gsoc2023]
comments: true
---

## Motivation
[bl.ock.org](http://bl.ock.org) stop functioning since Jan, 2023 (the github repo shows it is archived by the owner on Jan 10, 2023). One of the convenient way for the animint2 users is to share our animint visualization through `animint2gist()` and bl.ock. 

So we decide to move the gist to github pages, which is also a non-server way to share pure html/js, like our animint viz, on the internet.

## Challenges
### **1. S3 Classes in R:**

R's S3 system is a simple object-oriented system. I encountered the nuances of S3 classes when the error **`Error in 'git2r_branch_create': 'commit' must be an S3 class git_commit`** was raised. It turns out, before creating a branch, it's vital to ensure that there's an initial commit to provide the branch a starting point.

### **2. `git2r` and GitHub operations:**

I walked through a comprehensive script aimed to automate the process of:

- Checking if a GitHub repo exists
- Creating it if it doesn't
- Initializing a local git repo
- Checking for and creating a 'gh-pages' branch
- Committing and pushing changes to GitHub

However, it wasn't without challenges. For example, when there's a fresh GitHub repository with no commits, some operations might fail because they expect certain git states.

### **3. Resolving Errors:**

I addressed a couple of specific errors:

- Issues with the **`git2r::branch_rename()`** function
- **`Error in 'git2r_push': Unable to authenticate with supplied credentials`**

For the latter, I learned about the importance of proper authentication. I explored **`git2r::cred_token()`** for authentication, but it seems that setting up SSH or token-based authentication is crucial for a seamless push operation.

### **4. Handling Personal Access Tokens (PATs):**

I saw a code snippet where the **`GITHUB_PAT`** environmental variable is set if it's empty. It's a good security practice not to hard-code tokens. Instead, a more user-friendly approach involves prompting the user to input their token if not found in the environment.

### **5. SSH Keys:**

Towards the end, I touched upon generating SSH keys in different formats. I clarified the distinction between PEM and OpenSSH formats and highlighted the importance of matching key types with their use cases.
### **6. Setting Up GitHub Authentication**
Initially, the function didn’t work as expected. It required GitHub authentication. One way to authenticate is to set up a GitHub token, 
which can be done from your GitHub account settings. I used `gitcreds::gitcreds_get()` to get the credential in the environment.
After some analysis, see [here](https://github.com/animint/animint2/pull/101#discussion_r1328033433), I decided to use `gitcreds_get()` still.
