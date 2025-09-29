# ECS160-HW2
## _(TBD)_
## Problem: Microservices and LLMs for Bug Detection

_Learning objectives:_ 
1. Java annotations
2. Java reflection
3. Microservices
4. Unit testing and Mock testing

_**(Total points: TBD)**_

## Objective
Validate whether an LLM can detect bugs in code by cross-referencing its findings with real GitHub issue reports

You will build a reflection-based dependency injection framework that can be used to interface with any LLM model.

You will also build a microservice-based system that:  
1. Uses LLMs to summarize bugs.  
2. Cross-checks LLM bug summaries with real issue reports.  

---

## Part A. Dependency Injection Framework

1. Build a dependency injection framework (must be in **Java**) using Java annotations and reflections:  
   - Accepts *Issue* type objects.  
   - Generates summaries for them, as shown in the issue Json format below.
   ```json
   {
     "bug_type": "NullPointerException",
     "line": 42,
     "description": "Possible null dereference when accessing 'user.profile'",
     "filename": "src/main/model/Record.java"
   }

2. Deployment:  
   - Package the framework as a JAR.  
   - Install it into your local Maven repository.  
   - Import it into your application via `pom.xml`.  


## Part B. Application (Must be in Java)
1. Pick the most-starred C codebase cloned in HW1 (mandatory to use **C**).  
2. Use your dependency injection framework to initialize the LLM model (LLAMA 3).  
3. For the 10 most recent open issues, ask the LLM model to summarize them.
4. Processing workflow:  
   - Load the file contents referenced in the LLM summary.  
   - Send the file to the LLM-based Bug Detection Microservice to generate a bug summary.  
   - Send both bug summaries to the LLM-based Bug Similarity Checker Microservice to compare results.
5. Output results in the format:  
   - Issue ID: [GitHub ID]  
   - Bug summary from issue description  
   - Bug summary from source code  
   - Equivalent: [True/False]  

## Part C. LLM-based Bug Detection Microservice
- Can be written in any language.  
- Must:  
  - Accept C code as input.  
  - Ask the LLAMA 3 model to detect bugs.  
  - Return bug summaries in a structured format.  

## Part D. LLM-based Bug Similarity Checker Microservice
- Can be written in any language.  
- Must:  
  - Accept two bug summaries as input.  
  - Compare them.  
  - Report whether they describe the same bug.
    
## Part E. Testing
- Provide automatic tests.  
- Use mock testing where possible (e.g., mock LLM responses).  


