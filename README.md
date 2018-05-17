# Software process framework, v0.1

The document describes the process of developing, testing and shipping software.

It aims to achieve the goal of an efficiency of communication and collaboration between team members.

## Related tools

- [srelease](https://github.com/achempion/srelease) — bash script created to automate release management process


## Topics

1. git-flow
2. release history
3. commit format
4. task management
5. how to ship code to QA team
6. automated deployment

## Git-flow

<p align="center">
  <img src="https://raw.githubusercontent.com/achempion/software_process_framework/master/illustrations/git-flow.png" />
</p>

Gitflow is a tool to decrease entropy into your repository.
It contains information how to work with branches (develop, release, master),
how to ship features, how to implement hot fixes.

Useful links:
- http://nvie.com/posts/a-successful-git-branching-model/ — blog post
- https://github.com/petervanderdoes/gitflow-avh — repository
- https://github.com/petervanderdoes/gitflow-avh/wiki/Installation — how to install

Each branch should have a format that follows the naming rule:  
`<task number>-task_description_with_underline`

Feature and Hotfix branches should:
- start with `<task number>`  
- end with `-task_desctiption`

It's possible to use a date instead of `<task number>`.

**feature**  
`feature/task-11-sign_up_tests`  
`feature/2018-05-17-allow_user_to_edit_email`

**hotfix**  
`hotfix/task-11-fix_user_validation`  
`hotfix/2017-02-02-fix_user_validation`

## Release history

<p align="center">
  <img src="https://raw.githubusercontent.com/achempion/software_process_framework/master/illustrations/release-history.png" />
</p>

To track our process of development, keep what we do and what we did,  
we need to create `releases/` directory.

Here is the structure of `releases/` catalog:

```
└── releases/
    ├── current/
    │   ├── .keep
    │   ├── task-11
    │   ├── task-13
    │   └── task-15
    └── history/
        ├── .keep
        └── 2017-02-02
```

`.keep` need to keep an empty folder at the git repository

`task-11`, `task-13`, `task-15` is the identifications of a task at your task management system.

The content of the task file should describe additional actions needed to be done after deployment, such as:
- notify someone
- run some custom command on the server

Here is an example of task file:

**releases/current/task-11**
```ruby
# updates product rating by new formula
rake features:task_11
```

After, we can automatically concatenate all task files into the release file

**releases/history/2017-02-02**
```ruby
task-11
    # updates product rating by new formula
    rake features:task_11
```

`2017-02-02` in `releases/history/2017-02-02` is a date of release branch creation.  

You can also additionally specify the release pourpose by adding a postfix: `2017-02-02-backend_team`.

Here is a tool to automate work with `releases/` directory  
https://github.com/achempion/srelease

## Commit format

<p align="center">
  <img src="https://raw.githubusercontent.com/achempion/software_process_framework/master/illustrations/commit-format.png" />
</p>

Each commit should refers to task into your tast management software.

Here is an example:

`[refs #task-13] commit message` — if your want refer to one  
`[refs #task-11, #task-15] commit message` — if your want refer to more

## Task management

<p align="center">
  <img src="https://raw.githubusercontent.com/achempion/software_process_framework/master/illustrations/task-management.png" />
</p>

Task management is really on your own.

One thing that required is  
**each task should contain a link to the PR (pull request)**.

## How to ship code to QA team

<p align="center">
  <img src="https://raw.githubusercontent.com/achempion/software_process_framework/master/illustrations/ship-to-qa.png" />
</p>

Here is the most interesting part.

When your team develops, you have many pull requests to develop branch.  
Also, your QA needs to test each task.

In usual — you have a `testing` branch and all your team  
merging their *feature branches* to `testing` then deploy.

That approach has several disadvantages:  
- you can't simply "undeploy" branch from testing
- you need to manually merge each time
- you need to manually deploy each time
- you need to wait until deploy have been finished,
  run tasks if needed then notify QA

You need a tool to automate process of creating testing branches  
from labeled PRs.

## Automated deployment

<p align="center">
  <img src="https://raw.githubusercontent.com/achempion/software_process_framework/master/illustrations/automated-deployment.png" />
</p>

When testing branch builds you need to deploy.  
This process should be automated too.

The clue is that you can go even further and  
automate running tasks from `releases/current/*`.
