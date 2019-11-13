# CI: Jira comment with build information
[![Build Status](https://travis-ci.org/levonet/ansible-ci-jira-comment.svg?branch=master)](https://travis-ci.org/levonet/ansible-ci-jira-comment)

Add new or update last CI comment with build information to Jira task.

## Role Variables

- `ci_jira_api` (required): Jira API url.
- `ci_jira_username` (required): Jira username.
- `ci_jira_password` (required): Jira password.
- `ci_jira_github_branch` (required): Github branch name. Must include Jira task id. For example `TODO-44.feature`.
- `ci_jira_github_pr` (required): Github Pull Request number.
- `ci_jira_github_repository_url` (optional): Github repository url.
- `ci_jira_task_filter` (required): Regexp filter for Jira task from github branch. For example: `(TODO|BUGS)-\d+`.
- `ci_jira_message_body` (optional): Some text messages with CI information.
- `ci_jira_message_id` (optional): Unique text messages id. It needs if there should be more than one message in a Jira comment.
- `ci_jira_message_title` (optional): Default `Continuous Integration`

## Example Playbook

```yaml
- hosts: 127.0.0.1
  connection: local
  gather_facts: no
  vars:
    ci_jira_api: https://myorg.atlassian.net/rest/api/2
    ci_jira_username: ci-bot
    ci_jira_password: secret
    ci_jira_github_branch: "{{ github_branch }}"
    ci_jira_github_pr: "{{ github_pr_number }}"
    ci_jira_github_repository_url: https://github.com/myorg/myapp
    ci_jira_task_filter: (MYAPPAPI|MYAPPDB|BUGS)-\d+
    ci_jira_message_body: |
      * App: [pr-{{ ci_jira_github_pr }}.myapp.myorg.com|http://pr-{{ ci_jira_github_pr }}.myapp.myorg.com]
      * Logs: [myapp-PR-{{ ci_jira_github_pr }}|http://grafana.myorg.com/d/XxXxXx/logs?var-host=sandbox1&var-app=myapp-PR-{{ ci_jira_github_pr }}]
      * Jenkins: [PR-{{ ci_jira_github_pr }}|http://jenkins.myorg.com/job/myapp/view/change-requests/job/PR-{{ ci_jira_github_pr }}/]
  roles:
    - role: levonet.ci_jira_comment
```

And run in Jenkins:

```bash
ansible-playbook myplaybook.yml -e github_branch="${CHANGE_BRANCH}" -e github_pr_number="${CHANGE_ID}"
```

As a result, you will receive a comment in Jira task:

> ### Continuous Integration
> * Github: [PR-115](#)
> * App: [pr-115.myapp.myorg.com](#)
> * Logs: [myapp-PR-115](#)
> * Jenkins: [PR-115](#)

## License

[MIT](https://opensource.org/licenses/MIT)

## Author Information

This role was created by [Pavlo Bashynskyi](https://github.com/levonet)
