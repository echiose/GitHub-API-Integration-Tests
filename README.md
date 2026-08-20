# GitHub API Back-End Tests

Automated integration tests for GitHub issue, label, and comment endpoints. The solution is written in C# with .NET 8, uses RestSharp for HTTP requests, and uses NUnit for test execution.

## What Is Covered

The test suite exercises:

- Listing issues in a repository
- Getting an issue by number
- Listing labels for an issue
- Listing comments for an issue
- Creating an issue
- Creating, reading, editing, and deleting an issue comment
- Data-driven issue and label checks

The write tests create an issue and comment in the configured repository, then edit and delete the created comment.

## Prerequisites

- .NET 8 SDK
- A GitHub account with access to the repository under test
- A GitHub personal access token with the permissions required to read and modify issues and comments
- Network access to `api.github.com`

## Project Structure

```text
RestSharpGitHubAPI/
├── RestSharpExercise.sln
├── RestSharpServices/
│   ├── Models/                 # Issue, comment, and label models
│   ├── RestSharpServices.cs    # GitHubApiClient implementation
│   └── RestSharpServices.csproj
└── TestGitHubApi/
	├── TestGitHubApi.cs        # NUnit integration tests
	└── TestGitHubApi.csproj
```

## Configuration

Before running the tests, update the `Setup` method in `TestGitHubApi/TestGitHubApi.cs` with:

- The GitHub API base URL
- The GitHub username
- A personal access token
- The repository name used by the tests

Do not commit tokens or other credentials. Prefer environment variables or a local user-secrets solution when adapting this project for shared or CI environments.

The current fixture targets the `testnakov/test-nakov-repo` repository and uses fixed issue numbers for read-only checks. Update those values if the repository contents differ.

## Restore, Build, and Test

Run these commands from the `RestSharpGitHubAPI` directory:

```powershell
dotnet restore RestSharpExercise.sln
dotnet build RestSharpExercise.sln --configuration Release
dotnet test RestSharpExercise.sln --configuration Release
```

To run only the NUnit test project:

```powershell
dotnet test TestGitHubApi/TestGitHubApi.csproj
```

## Important Test Behavior

- These are live integration tests, not isolated unit tests. They send requests to GitHub and can change repository data.
- Tests that create, edit, and delete comments depend on the ordered execution of earlier tests and shared in-memory IDs.
- The test account must have permission to create issues and manage comments in the target repository.
- GitHub API rate limits and repository state can affect results.

## Main Dependencies

- RestSharp 113.1.0
- NUnit 4.4.0
- NUnit3TestAdapter 6.1.0
- Microsoft.NET.Test.Sdk 18.0.1
- Selenium.WebDriver 4.40.0
