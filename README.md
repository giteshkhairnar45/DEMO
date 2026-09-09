IPC using Pipe

Frist command 
nano ipc_pipe.c

#include <stdio.h>
#include <unistd.h>
#include <string.h>
#include <sys/types.h>

int main()
{
    int pipefd[2];
    pid_t pid;

    char message[] = "Hello from Parent Process!";
    char buffer[100];

    // Create pipe
    if (pipe(pipefd) == -1)
    {
        perror("Pipe creation failed");
        return 1;
    }

    // Create child process
    pid = fork();

    if (pid < 0)
    {
        perror("Fork failed");
        return 1;
    }

    if (pid > 0)
    {
        // Parent process
        close(pipefd[0]);  // Close reading end

        // Write message to pipe
        write(pipefd[1], message, strlen(message) + 1);

        printf("Parent sent: %s\n", message);

        close(pipefd[1]);  // Close writing end
    }
    else
    {
        // Child process
        close(pipefd[1]);  // Close writing end

        // Read message from pipe
        read(pipefd[0], buffer, sizeof(buffer));

        printf("Child received: %s\n", buffer);

        close(pipefd[0]);  // Close reading end
    }

    return 0;
}
 
Compile 
gcc ipc_pipe.c -o ipc_pipe

Run 
./ipc_pipe


Sample output 
Parent sent: Hello from Parent Process!
Child received: Hello from Parent Process!