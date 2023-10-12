# Trabalho realizado na Semana #4

## 2.1 Task 1: Manipulating Environment Variables

In this task, we're using Bash in a seed account to learn about managing environment variables. Environment variables store information for Unix-based system processes. The primary goal is to **understand how to manipulate these variables** with Bash commands.

To begin, we can use commands like **printenv** or **env** to view all current environment variables. Additionally, we can display specific ones, such as the current working directory (PWD), using commands like **printenv PWD**.
<table>
  <tr>
    <td><img src="../screenshots/logbook4/2.1__1_.png" alt="Image 2.1_1"></td>
    <td><img src="../screenshots/logbook4/2.1__2_.png" alt="Image 2.1_2"></td>
  </tr>
</table>

The second part of this task involves using the export and unset commands to modify environment variables.
- **export** is used to set or create new environment variables.
- **unset** is used to remove or unset an environment variable, effectively deleting it from the environment.

<img src="../screenshots/logbook4/2.1__3_.jpg" alt="Image 2.1_3">


## 2.2 Task 2: Passing Environment Variables from Parent Process to Child Process

In this task, we investigate how child processes inherit environment variables from their parent in Unix operating systems (using fork() system call). 

When fork() is invoked in Unix, it creates a new process by duplicating the parent process. The child process is nearly identical to the parent, but not all attributes are inherited.

The task aims to determine whether environment variables of the parent process are inherited by the child process when fork() is used.

<img src="../screenshots/logbook4/2.2__1_.png" alt="Image 2.2_1">

When we use the diff command in Step 3 to compare the two output files, it reveals no differences because both files should be identical, illustrating that the child process inherits the parent's environment variables.


## 2.3 Task 3: Environment Variables and execve()

In this task, we're investigating how environment variables behave when a new program is executed using the **execve()** function.

By doing this, we aim to understand if the environment variables from the calling process are inherited by the new program.

<img src="../screenshots/logbook4/2.3__1_.png" alt="Image 2.3_1">

In the first step, when execve() was called with the environ argument set to NULL, the new program did not inherit environment variables from the calling process, indicating that environment variables are not automatically inherited by default. 

However, in the second step, when **execve()** was called with the environ argument **explicitly set** to the environ variable, the new program did inherit environment variables.


## 2.5 Task 5: Environment Variable and Set-UID Programs

Initially, we created a program to display the environment variables within the current process. Afterward, we compiled the program, changed its ownership to the root user, and transformed it into a Set-UID program following the provided instructions. In the shell, we utilized the "export" command in three different scenarios outlined in the lab tutorial.

<table>
  <tr>
    <td><img src="../screenshots/logbook4/2.5__1_.png" alt="Image 2.1_1"></td>
    <td><img src="../screenshots/logbook4/2.5__2_.png" alt="Image 2.1_2"></td>
  </tr>
</table>

After running the program we confirmed that the environment variables we previously defined were present in the program's list of environment variables. 

However, one key exception was LD_LIBRARY_PATH, which was omitted. This omission is due to LD_LIBRARY_PATH's ability to specify a path where the program looks for shared dynamic libraries. 

Allowing unrestricted modifications to this variable could create security risks by enabling the execution of potentially harmful programs that replace essential libraries. Therefore, for security reasons, LD_LIBRARY_PATH has been omitted to prevent such vulnerabilities.


## 2.6 Task 6: The PATH Environment Variable and Set-UID Programs

First, we begin by creating a file named "2_6.c" in which we implement the code as specified in the assignment. Next, we develop our malicious code in a file named "ls.c." Then, we compile and run our malicious code.

Afterwards, we change the ownership to root and turn it into a Set-UID program.

Finally, we modify the PATH environment variable to point to the directory where the malicious code is located.

As a result, when the "ls" command is executed, the code produced will be the malicious code rather than the system's "ls."

<img src="../screenshots/logbook4/2.6__1_.png" alt="Image 2.6">
