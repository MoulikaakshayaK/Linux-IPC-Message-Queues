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

#define MAX_TEXT 100

// Define message structure
struct message {
    long msg_type;
    char msg_text[MAX_TEXT];
};

int main() {
    key_t key;
    int msgid;
    struct message msg;

    // Generate unique key (same key must be used by sender)
    key = ftok("msgqueuefile", 65);  // File "msgqueuefile" must exist
    if (key == -1) {
        perror("ftok");
        exit(EXIT_FAILURE);
    }

    // Get message queue id
    msgid = msgget(key, 0666 | IPC_CREAT);
    if (msgid == -1) {
        perror("msgget");
        exit(EXIT_FAILURE);
    }

    // Receive message of type 1
    if (msgrcv(msgid, &msg, sizeof(msg.msg_text), 1, 0) == -1) {
        perror("msgrcv");
        exit(EXIT_FAILURE);
    }

    // Display received message
    printf("Received message: %s\n", msg.msg_text);

    // Optional: Delete the message queue
    msgctl(msgid, IPC_RMID, NULL);

    return 0;
}
```



## OUTPUT




# RESULT:
The programs are executed successfully.
