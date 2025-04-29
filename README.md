# Linux-IPC-Message-Queues
Linux IPC-Message Queues

# AIM:
To write a C program that receives a message from message queue and display them

# DESIGN STEPS:

### Step 1:

Navigate to any Linux environment installed on the system or installed inside a virtual environment like virtual box/vmware or online linux JSLinux (https://bellard.org/jslinux/vm.html?url=alpine-x86.cfg&mem=192) or docker.

### Step 2:

Write the C Program using Linux message queues API 

### Step 3:

Execute the C Program for the desired output. 

# PROGRAM:

## C program that receives a message from message queue and display them
```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <sys/ipc.h>
#include <sys/msg.h>

#define MAX_TEXT 512
#define QUEUE_KEY_PATH "/tmp" // Any existing path
#define PROJ_ID 65            // Project identifier

// Define message structure
struct message {
    long msg_type;            // Message type must be > 0
    char msg_text[MAX_TEXT];  // Message data
};

int main() {
    key_t key;
    int msgid;
    struct message msg;

    // Generate unique key
    key = ftok(QUEUE_KEY_PATH, PROJ_ID);
    if (key == -1) {
        perror("ftok");
        exit(EXIT_FAILURE);
    }

    // Access message queue
    msgid = msgget(key, 0666 | IPC_CREAT);
    if (msgid == -1) {
        perror("msgget");
        exit(EXIT_FAILURE);
    }

    printf("Waiting for message...\n");

    // Receive message
    if (msgrcv(msgid, &msg, sizeof(msg.msg_text), 0, 0) == -1) {
        perror("msgrcv");
        exit(EXIT_FAILURE);
    }

    // Display the message
    printf("Received message: %s\n", msg.msg_text);

    return 0;
}

```



## OUTPUT




# RESULT:
The programs are executed successfully.
