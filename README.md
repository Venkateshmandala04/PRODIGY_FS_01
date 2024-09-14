# PRODIGY_FS_01
Here’s a simplified version of the README without the pictures for **Streamlit Authenticator**:

---

# Secure user Authenticator

**A secure authentication module to manage user access in a Streamlit application**

## Table of Contents
1. [Quickstart](#1-quickstart)
2. [Installation](#2-installation)
3. [Creating a Configuration File](#3-creating-a-configuration-file)
4. [Setup](#4-setup)
5. [Creating a Login Widget](#5-creating-a-login-widget)
6. [Authenticating Users](#6-authenticating-users)
7. [Creating a Reset Password Widget](#7-creating-a-reset-password-widget)
8. [Creating a New User Registration Widget](#8-creating-a-new-user-registration-widget)
9. [Creating a Forgot Password Widget](#9-creating-a-forgot-password-widget)
10. [Creating a Forgot Username Widget](#10-creating-a-forgot-username-widget)
11. [Creating an Update User Details Widget](#11-creating-an-update-user-details-widget)
12. [Updating the Configuration File](#12-updating-the-configuration-file)
13. [License](#license)

---

### 1. Quickstart

- Check out the [demo app](https://demo-app-v0-3-3.streamlit.app/).
- Refer to the [API reference](https://streamlit-authenticator.readthedocs.io/en/latest/).

---

### 2. Installation

Streamlit-Authenticator can be installed via PyPI:

```bash
pip install streamlit-authenticator
```

---

### 3. Creating a Configuration File

- Create a YAML configuration file to define user credentials (e.g., names, usernames, passwords).
- The file should also include cookie settings and a list of pre-authorized emails for registration.

Example configuration file (`config.yaml`):

```yaml
credentials:
  usernames:
    jsmith:
      email: jsmith@gmail.com
      name: John Smith
      password: abc # Will be hashed automatically
    rbriggs:
      email: rbriggs@gmail.com
      name: Rebecca Briggs
      password: def # Will be hashed automatically
cookie:
  expiry_days: 30
  key: some_signature_key
  name: some_cookie_name
pre-authorized:
  emails:
  - melsby@gmail.com
```

---

### 4. Setup

- Import the YAML configuration and create an authentication object.

```python
import yaml
import streamlit_authenticator as stauth
from yaml.loader import SafeLoader

with open('../config.yaml') as file:
    config = yaml.load(file, Loader=SafeLoader)

authenticator = stauth.Authenticate(
    config['credentials'],
    config['cookie']['name'],
    config['cookie']['key'],
    config['cookie']['expiry_days'],
    config['pre-authorized']
)
```

---

### 5. Creating a Login Widget

- To render the login widget:

```python
authenticator.login()
```

---

### 6. Authenticating Users

- To verify users and allow access to restricted content:

```python
if st.session_state['authentication_status']:
    authenticator.logout()
    st.write(f'Welcome *{st.session_state["name"]}*')
elif st.session_state['authentication_status'] is False:
    st.error('Incorrect username/password')
elif st.session_state['authentication_status'] is None:
    st.warning('Please enter your username and password')
```

---

### 7. Creating a Reset Password Widget

- Allow logged-in users to modify their password:

```python
if st.session_state['authentication_status']:
    if authenticator.reset_password(st.session_state['username']):
        st.success('Password modified successfully')
```

---

### 8. Creating a New User Registration Widget

- Allow users to register:

```python
try:
    email, username, name = authenticator.register_user(pre_authorization=False)
    if email:
        st.success('User registered successfully')
except Exception as e:
    st.error(e)
```

---

### 9. Creating a Forgot Password Widget

- Allow users to generate a new password:

```python
try:
    username, email, new_password = authenticator.forgot_password()
    if username:
        st.success('New password generated')
except Exception as e:
    st.error(e)
```

---

### 10. Creating a Forgot Username Widget

- Allow users to retrieve forgotten usernames:

```python
try:
    username, email = authenticator.forgot_username()
    if username:
        st.success('Username retrieved')
except Exception as e:
    st.error(e)
```

---

### 11. Creating an Update User Details Widget

- Allow users to update their name or email:

```python
if st.session_state['authentication_status']:
    if authenticator.update_user_details(st.session_state['username']):
        st.success('Details updated successfully')
```

---

### 12. Updating the Configuration File

- Always update the configuration file after resetting passwords, registering users, or changing user details.

---

### License

This project is licensed under the terms of the MIT license.

---

This simplified version includes the essential details for setting up and using Streamlit Authenticator without images and extra explanations.
