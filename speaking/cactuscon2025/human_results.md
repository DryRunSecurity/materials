We assessed commit `e1b245b78a0b8b36361f988a4a065a4ac5524f18`

# Notes for you/your team

## Behavior

* Misc
  - Credentials mentioned in the README for logins: "Login with the username `chris` and a password of `test123`"
  - Are notes tied to a specific user? Like, can only one user see it? Or is a group permission and that is more for attribution of WHO created the note?


* What does it do? (business purpose)
  - Task Manager -> Projects -> Tasks -> Notes
    -  Projects can have multiple users assigned

  - Web Application
* Who does it do this for? (internal / external customer base)
  - Appears to be both
* What kind of information will it hold?
  * PII - dob/ssn
* What are the different types of roles?
  - Auth Groups:
    - Admin
    - project managers
    - team members
  - User permissions
    * Actually called `user_permissions`
    * is_staff
    * is_superuser
* What aspects concern your client/customer/staff the most?

## Tech Stack

* Framework & Language 

Python - 3
Django - 3.2.21

* 3rd party components, Examples:
- [xlwt / django-excel](https://github.com/redpointsec/vtm/blob/9e3c64c331951c15f63c847be4f08a6811c3ca55/requirements.txt#L10-L11)
- django-health-check
- requests
- sqlparse


* Datastore
 - MySQL or SQLite


## Brainstorming / Risks

* We need to look csv injection due to excel/csv related libraries
* Look for where `requests` are used for SSRF issues
* django-health-check - Just need to look into it
* Using non-validating sqlparse library - look for validation issues. (ie - .format, .split)
* Sensitive data being handled
  - Auditing (storing this dob/ssn style stuff?)
  - Injection that would lead to leakage
  - Priv Esc
* It appears their is a potential for file handling
* I dont' know what [settings.AUTH_USER_MODEL is](https://github.com/redpointsec/vtm/blob/078c92d10285fbb717bb991308a85a302d0009e8/taskManager/migrations/0001_initial.py#L4)
* [MD5 password hashing?](https://github.com/redpointsec/vtm/blob/298fbbec58a256d5c26278bb4234496360fbf99b/taskManager/fixtures/users.json#L12)
* The permission checks for user seem to have different types of access - so it has Group permission, User permissions, and roles: is_staff / is_superuser
* File handling is happening, appears to be local on the filesystem, and that brings in a new set of concerns
