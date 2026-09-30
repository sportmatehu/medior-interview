# SportMate Medior Developer Interview Task

A good part of our backend work is about connecting our own product to other systems - for example payment providers, invoicing platforms, access-control systems, parking systems, and our mobile app.

This task is a small version of that kind of work. We are mainly interested in how you structure Laravel code, work with an external API, make practical choices, and explain the trade-offs you made.

## What we would like you to build

Build a small Laravel feature that syncs public GitHub repositories into a local database.

Please start from a fresh Laravel application using the official [Laravel Vue starter kit](https://laravel.com/docs/13.x/starter-kits#vue), which comes with Inertia.js and Vue already set up.

A user should be able to add a GitHub username or organization, start a sync, and then browse the repositories stored by the app. Please store the repository data locally instead of simply proxying GitHub responses.

> **About the scope.** There is intentionally more here than we expect most people to finish in eight hours. Please get the baseline working first, then spend the remaining time where you think it adds the most value. It is completely fine to leave some things unfinished - just tell us what you would do next and why.

## Baseline: please get these working

These are the parts we would like to see working when we review the task together.

### 1. Synchronization target

A user should be able to add a GitHub username or organization and save it locally with basic validation.

Store enough information to support the synchronization flow. Suggested fields include:

- Target name
- Target type, if users and organizations are handled differently
- Synchronization status
- Last successful synchronization time
- Most recent synchronization error, if applicable

### 2. GitHub integration boundary

Retrieve public repositories through a dedicated integration class, client, or service.

### 3. Local repository storage

Store a useful subset of the repository data locally. Suggested fields include:

- External repository ID
- Repository name and full name
- Description and repository URL
- Star count and open issue count
- Archived status
- External last-updated time

The sync should create new repositories, update ones we already know about, and prevent duplicates with an appropriate database constraint.

You do not need to fully solve what happens when a repository disappears from GitHub. A short note about how you would handle it is enough.

### 4. Queued synchronization

Run the synchronization through a Laravel queue job so the user is not waiting for GitHub to respond.

Keep the queue implementation simple. For the topics below, a few comments or short notes about how you would handle them in a real application are enough:

- Duplicate synchronization requests
- Job retries and failed jobs
- Timeouts
- Overlapping jobs

We will talk through these during the follow-up call, so they do not all need to be implemented.

### 5. Inertia.js and Vue UI

Provide a small interface where a user can:

- View synchronization targets
- Add a synchronization target
- Trigger synchronization
- View synchronized repositories
- View each target's synchronization status and most recent error, if one occurred
- Search, filter, or sort repositories in at least two useful ways

The repository list should display a useful subset of the stored information, such as name, description, language, star count, open issue count, and external last-updated time.

Do not spend much time on styling. A simple, clear interface is enough.

### 6. Basic automated testing

Please include a few completed unit and integration tests so we can see how you approach testing in Laravel. Browser-based end-to-end tests are not required.

As a baseline, cover:

- Creating a synchronization target
- Synchronizing repositories with a faked or mocked GitHub response
- Preventing or avoiding duplicate repository records

External HTTP calls should be faked or mocked.

You do not need to fully implement every useful test case. For the rest, feel free to add clearly named placeholder test methods so we can see what else you would cover.

Possible placeholder scenarios include API failures, pagination, job retries, scheduled synchronization, missing repositories, cache invalidation, and invalid GitHub targets.

### 7. AI usage documentation

Feel free to use AI tools such as Claude Code, Codex, OpenCode, Pi, or similar tools. Using AI is not a negative in this task.

Submit an `AI_USAGE.md` file containing:

- The tools used
- Important prompts or instructions
- Areas that were substantially AI-generated
- Examples of suggestions you changed or rejected
- How the generated code was validated

If you did not use AI, state that in the file. We will ask you about the code during the call, so please make sure you understand anything you submit - whether you wrote it yourself or generated it with AI.

## If you have time

Once the baseline works, pick whichever of these you think are worth spending the remaining time on. We do not expect you to complete all of them.

| Area                              | Possible scope                                                                                                                     |
|-----------------------------------|------------------------------------------------------------------------------------------------------------------------------------|
| Scheduled synchronization         | Add a scheduled command or job that synchronizes existing targets on a fixed interval chosen by you. Briefly explain the interval. |
| REST API                          | Expose target and repository data for a hypothetical mobile application, with consistent responses and appropriate status codes.   |
| Pagination                        | Handle pagination in the GitHub API and explain how the sync processes all repository pages.                                         |
| README full-text search            | Store repository README content and let users search repository READMEs by text.                                                     |
| Simple multitenancy                | Scope synchronization targets and repositories to users so each user sees only their own data.                                        |
| Reconciliation                    | Define or implement how repositories no longer returned by GitHub are treated.                                                     |
| Reliability and tests             | Improve error handling, logging, retry behavior, overlap protection, or automated test coverage.                                   |

Pick the things you think add the most value.

## Hand-in

Create a public Git repository containing your solution and send us the repository link when you are ready to hand it in.

GitHub, GitLab, or another publicly accessible Git hosting service is fine.

## Timebox

Please spend no more than eight hours in total on the task. If the eight hours are up and something is unfinished, stop there.

You have up to two weeks from receiving the task to hand in your solution. The two-week window is there so you can fit the task around your schedule; the implementation time should still stay within the eight-hour limit.

We are not looking for a production-ready system within those eight hours. We would rather see a smaller, clean solution than a rushed attempt to squeeze everything in.

Add a short note with:

- Approximate time spent
- Completed parts
- Incomplete parts
- What you would implement next
- Important compromises or shortcuts

Leaving something unfinished is fine. What matters is that the baseline works, the code is understandable, and you can talk us through the choices you made.

## A few technical expectations

### Third-party integration structure

Please keep responsibilities reasonably separated - for example controllers, synchronization logic, queue jobs, GitHub API communication, and mapping external data into local models should not all live in one place.

### Database design

Use Laravel migrations and include appropriate foreign keys and unique constraints. For the sake of the interview, you can use a local SQLite database to avoid a MySQL dependency.

Add a few short notes about which columns you would consider indexing and why.

### Error handling

Handle GitHub failures without showing raw exceptions to the user. Leave enough information behind that someone debugging the app can tell which sync failed and why.

## Follow-up call and discussion topics

During the follow-up call, we will go through the solution together. Think of it more as a code walkthrough and technical conversation than a quiz.

You do not need to implement every topic below. We may ask you to demonstrate the app, explain your choices, or describe how you would handle these topics in a real application:

- Synchronization flow, integration structure, database constraints, and index choices
- Queue behavior, including duplicate requests, overlapping jobs, retries, timeouts, and failed jobs
- GitHub rate limits and API pagination
- Transaction boundaries and cache invalidation
- API authentication and authorization
- Queue worker deployment and monitoring failed synchronizations
- Testing approach, including planned test cases
- Weaknesses, shortcuts, unfinished parts, and AI-generated code

We may also ask you to make one small change, debug a simple issue, or talk through a realistic failure scenario. We are interested in how you think and work with the code, not in framework trivia.
