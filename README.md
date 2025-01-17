# Update December 14, 2022

![Image](https://media.discordapp.net/attachments/1052243530627686410/1052571173407445142/gpt.png)
**THE NEW CHATGPT API**

Since they introduced Cloudflare on December 12. I have been trying to reverse engineer their new method. By first bypassing their Cloudflare page. When I was done, they Introduced Google reCAPTCHA when logging in, this made stuff a bit more complicated. So many things could go wrong in a 12 step process.

I finally did it. It’s all done. I was unsure about publishing the code though. Because if they could patch the first one, they can definitely patch this new one. And I’d like to tell you. THEY ARE WATCHING.


So I found a way to let you guys access their API without the overhead of Installing, Running code. We have an API now, You just make a GET request to the endpoint and It’ll return the ChatGPT response. 


Behind the scenes: 
- I solve the captcha for you
- I bypass Cloudflare
- I manage accounts
- I rotate Cloudflare keys/ Access Tokens
- I manage code dependencies, and make sure the code is up-to-date, and have the ability to fix the code in case they change their code 

While you: 
- Just make a GET request to an endpoint

I plan on allowing the same level of customization as PyChatGPT.   I’m running all of this on AWS, On money from my pocket. And the total cost is near $80 for a month. While we’re in a waitlist based model, I’ll maintain all the costs myself.  

##### Go ahead and write “I want access” on #waitlist (First **2-8 ** people will be selected for the initial tryout)
[Waitlist on Discord](https://discord.gg/Z2fpFKPE4j)


# 🔥 PyChatGPT
[Read More - How OpenAI filters requests made by bots/scrapers](https://github.com/rawandahmad698/PyChatGPT/discussions/103)

[![Python](https://img.shields.io/badge/python-3.8-blue.svg)](https://img.shields.io/badge/python-3.8-blue.svg)
[![PyPi](https://img.shields.io/pypi/v/chatgptpy.svg)](https://pypi.python.org/pypi/chatgptpy)
[![PyPi](https://img.shields.io/pypi/dm/chatgptpy.svg)](https://pypi.python.org/pypi/chatgptpy)


*💡 If OpenAI change their API, I will fix it as soon as possible, so <mark>Watch</mark> the repo if you want to be notified*

### Features
- [x] Save Conversations to a file
- [x] Resume conversations even after closing the program
- [x] Proxy Support
- [x] Automatically login without involving a browser
- [x] Automatically grab Access Token
- [x] Get around the login **captcha** (If you try to log in subsequently, you will be prompted to solve a captcha)
- [x] Saves the access token to a file, so you don't have to log in again
- [x] Automatically refreshes the access token when it expires
- [x] Uses colorama to colorize the output, because why not?
- [x] Smart Conversation Tracking 

## Web Demo
Integrated into [Huggingface Spaces 🤗](https://huggingface.co/spaces) using [Gradio](https://github.com/gradio-app/gradio). Try out the Web Demo

[![Hugging Face Spaces](https://img.shields.io/badge/%F0%9F%A4%97%20Hugging%20Face-Spaces-blue)](https://huggingface.co/spaces/yizhangliu/chatGPT)

<p align="center">Chatting</p>

![Screenshot 1](https://media.discordapp.net/attachments/1038565125482881027/1049255804366237736/image.png)

[//]: # (Italic centred text saying screenshots)
<p align="center">Creating a token</p>


## Install
```
pip install chatgptpy --upgrade
```

## Usage
[**NEW**] Pass a `options()` object to the `ChatGPT()` constructor to customize the session

[**NEW**] You can now save your conversations to a file

```python
from pychatgpt import Chat, Options

options = Options()

# [New] Pass Moderation. https://github.com/rawandahmad698/PyChatGPT/discussions/103
# options.pass_moderation = False

# [New] Enable, Disable logs
options.log = True

# Track conversation
options.track = True 

# Use a proxy
options.proxies = 'http://localhost:8080'

# Optionally, you can pass a file path to save the conversation
# They're created if they don't exist

# options.chat_log = "chat_log.txt"
# options.id_log = "id_log.txt"

# Create a Chat object
chat = Chat(email="email", password="password", options=options)
answer = chat.ask("How are you?")
print(answer)
```

[**NEW**] Resume a conversation
```python
from pychatgpt import Chat

# Create a Chat object
chat = Chat(email="email", password="password", 
            conversation_id="Parent Conversation ID", 
            previous_convo_id="Previous Conversation ID")

answer, parent_conversation_id, conversation_id = chat.ask("How are you?")

print(answer)

# Or change the conversation id later
answer, _, _ = chat.ask("How are you?", 
                        previous_convo_id="Parent Conversation ID",
                        conversation_id="Previous Conversation ID")
print(answer)

```
Start a CLI Session
```python
from pychatgpt import Chat

chat = Chat(email="email", password="password")
chat.cli_chat()
```

Ask a one time question
```python
from pychatgpt import Chat

# Initializing the chat class will automatically log you in, check access_tokens
chat = Chat(email="email", password="password") 
answer, parent_conversation_id, conversation_id = chat.ask("Hello!")
```

#### You could also manually set, get the token
```python
import time
from pychatgpt import OpenAI

# Manually set the token
OpenAI.Auth(email_address="email", password="password").save_access_token(access_token="", expiry=time.time() + 3600)

# Get the token, expiry
access_token, expiry = OpenAI.get_access_token()

# Check if the token is valid
is_expired = OpenAI.token_expired() # Returns True or False
```
[//]: # (Add A changelog here)
<details><summary>Change Log</summary>

#### Update using `pip install chatgptpy --upgrade`

#### 1.0.8
- Fixes an issue when reading from id_log.txt
- Introduces a new `pass_moderation` parameter to the `options()` class, defaults to `False`
- Adds proxies to moderation.
- If `pass_moderation` is True, the function is invoked in another thread, so it doesn't block the main thread.

#### 1.0.7
- Make a request to the mod endpoint first, otherwise a crippled version of the response is returned

#### 1.0.6
- New option to turn off logs. 
- Better Error handling.
- Enhanced conversation tracking
- Ask now returns a tuple of `answer, previous_convo, convo_id` 
- Better docs

#### 1.0.5
- Pull requests/minor fixes

#### 1.0.4
- Fixes for part 8 of token authentication

#### 1.0.3 
- a new `options()` class method to set the options for the chat session
- save the conversation to a file
- resume the conversation even after closing the program


#### 1.0.2
- ChatGPT API switches from `action=next` to `action=variant`, frequently. This library is now using `action=variant` instead of `action=next` to get the next response from the API.
- Sometimes when the server is overloaded, the API returns a `502 Bad Gateway` error.
- Added Error handling if the auth.json file is not found/corrupt

#### 1.0.0
- Initial Release via PyPi
</details>

### Other notes
If the token creation process is failing:
1. Try to use a proxy (I recommend using this always)
2. Don't try to log in too fast. At least wait 10 minutes if you're being rate limited.
3. If you're still having issues, try to use a VPN. On a VPN, the script should work fine.


### What's next?
I'm planning to add a few more features, such as:


- [x] A python module that can be imported and used in other projects
- [x] A way to save the conversation
- [ ] Better error handling
- [ ] Multi-user chatting

### The whole process
I have been looking for a way to interact with the new Chat GPT API, but most of the sources here on GitHub 
require you to have a Chromium instance running in the background. or by using the Web Inspector to grab Access Token manually.

No more. I have been able to reverse engineer the API and use a TLS client to mimic a real user, allowing the script to login without setting off any bot detection techniques by Auth0

Basically, the script logs in on your behalf, using a TLS client, then grabs the Access Token. It's pretty fast.

First, I'd like to tell you that "just making http" requests is not going to be enough, Auth0 is smart, each process is guarded by a 
`state` token, which is a JWT token. This token is used to prevent CSRF attacks, and it's also used to prevent bots from logging in.
If you look at the `auth.py` file, there are over nine functions, each one of them is responsible for a different task, and they all
work together to create a token for you. `allow-redirects` played a huge role in this, as it allowed to navigate through the login process



### Why did I do this?
No one has been able to do this, and I wanted to see if I could.

### Credits
- [OpenAI](https://openai.com/) for creating the ChatGPT API
- [FlorianREGAZ](https://github.com/FlorianREGAZ) for the TLS Client
