# ECS160-HW2
## Start Date: 10/24/2025; Due Date: 11/24/2025

## Problem: LLMs for Bug Detection

_Learning objectives:_ 
1. Java annotations
2. Java reflection

_**(Total points: 25 (+5 extra credit for Lazy Loading)**_

### Objective
Validate whether an LLM can detect bugs in code by cross-referencing its findings with real GitHub issue reports. You will build reflection-based microservice and Redis persistence frameworks to answer the above question.

The persistence framework will be used load Repos and their associated Issues from the Redis database. The microservice framework will be used to develop three microservices---(i) an LLM-based _Issue Summarizer_, (ii) an LLM-based _Bug Finder_, and (iii) an LLM-based _Issue Summary Comparator_.

These components are described below.

---

### Part 0. Updating HW1 if necessary
1. Make sure your Redis database has atleast the `Repo` and `Issue` objects stored in them.
2. A `Repo` record should have a comma-separated value that concatenates all the `Issue` ids. 
   Something like this: 
   ```
   ----------------------------------------------------------------------------------------------------------------------------------------------------
   |repo-101 || Url | https://github.com/octocat/ | CreatedAt | 2020-11-11 | Author Name | Person A | Issues | iss-101,iss-102,iss-110,iss-112,iss-109 |
   ----------------------------------------------------------------------------------------------------------------------------------------------------
   ```
   Then, the Issue itself should also be stored in redis (potentially in a different logical database)
   ```
   -----------------------------------------------------------
   |iss-101 || Date | 2025-12-12 | Description | Page crashes|
   ----------------------------------------------------------
   ```
3. **Important:** Make sure that the Repo records at least have `Url` and `Issues` fields and Issue records have a `Description` field.
4. Rerun Hw1 to make sure the entries are persisted in this format.
5. **You should not make any GitHub API calls in this HW.**

   
### Part A. Redis Persistence Framework 
1. The framework must be implemented as an independent Java library.
2. The library should provide the following annotations
   - `@PersistableObject`: a class-level annotation that indicates that the class can be saved in Redis.
   - `@PersistableField`: a field-level annotation that indicates that the field can be saved in Redis. Only the fields annotated with this annotation must be persisted.
   - `@Id`: a field-level annotation indicating which field is the Id field.
   - [EC] `@LazyLoad(field="[FIELDNAME])`: a method-level annotation with a single argument that specifies which field is lazy loaded.
3. The library should contain a class `RedisDB`, with methods `bool persist(Object o)` and `Object load(Object o)`. 
   - The `persist` method should persist all fields annotated with the `@Persistable` annotation in the database.
   - The `load` method should accept an Object `o` with a field annotated with `@Id` populated, and load that object from the Redis databse
   - [EC] For the `load` method, if a field is marked as lazy-loaded, it should be lazy loaded.
4. Build and install the persistence framework to your local Maven repository using the correct `mvn` command.

### Part B. Microservice Framework

1. The library must be implemented as an independent Java library.
2. The library must provide the following annotation
   - `@Microservice`: a class-level annotation that indicates that the class defines a microservice.
   - `@Endpoint(url = "[url]`: a method level annotation that indicates the method that is the entry point of a particular microservice url.
     - The method signature should be `String handleRequest(String input)`, where it accepts `input` as the argument, processes it and returns a `String`.
3. The library must support a method `bool launch(int port)` that launches the annotated microservices to listen at the specified port.
   - This function should run an infinite loop, inspecting each incoming request, mapping it to the right microservice and invoking it, collecting the response, and sending it back to the client.
4. Build and install the microservice framework to your local Maven repository using the correct `mvn` command.

### Part C. Using the Microservice Framework

1. Design ONE Java projects and import the Microservice framework in it. This will implement the three microservices mentioned earlier, using the Microservice framework designed in Part B.
2. Microservice A (_Issue Summarizer_): Provides an `@Endpoint(url = "summarize_issue")` that accepts a JSON of a Github issue (see Part D) and returns an Issue summary in the JSON format specified below.
3. Microservice B (_Bug Finder_): Provides an `@Endpoint(url = "find_bugs")` that accepts C file contents and returns a list of Json Issues.
4. Microservice C (_Issue Summary Comparator_): Provides an `@Endpoint(url = "check_equivalence")` that accepts a list of two lists of Issues in Json format and returns a Json list of Issues that are common in both.
5. You must use the Ollama framework to interface with the LLM models. Feel free to use any Java library that can interface with Ollama.
6. You must use Ollama's deepcoder:1.5b model
7. Feel free to support only GET requests.
8. Feel free to use the Apache HTTP Core library or any other library. You should figure out how to set up the server and how to have it handle the different endpoint urls.
9. The Issue format in Json: 
   ```json
   {
     "bug_type": "NullPointerException",
     "line": 42,
     "description": "Possible null dereference when accessing 'user.profile'",
     "filename": "src/main/model/Record.java"
   }
10. Launch the three microservices. 


### Part D. Java Application
1. Among the repos stored in Redis, pick a C/C++ repo that you wish to analyze.
2. Create a `selected_repo.dat` file and store the `repoId` of that repository in it. Also, manually enter a list of 3-4 files in the repository you wish to analyze.
1. Load that repo and its corresponding issues from the Redis database that you created in HW1, using the Part A persistence framework. 
2. Clone the repository (feel free to use the `git` binary, via the `ProcessBuilder`, or any other library).
3. For the issues in that repo, invoke Microservice A to summarize them. Let's call this `IssueList1`.
4. Load the `.c` files, specified in `selected_repo.dat`, in that repo and invoke Microservice B to get the list of bugs in it. Let's call this `IssueList2`.
5. Send both `IssueList1` and `IssueList2` to Microservice C and print the set of issues that it says are common - that the LLM identified it from the source code AND it was mentioned in a GitHub Issue.
6. In `README.md` discuss the results.

### Part E. Testing
1. Provide automatic tests.  
2. Use mock testing where possible (e.g., mock LLM responses).

### Important Points
1. You **cannot** use any existing Java library that provides annotation-based persistence.
2. You **cannot** use any existing Java library that provides annotation-based microservices.
3. You can use annotation-based libraries for any other purposes.
4. If in doubt, please post on Piazza to check if the library you want to use is allowed or not.

### Skeleton code and script
1. HW2 skeleton code is [here](https://github.com/davsec-teaching/f25-hw2-skeleton-code).
2. All submissions **must** follow the top-level hierarchy of organizing the different components.
3. Note that only a handful of classes and annotations have been created in the skeleton. Please feel free to add more classes as needed.
4. Do **NOT** fork the repo. Instead clone it and then update the remote origin.
5. Running `./script.sh` should compile and run all the projects correctly.

### Sample code
1. Redis-based logging framework using Java annotations, reflections, and dynamic proxy is [here](https://github.com/davsec-teaching/proxy-logging-full-demo). This is basically an expanded version of the logging example we worked through in class.
2. Code to load all classes [here](https://gist.github.com/taptipalit/fef1576d9740344ac7e1d3460b25c52a)

### Grading Rubric
1. Persistence Framework - 7 points (+5 points EC)
2. Microservice Framework - 7 points
3. Microservices - 5 points
4. Main app - 4 points
5. Unit tests - 2 points

### Submission
1. Run `mvn clean` and delete the cloned repository
2. Zip the homework directory.
1. Make sure to include the redis `.db` file.
2. Make sure to include the `selected_repo.dat` file.
3. Submit on Canvas before the deadline.
