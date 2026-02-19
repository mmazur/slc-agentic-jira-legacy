# Jira CLI Repository

This repository is for interacting with Jira using the `jira` CLI tool.

## Default Project

When creating new cards, default to the **AROSLSRE** project.

## Creating Issues

To create a new issue in the AROSLSRE project:

```bash
jira issue create -p AROSLSRE -t <type> -s "<summary>" -b "<description>"
```

### Common Issue Types
- `Bug` - For bug reports
- `Task` - For general tasks
- `Story` - For user stories
- `Epic` - For epics

### Example Commands

```bash
# Create a bug with high priority
jira issue create -p AROSLSRE -t Bug -s "Fix login issue" -y High -l bug -l urgent -b "Description of the bug"

# Create a task
jira issue create -p AROSLSRE -t Task -s "Update documentation" -b "Need to update the README file"

# Create a story with assignee
jira issue create -p AROSLSRE -t Story -s "User can reset password" -a "username" -b "As a user, I want to reset my password"
```

## Working with Epics

### Creating an Epic

Epics require a custom field called "Epic Name" in addition to the summary. **Important: The epic name should be the same as the summary.**

```bash
# Create an epic
jira issue create -p AROSLSRE -t Epic -s "Velero image updating" -b "Epic description" --custom "epic-name=Velero image updating"
```

### Linking Tasks to Epics

To add an existing task to an epic, use the `epic-link` custom field:

```bash
# Link a task to an epic
jira issue edit AROSLSRE-123 --custom "epic-link=AROSLSRE-456" --no-input
```

Alternatively, you can set the parent epic when creating a new task:

```bash
# Create a task under an epic
jira issue create -p AROSLSRE -P AROSLSRE-456 -t Task -s "Task summary" -b "Task description"
```

### Useful Flags
- `-t, --type` - Issue type (Bug, Task, Story, etc.)
- `-s, --summary` - Issue title/summary (required)
- `-b, --body` - Issue description
- `-y, --priority` - Priority (High, Medium, Low)
- `-a, --assignee` - Assign to user (username, email, or display name)
- `-l, --label` - Add labels (can be used multiple times)
- `-C, --component` - Add components
- `--web` - Open in browser after creation

## Getting Current User

To get the current user's email (useful for assigning issues to yourself):

'mmazur@redhat.com' is the current user.

This can be used with the `-a` flag.

## Moving Issues Between Statuses

To transition an issue to a different status:

```bash
# Move to In Progress
jira issue move AROSLSRE-123 "In Progress"

# Move to Done
jira issue move AROSLSRE-123 "Done"

# Move to any other status
jira issue move AROSLSRE-123 "Status Name"
```

Note: The `jira issue move` command does not support the `--no-input` flag.

## Working with Sprints

### View Current Sprint

```bash
# List all sprints that are mine
jira sprint list -p AROSLSRE --state active|grep SL

# View current/active sprint
jira sprint list -p AROSLSRE --state active|grep SL

The `grep SL` is because my team is Service Lifecycle (SL), to only sprints with SL in the name are the correct ones.
```

### Add Issue to Sprint

```bash
# Add issue to a specific sprint
jira sprint add SPRINT-ID AROSLSRE-123
```

## Other Useful Commands

```bash
# View issues
jira issue list -p AROSLSRE

# Open issue in browser
jira open <issue-key>
```
