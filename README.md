# SISOPPrak2_1

#include <sys/wait.h>
#include <sys/types.h>
#include <unistd.h>
#include <stdlib.h>

void bikin ()
{
	char *argv[6] = {"printf","'%s\n'","*",">","output.txt",NULL};
	execv("/bin/printf",argv);
}

void comprss()
{
	char *argv[5] = {"sudo","zip","output.zip","output.txt",NULL};
	execv("/bin/sudo", argv);
}

void del()
{
	char *argv[3] = {"rm","output.txt",NULL};
	execv("/bin/rm",argv);
}

int main()
{
	pid_t child_id;
	int status;

	child_id = fork();

	if (child_id < 0) exit(EXIT_FAILURE);

	if (child_id == 0)
	{
		bikin();
	}

	else
	{
		while((wait(&status))>0);
			comprss();
			del();
	}
	return 0;
}
