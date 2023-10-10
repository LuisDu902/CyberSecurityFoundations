# Trabalho realizado na Semana #4

## 2.1 Task 1: Manipulating Environment Variables

In this task, we're using Bash in a seed account to learn about managing environment variables. Environment variables store information for Unix-based system processes. The primary goal is to **understand how to manipulate these variables** with Bash commands.

To begin, we can use commands like **printenv** or **env** to view all current environment variables. Additionally, we can display specific ones, such as the current working directory (PWD), using commands like **printenv PWD**.
<table>
  <tr>
    <td><img src="screenshots/2.1__1_.png" alt="Image 2.1_1"></td>
    <td><img src="screenshots/2.1__2_.png" alt="Image 2.1_2"></td>
  </tr>
</table>

The second part of this task involves using the export and unset commands to modify environment variables.
- **export** is used to set or create new environment variables.
- **unset** is used to remove or unset an environment variable, effectively deleting it from the environment.

<img src="screenshots/2.1__3_.jpg" alt="Image 2.1_3">


## 2.2 Task 2: Passing Environment Variables from Parent Process to Child Process

In this task, we investigate how child processes inherit environment variables from their parent in Unix operating systems (using fork() system call). 

When fork() is invoked in Unix, it creates a new process by duplicating the parent process. The child process is nearly identical to the parent, but not all attributes are inherited.

The task aims to determine whether environment variables of the parent process are inherited by the child process when fork() is used.

<img src="screenshots/2.2__1_.png" alt="Image 2.2_1">

When we use the diff command in Step 3 to compare the two output files, it reveals no differences because both files should be identical, illustrating that the child process inherits the parent's environment variables.


## 2.3 Task 3: Environment Variables and execve()

In this task, we're investigating how environment variables behave when a new program is executed using the **execve()** function.

By doing this, we aim to understand if the environment variables from the calling process are inherited by the new program.

<img src="screenshots/2.3__1_.png" alt="Image 2.3_1">

In the first step, when execve() was called with the environ argument set to NULL, the new program did not inherit environment variables from the calling process, indicating that environment variables are not automatically inherited by default. 

However, in the second step, when **execve()** was called with the environ argument **explicitly set** to the environ variable, the new program did inherit environment variables.


## 2.4 Task 4: Environment Variables and system()

In this task, we're investigating how environment 


## 2.5 Task 5: Environment Variable and Set-UID Programs

In this task, we're investigating how environment 


## 2.6 Task 6: The PATH Environment Variable and Set-UID Programs

In this task, we're investigating how environment 


## 2.7 Task 7: The LD PRELOAD Environment Variable and Set-UID Programs

In this task, we're investigating how environment 


## 2.8 Task 8: Invoking External Programs Using system() versus execve()

In this task, we're investigating how environment 


## 2.9 Task 9: Capability Leaking

In this task, we're investigating how environment 
