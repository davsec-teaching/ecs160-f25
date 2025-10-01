# ECS160-HW1 
## _(Start date: 10/10/2025)_
## _(Due date: TBD)_
## Problem 1: Getting Started with the GitHub API  

### Preliminaries
1. This assignment can be completed in either Java, C++, or Rust.
2. You're welcome to use any libraries or crates that will help you with your job, but for Java, you can't use any reflection or annotations-based API. Basically, if you find yourself doing something like applying `@[annotation_name` to a class or field, or ever specify `[Classname].class` as an argument to a method---you can't use it. 

### Learning objectives
1. Java basics: Encapsulation, Inheritance, File I/O, Exceptions.
2. Testing: JUnit, continuous integration via Github Actions (extra credit).
3. Tools and libraries: [for Java] Maven, adding dependencies to `pom.xml`, Gson for parsing JSON files, databases (NoSQL).

### Total points: 10

## Task Overview

We will use the **GitHub API** to gather data about popular repositories in different languages, analyze their activity, and store results in Redis. 
We will use the number of stars to determine the popularity of repositories. To determine how active each repository is, we will use how many times
the repository was forked, and how many _new_ commits were made to the forked repository. We will store these details in a way that is easily retrievable
in Redis. The future assignments will directly use the data stored in Redis, instead of making the GitHub API calls again. 


### Part A. Download Repository Data
1. **Fetch data with the GitHub API**  
   - Download the list of the top 10 most popular repositories in each of these languages:  
     - Java  
     - C / C++  
     - Rust  
   - Also download the list of all forks for each repository, and the recent commits to these forked repositories.
   - Helpful URLs to get started
        1. GitHub REST API - https://docs.github.com/en/rest?apiVersion=2022-11-28
        2. Search API - https://docs.github.com/en/rest/search/search?apiVersion=2022-11-28#search-repositories
        
2. **Parse the results**  
   - Use the JSON responses to create class/struct objects:  
     - **Repo**. Some suggested fields are---
       1. name
       2. ownerLogin
       3. htmlUrl
       4. forksCount
       5. language
       6. openIssuesCount
       7. List<Repo> forks; // List to hold forked repositories
       8. List<Commit> recentCommits; // List to hold commits
       9. List<Issue> issues; // List to hold issues
       10. commitCount; // To hold the number of commits

     - **Owner**
        1. login
        2. id
        3. htmlUrl
        4. site_admin
     - **Issue**
        1. title
        2. body
        3. state
        4. createdAt
        5. updatedAt
   - You're welcome to store more fields or fewer, as long as you can still compute the statistics mentioned below.


### Part B. Compute Statistics
For the top 10 repositories in each language, compute:  

1. **Popularity / Activity Metrics**
   - Total stars across the top 10 repos.  
   - Total forks across the top 10 repos.  
   - Last 50 (or fewer) commits:  
     - Which files were modified?  
     - Identify the Top-3 *most modified files*.  
   - Number of **new commits** in forked repos.  
   - Total open issues in the top 10 repos.  

2. **Display Results**  
   After your program runs, it should display:  
   - **Language:** [lang name]  
   - **Total stars:** [star count]  
   - **Total forks:** [fork count]  
   - **Most modified file per repo:**  
     - Repo name: [repo name]  
     - File name: [file name]  
   - **New commits in forked repos:** [count]  
   - **Open issues in top-10 repos:** [count]  


### Part C. Clone and Inspect Repositories
1. From the top-10 repositories, determine which ones actually contain **source code** (not just tutorials or documentation).  
2. Clone the **most popular repository** for each language.  
   - Verify that it’s a real codebase and not a tutorial repository. (Question: How should you do that?)


### Part D. Store Results
- Save the repository details and computed stats into **Redis**.  
- Use different logical keyspaces in Redis if needed for organization.




---

### Suggested Libraries for Java
- Gson for Json parsing (as long as you don't use the reflection or annotations-based approach)
- Apache HttpComponents for sending and receiving HTTP requests and responses
- JGit for cloning Git repos from Java

### Sample addition to `pom.xml` to add the Gson library

Please add this under the `<dependencies></dependencies>` block.
```
        <dependency>
            <groupId>com.google.code.gson</groupId>
            <artifactId>gson</artifactId>
            <version>2.11.0</version>
        </dependency>
```

