# SISOPPrak2_1

#include <sys/wait.h>
#include <sys/types.h>
#include <unistd.h>
#include <stdlib.h>

void bikin ()
{
    FILE * filein;
    filein = fopen ("output.txt", "w");
    struct dirent *de;
 
    DIR *dr = opendir("/bin");
 
    if (dr == NULL)
    {
        printf("Could not open current directory" );
        return 0;
    }
 
    while ((de = readdir(dr)) != NULL)
        fprintf(filein, "%s\n", de->d_name);
 
    closedir(dr);    
    return 0;
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
