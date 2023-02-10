---
author:
  name: "test"
date: 2022-02-10
linktitle: test 
type:
- post
- posts
title: Test 
categories: [test]
weight: 10
series:
- Hugo 101
---


## Test

> https://repnz.github.io/posts/apc/user-apc/#nttestalert

Hello.

```c
int main(int argc, const char** argv)
{
	
    //
    // Create the thread as a suspended thread.
    // NtTestAlert is called at the beginning of the lifetime of the new thread,
    // before 'NewThread' is executed.
    //
    // Because we want this NtTestAlert call to cause the thread to execute the APC,
    // we need to queue the APC before this NtTestAlert call is called.
    //
    // This is why we create the thread in a suspended state, then queue the APC, then
    // resume the thread.
    //
    // This technique can be used with remote threads as well and often used with CreateProcess(CREATE_SUSPENDED).
    //
	HANDLE ThreadHandle = CreateThread(NULL, 0, NewThread, NULL, CREATE_SUSPENDED, NULL);
	
	if (!QueueUserAPC(ApcRoutine, ThreadHandle, 0)){
		printf("Error queueing APC\n");
	}	

	printf("APC Queued. Calling ResumeThread..\n");

	ResumeThread(ThreadHandle);

	WaitForSingleObject(ThreadHandle, INFINITE);

	return 0;
}

VOID ApcRoutine(ULONG_PTR dwData){
	printf("Inside the ApcRoutine.\n");
}

DWORD NewThread(PVOID ThreadArgument){
	printf("Inside the thread.\n");
	return 0;
}
```
