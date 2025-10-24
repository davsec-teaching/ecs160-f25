# ECS160-HW2
## Start Date: 10/24/2025; Due Date: 11/21/2025

## Problem: LLMs for Bug Detection

_Learning objectives:_ 
1. Java annotations
2. Java reflection

_**(Total points: 25 (+5 extra credit for Lazy Loading)**_

## Objective
Validate whether an LLM can detect bugs in code by cross-referencing its findings with real GitHub issue reports. You will build reflection-based microservice and Redis persistence frameworks to answer the above question.

The persistence framework will be used load Repos and their associated Issues from the Redis database. The microservice framework will be used to develop three microservices---(i) an LLM-based _Issue Summarizer_, (ii) an LLM-based _Bug Finder_, and (iii) an LLM-based _Issue Summary Comparator_.

These components are described below.

---

## Part A. Redis Persistence Framework
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

## Part B. Microservice Framework

1. The library must be implemented as an independent Java library.
2. The library must provide the following annotation
   - `@Microservice`: a class-level annotation that indicates that the class defines a microservice.
   - `@Endpoint(url = "[url]`: a method level annotation that indicates the method that is the entry point of a particular microservice url.
     - The method signature should be `String handleRequest(String input)`, where it accepts `input` as the argument, processes it and returns a `String`.
3. The library must support a method `bool launch(int port)` that launches the annotated microservices to listen at the specified port.
   - This function should run an infinite loop, inspecting each incoming request, mapping it to the right microservice and invoking it, collecting the response, and sending it back to the client.

## Part C. Using the Microservice Framework

1. Design THREE Java projects and import the Microservice framework in each of them. These three projects will implement the three microservices mentioned earlier, using the Microservice framework designed in Part B.
2. Microservice A: Provides an `@Endpoint(url = "summarize_issue")` that accepts a JSON of a Github issue (see Part D) and returns an Issue summary in the JSON format specified below.
3. Microservice B: Provides an `@Endpoint(url = "find_bugs")` that accepts C file contents and returns a list of Json Issues.
4. Microservice C: Provides an `@Endpoint(url = "check_equivalence")` that accepts a list of two lists of Issues in Json format and returns a Json list of Issues that are common in both.
5. Please use the Ollama framework to interface with the LLM models.
6. Please use Ollama's deepcoder:1.5b model
7. Feel free to support only GET requests
8. Feel free to use the Apache HTTP Core library.
9. The Issue format in Json: 
   ```json
   {
     "bug_type": "NullPointerException",
     "line": 42,
     "description": "Possible null dereference when accessing 'user.profile'",
     "filename": "src/main/model/Record.java"
   }
10. Launch the three microservices. 


## Part D. Java Application 
1. Load the repos and issues from the Redis database that you created in HW1, using the Part A persistence framework. 
2. Find the C repo with most issues, which is not the Linux kernel, and clone it (feel free to use the `git` binary, via the `ProcessBuilder`, or any other library).
3. Load the issues in that repo, invoke Microservice A to summarize them. Let's call this `IssueList1`.
4. Load each `.c` file in that repo and invoke Microservice B to get the list of bugs in it. Let's call this `IssueList2`.
5. Send both `IssueList1` and `IssueList2` to Microservice C and print the set of issues that it says are common - that the LLM identified it from the source code AND it was mentioned in a GitHub Issue.
6. In `README.md` discuss the results.

## Part E. Testing
- Provide automatic tests.  
- Use mock testing where possible (e.g., mock LLM responses).  


