
push event:
```json
{

"object_kind": "push",

"event_name": "push",

"before": "582ed706d5c392bc8406b53af22df691415ab873",

"after": "fb24674b4661afc4a1fdb4cc3e62d8af5e873894",

"ref": "refs/heads/main",

"ref_protected": true,

"checkout_sha": "fb24674b4661afc4a1fdb4cc3e62d8af5e873894",

"message": null,

"user_id": 22064404,

"user_name": "PAI PENG",

"user_username": "ppeng24",

"user_email": null,

"user_avatar": "https://secure.gravatar.com/avatar/7369fd66f4a56641f07ce6c13e78726e8f37fb5ba89fd4ce2a3f23767960cfe4?s=80&d=identicon",

"project_id": 60287318,

"project": {

"id": 60287318,

"name": "code-reviewer-final",

"description": null,

"web_url": "https://gitlab.com/isv5994829/code-reviewer-final",

"avatar_url": null,

"git_ssh_url": "git@gitlab.com:isv5994829/code-reviewer-final.git",

"git_http_url": "https://gitlab.com/isv5994829/code-reviewer-final.git",

"namespace": "ISV",

"visibility_level": 0,

"path_with_namespace": "isv5994829/code-reviewer-final",

"default_branch": "main",

"ci_config_path": "",

"homepage": "https://gitlab.com/isv5994829/code-reviewer-final",

"url": "git@gitlab.com:isv5994829/code-reviewer-final.git",

"ssh_url": "git@gitlab.com:isv5994829/code-reviewer-final.git",

"http_url": "https://gitlab.com/isv5994829/code-reviewer-final.git"

},

"commits": [

{

"id": "fb24674b4661afc4a1fdb4cc3e62d8af5e873894",

"message": "Merge branch 'feat/webhooks' into 'main'\n\nRemoved the production environment file, updated configuration settings, and...\n\nSee merge request isv5994829/code-reviewer-final!1",

"title": "Merge branch 'feat/webhooks' into 'main'",

"timestamp": "2024-08-16T07:42:26+00:00",

"url": "https://gitlab.com/isv5994829/code-reviewer-final/-/commit/fb24674b4661afc4a1fdb4cc3e62d8af5e873894",

"author": {

"name": "PAI PENG",

"email": "ppeng24@wisc.edu"

},

"added": [

".gitignore",

"TODO",

"__init__.py",

"__pycache__/config.cpython-311.pyc",

"__pycache__/config.cpython-312.pyc",

"app/__init__.py",

"app/__pycache__/__init__.cpython-311.pyc",

"app/__pycache__/__init__.cpython-312.pyc",

"app/__pycache__/main.cpython-311.pyc",

"app/__pycache__/main.cpython-312.pyc",

"app/controller/__pycache__/webhook_controller.cpython-312.pyc",

"app/controller/webhook_controller.py",

"app/main.py",

"app/routes/__pycache__/webhooks.cpython-312.pyc",

"app/routes/example_hook/comment_event.json",

"app/routes/webhooks.py",

"app/service/__init__.py",

"app/service/__pycache__/__init__.cpython-312.pyc",

"app/service/agents/__init__.py",

"app/service/agents/base.py",

"app/service/agents/discussion_agent.py",

"app/service/agents/feedback_agent.py",

"app/service/agents/merge_agent.py",

"app/service/agents/push_agent.py",

"app/service/agents/test_agent.py",

"app/service/diffcode/__pycache__/diffcode.cpython-312.pyc",

"app/service/diffcode/base.py",

"app/service/diffcode/diffcode.py",

"app/service/diffcode/test_diff_output.txt",

"app/service/diffcode/test_diffcode.py",

"app/service/gitlab_polling.py",

"app/service/model_setup/README.md",

"app/service/model_setup/__init__.py",

"app/service/model_setup/__pycache__/__init__.cpython-312.pyc",

"app/service/model_setup/__pycache__/base.cpython-312.pyc",

"app/service/model_setup/__pycache__/model_setup.cpython-311.pyc",

"app/service/model_setup/__pycache__/model_setup.cpython-312.pyc",

"app/service/model_setup/base.py",

"app/service/model_setup/model_setup.py",

"app/service/model_setup/model_setup_test.py",

"config.py",

"requirements.txt"

],

"modified": [],

"removed": []

},

{

"id": "3c7cde1081d6f2f0fb334ca6fb4734d15014a59f",

"message": "none\n",

"title": "none",

"timestamp": "2024-07-25T14:48:12+08:00",

"url": "https://gitlab.com/isv5994829/code-reviewer-final/-/commit/3c7cde1081d6f2f0fb334ca6fb4734d15014a59f",

"author": {

"name": "Pai",

"email": "ppeng24@wisc.edu"

},

"added": [

"app/__pycache__/__init__.cpython-311.pyc",

"app/__pycache__/main.cpython-311.pyc",

"app/__pycache__/main.cpython-312.pyc",

"app/controller/__pycache__/webhook_controller.cpython-312.pyc",

"app/routes/__pycache__/webhooks.cpython-312.pyc",

"app/routes/example_hook/comment_event.json"

],

"modified": [

"TODO",

"__pycache__/config.cpython-312.pyc",

"app/controller/webhook_controller.py",

"app/routes/webhooks.py",

"app/service/diffcode/__pycache__/diffcode.cpython-312.pyc",

"app/service/diffcode/diffcode.py",

"app/service/diffcode/test_diffcode.py",

"app/service/gitlab_polling.py",

"app/service/model_setup/model_setup_test.py"

],

"removed": []

},

{

"id": "582ed706d5c392bc8406b53af22df691415ab873",

"message": "Removed the secret token from the .env file and updated related code in config and webhooks files.\n",

"title": "Removed the secret token from the .env file and updated related code in config and webhooks files.",

"timestamp": "2024-07-25T14:19:34+08:00",

"url": "https://gitlab.com/isv5994829/code-reviewer-final/-/commit/582ed706d5c392bc8406b53af22df691415ab873",

"author": {

"name": "Pai (aider)",

"email": "ppeng24@wisc.edu"

},

"added": [],

"modified": [

"app/routes/webhooks.py",

"config.py"

],

"removed": []

}

],

"total_commits_count": 3,

"push_options": {},

"repository": {

"name": "code-reviewer-final",

"url": "git@gitlab.com:isv5994829/code-reviewer-final.git",

"description": null,

"homepage": "https://gitlab.com/isv5994829/code-reviewer-final",

"git_http_url": "https://gitlab.com/isv5994829/code-reviewer-final.git",

"git_ssh_url": "git@gitlab.com:isv5994829/code-reviewer-final.git",

"visibility_level": 0

}
}
```


