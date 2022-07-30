# GitHub API tools

GitHub API tools to use with your own repositories.

These tools are simple and intended for learning:

* Python 3 with pip3

* GitHub API: https://developer.github.com/v3/

* PyGitHub: https://pygithub.readthedocs.io/en/latest/introduction.html

Scripts are in the directory [`bin`](bin):

* [`github-api-repo-issues-map-to-data`](bin/github-api-repo-issues-map-to-data)

* [`github-api-user-repos-map-to-url`](bin/github-api-repo-issues-map-to-url)


## Example

Example code that connects to GitHub and reads repo information:

```python
#!/usr/bin/env python3

import os
import sys
from pprint import pprint
import logging
import json
from github import Github

def github_personal_access_token():
    x = os.getenv("GITHUB_PERSONAL_ACCESS_TOKEN")
    if x is None:
        print("This script needs an environment variable GITHUB_PERSONAL_ACCESS_TOKEN.", file=sys.stderr)
        quit()
    return x

def github_client():
    return Github(github_personal_access_token())

GITHUB_CLIENT = github_client()

def main():
    github_owner_repo = sys.argv[1]
    user = GITHUB_CLIENT.get_user()
    pprint(user)
    repo = GITHUB_CLIENT.get_repo(github_owner_repo)
    pprint(repo)
    for label in repo.get_labels():
        print(f"{github_owner_repo} label:{label.name} color:{label.color}")

if __name__== "__main__":
    main()
```


## Setup


### Setup: GitHub personal access token

These scripts need to use your GitHub personal access token.

To generate your GitHub personal access token:
 
  * Go to your profile settings tokens page at <https://github.com/settings/tokens>.
  * Do "Generate new token".
  * Type in a descriptive name, such as "repo list".
  * Check the scope "repo" so you can use your token to access repositories.
  * Click Generate token.
  * You should see a new token such as "gpt_fe7f2bf4057de85eb638dc7356047b".
   
To export your GitHub personal access token:

Run:

```sh
export  GITHUB_PERSONAL_ACCESS_TOKEN="ghp_fe7f2bf4057de85eb638dc7356047b"
```

Verify:

```sh
echo $GITHUB_PERSONAL_ACCESS_TOKEN
```

You may want to add your personal access token to your user account startup files such as `.bashrc` or `.zshenv` or similar.


### Setup: Dependencies

These scripts use the Python library `PyGithub`.

You can install it any way you like, such as:

```sh
pip3 install --upgrade PyGithub
```


## Troubleshooting


### Troubleshooting: No module named pip3

If you get an error message such as:

```sh
No module named pip3
```

Then try to install pip3:

```sh
easy_install pip3
```


### Troubleshooting: No module named github

If you get an error message such as:

```sh
ModuleNotFoundError: No module named 'github'
```

Then try to install using a specific python instance:

```sh
python3 -m pip install --upgrade pip
python3 -m pip install PyGithub
```


### Troubleshooting: macOS, Python 3, pip3

If you get a pip3 error message such as:

```
Traceback (most recent call last):
File "/Library/Developer/CommandLineTools/usr/bin/pip3", line 10, in <module>
sys.exit(main())
TypeError: 'module' object is not callable
```

Then try to uninstall pip:

```sh
python3 -m pip install --upgrade pip
```


## Troubleshooting: macOS, Python 3, pip3, homebrew

If you are using macOS Catalina, python 3.7, and pip3, and
you get an error message that shows pip3 is aborting, then
see: https://github.com/Homebrew/homebrew-core/issues/44996

And try this:

```sh
rm -rf python3.7/site-packages/asn1crypto
pip3 install asn1crypto
```


## Troubleshooting: GitHub Enterprise

If you use Github Enterprise with custom hostname,
then you may need to adjust this script by doing:

```python
g = Github(base_url="https://{hostname}/api/v3", login_or_token="token")
```

