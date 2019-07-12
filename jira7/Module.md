Connects to JIRA from Ballerina. 

# Module Overview

The JIRA connector allows you to view, create, update, and delete projects, project categories, project components, user 
accounts, and issues through the JIRA REST API. It handles basic authentication.

**JIRA Project Operations**

The `wso2/jira` module contains operations to create new JIRA projects, update or delete existing projects, and get all 
the information using either the ID or key of the project. It also contains operations for adding or removing users and 
groups related to a project role, viewing user account details of the project lead, viewing assignable issue types for a 
given project, etc.

**JIRA Project Category Operations**

The `wso2/jira` module contains operations that get all available project categories, delete existing categories, and 
create new project categories.

**JIRA Project Component Operations**

The `wso2/jira` module contains operations that get all details of a given project component, delete existing 
components, create a new project component related to a specific project, etc.

**JIRA Issue Operations**

The `wso2/jira` module contains operations that get all the details of a given issue using the issue key, delete existing 
issues, create new issues, etc.

## Compatibility
|                    |    Version     |  
|:------------------:|:--------------:|
| Ballerina Language |    0.991.0     |
| JIRA REST API      |    7.2.2       |  

## Sample
First, import the `wso2/jira7` module into the Ballerina project.
```ballerina
import wso2/jira7;
```
**Obtaining Credentials to Run the Sample**

1. Visit [Atlassian](https://id.atlassian.com/signup) and create an Atlassian Account.
2. Obtain the following credentials and base URL.
    * Username
    * Password  

You can now enter the credentials in the HTTP client config.
```ballerina
jira7:JiraConfiguration jiraConfig = {
    baseUrl: config:getAsString("test_url"),
    clientConfig: {
        auth: {
            scheme: http:BASIC_AUTH,
            config: {
                username: config:getAsString("test_username"),
                password: config:getAsString("test_password")
            }
        }
    }
};

jira7:Client jiraClient = new(jiraConfig);
```

The `getAllProjectSummaries` remote function returns the project summary of all the projects if successful or an `error` if unsuccessful.
```ballerina
var response = jiraClient->getAllProjectSummaries();
if (response is jira7:ProjectSummary[]) {
    io:println("Project Summary: ", response);
} else {
    io:println("Error: ", response);
}
```

The `createProject` remote function creates a JIRA project with the given name. It returns a `Project` object if successful or an `error` if unsuccessful.
```ballerina
var output = jiraClient->createProject("TST_PROJECT");
if (output is jira7:Project) {
    io:println("Project Details: ", output);
} else {
    io:println("Error: ", output.message);
}
```

The `createIssue` remote function creates an issue with the given issue details. `IssueRequest` is an object that contains all
the data that is required to create the issue. It returns an `Issue` object if successful or an `error` if unsuccessful.
```ballerina
jira7:IssueRequest newIssue = {
    key: "TEST_ISSUE",
    summary: "This is a test issue created for the Ballerina JIRA Connector",
    issueTypeId: "10002",
    projectId: ”1234”,
    assigneeName: “username”
};
var issueResponse = jiraClient->createIssue(newIssue);
if (issueResponse is jira7:Issue) {
    io:println("Issue Details: ", issueResponse);
} else {
    io:println("Error: ", issueResponse.message);
}
```
