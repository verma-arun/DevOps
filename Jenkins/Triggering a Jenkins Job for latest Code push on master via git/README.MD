# Trigger a Jenkins Job when we push latest changes to our github/bitbucket repository
This read me will help you configure jenkins to automate a job to deploy latest changes when the code at github/bitbucket is updated.

## Requirements

1. Github/Bitbucket Jenkins WebHook

2. Jenkins Github Integration Plugin - This triggers after pull requests are merged in repository.

3. Jenkins GitHub Plugin - This triggers after some push request is created 

4. Jenkins BitBucket Plugin.

## Steps to configure with GitHub

1. First Install *Jenkins GitHub Integration Plugin* or use already available *GitHub Plugin*.

2. Then go to Repo of yor project and then *Settings tab* > *WebHooks* and create a new webhook.

3. Enter payload url as `http://jenkinsUrl/github-webhook` and choose content type as application/json.

4. Select events to send github post request to jenkins for selected events like for pull request created, push, commits.

5. Now save.

6. Now to go to Jenkins > create a new Job.

7. Then Tick Github Pull Requests and Select *Hooks with Persisted Data* as Trigger Mode. 

8. Or you can use and Tick *Github hook trigger for GITScm polling*.

9. Now save the job.

10. Now whenever you push changes, commits and more over github , the webhook will be firesd to send a HTTP POST request to Jenkins Server. The Request will then trigger the Job.


