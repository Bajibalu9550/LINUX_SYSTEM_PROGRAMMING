# 1. C Program: Write User Input to a File  

```c
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
#include <string.h>

int main() {
    char ch[20];
    printf("Enter file name: ");
    scanf("%s", ch);

    FILE *fp = fopen(ch, "w");
    if (fp == NULL) {
        printf("Error opening file");
        exit(1);
    }

    getchar();  
    char str[100];
    fgets(str, sizeof(str), stdin);
    str[strlen(str) - 1] = '\0';  /
    fputs(str, fp);
    fclose(fp);
}

------------------------------------------------
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<fcntl.h>
#include<string.h>

int main() {
    char filename[20];
    printf("Enter filename: ");
    scanf("%s", filename);

    int fd = open(filename, O_WRONLY | O_CREAT, 0640);
    if (fd < 0) {
        perror("Failed to open\n");
        exit(1);
    }

    write(fd, "HELLO WORLD", strlen("HELLO WORLD"));
    close(fd);
}

```
# 2. C Program: Read Content from a File and Display  

```c
#include <stdio.h>
#include <string.h>
#include <stdlib.h>

int main() {
    char ch[20];
    printf("Enter file name: ");
    scanf("%s", ch);

    FILE *fp = fopen(ch, "r");
    if (fp == NULL) {
        printf("Error at opening file");
        exit(1);
    }

    char str[100];
    while (fgets(str, 100, fp) != NULL) {
        str[strlen(str) - 1] = '\0';  // Remove newline
        puts(str);
    }

    fclose(fp);
}
```
# 2. Open existing file and disply its contents
## Source code
```c

#include<stdio.h>
#include<unistd.h>
#include<stdlib.h>
#include<string.h>
#include<fcntl.h>

int main() {
    char filename[20];
    printf("Enter filename: ");
    scanf("%s", filename);

    int fd = open(filename, O_RDONLY);
    if (fd < 0) {
        perror("Failed open\n");
        close(fd);
        exit(1);
    }

    char str[1024];
    int n;
    while ((n = read(fd, str, sizeof(str)-1)) > 0) {
        str[n] = '\0';
        write(1, str, strlen(str));
    }

    close(fd);
}

```

# 3. C Program: Create a Directory  

```c
#include <stdio.h>
#include <stdlib.h>
#include <sys/stat.h>
#include <sys/types.h>

int main() {
    char ch[20];
    printf("Enter directory name to create: ");
    scanf("%s", ch);

    if (mkdir(ch, 0777) == -1) {
        printf("Error at creating directory.\n");
        exit(1);
    }

    printf("Directory created successfully\n");
}

```
# 4. C Program: Check if a File Exists in the Current Directory  

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main() {
    char ch[20];
    printf("Enter file name: ");
    scanf("%s", ch);

    FILE *fp = fopen(ch, "r");
    if (fp == NULL) {
        printf("File does not present in present directory\n");
    } else {
        printf("File present in present directory\n");
        fclose(fp);
    }
}

------------------------------------------------------------

#include<stdio.h>
#include<fcntl.h>
#include<string.h>
#include<unistd.h>

int main() {
    char filename[20];
    printf("Enter filename: ");
    scanf("%s", filename);

    int fd = open(filename, O_RDONLY);
    if (fd < 0) {
        write(1, "File doesnot exist.\n", strlen("File doesnot exist.\n"));
    } else {
        write(1, "File exist.\n", strlen("File exist.\n"));
        close(fd);
    }
}

```
# 5. C Program: Rename a File  

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    char old[20], new[20];
    printf("Enter old file name: ");
    scanf("%s", old);

    printf("Enter new file name: ");
    scanf("%s", new);

    if (rename(old, new) == 0)
        printf("Rename successfully\n");
    else
        printf("Rename failed\n");
}
```
# 6. C Program: Delete a File  

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    char ch[20];
    printf("Enter file name to delete: ");
    scanf("%s", ch);

    if (remove(ch) == 0)
        printf("File deleted successfully.\n");
    else
        printf("File deletion failed.\n");
}
----------------------------------------------------
#include<stdio.h>
#include<stdlib.h>
#include<fcntl.h>
#include<unistd.h>

int main(){
        char file[20];
        printf("Enter filename: ");
        scanf("%s",file);

        int fd=open(file,O_RDONLY);
        if(fd>0){
                if(remove(file)==0){
                        printf("File removed successfully.\n");
                }
                else {
                        printf("Error at remove.\n");
                }
        }
        else {
                printf("File does not exist.\n");
        }
}

```
# 7. C Program: Copy Content from One File to Another  

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    char ch[20];
    printf("Enter source file name: ");
    scanf("%s", ch);

    char dch[20];
    printf("Enter destination file name: ");
    scanf("%s", dch);

    FILE *sfp = fopen(ch, "r");
    FILE *dfp = fopen(dch, "w");

    if (sfp == NULL || dfp == NULL) {
        printf("Error at opening file.\n");
        exit(1);
    }

    char str[100];
    while (fgets(str, 100, sfp) != NULL) {
        fputs(str, dfp);
    }

    fclose(sfp);
    fclose(dfp);
}
-------------------------------------------------
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <fcntl.h>

int main() {
    char sourcefile[20], destfile[20];
    printf("Enter source file name: ");
    scanf("%s", sourcefile);
    printf("Enter destfile: ");
    scanf("%s", destfile);

    int fd1 = open(sourcefile, O_RDONLY);
    int fd2 = open(destfile, O_WRONLY | O_CREAT, 0640);
    if (fd1 < 0 || fd2 < 0) {
        perror("Failed open()\n");
        exit(1);
    }

    char str[1024];
    int n;
    while ((n = read(fd1, str, 1024)) > 0) {
        write(fd2, str, n);
    }

    close(fd1);
    close(fd2);
}

```
# 8. C Program: Copy a File from One Directory to Another  

```c

#include<stdio.h>
#include<stdlib.h>
#include<fcntl.h>
#include<unistd.h>

int main(int args,char *argv[]){

        if(args!=3){
                printf("Enter <source file path> <Destination file path>.\n");
                return 1;
        }

        int sfd=open(argv[1],O_RDONLY);
        if(sfd<0){
                perror("Failed source open().\n");
                return 1;
        }

        int dfd=open(argv[2],O_WRONLY | O_CREAT,0640);
        if(dfd<0){
                perror("Failed dest open().\n");
                close(sfd);
                return 1;
        }

        char ch[1024];
        int bytes_read=0,bytes_write=0;

        while((bytes_read=read(sfd,ch,1024))!=0){
                bytes_write=write(dfd,ch,bytes_read);
                if(bytes_read != bytes_write){
                        printf("Error in written to file.\n");
                        close(sfd);
                        close(dfd);
                        return 1;
                }
        }
        printf("File moved successfully.\n");
        close(sfd);
        close(dfd);
}
--------------------------------------------------------------------
#include <stdio.h>
#include <stdlib.h>

int main() {
    char src[100], dest[100];

    printf("Enter source file path (with directory): ");
    scanf("%s", src);

    printf("Enter destination file path (with directory): ");
    scanf("%s", dest);

    FILE *sfp = fopen(src, "r");
    FILE *dfp = fopen(dest, "w");

    if (sfp == NULL || dfp == NULL) {
        printf("Error at opening file.\n");
        exit(1);
    }

    char ch;
    while ((ch = fgetc(sfp)) != EOF) {
        fputc(ch, dfp);
    }

    fclose(sfp);
    fclose(dfp);

    printf("File copied successfully from %s to %s\n", src, dest);
}

-----------------------------------------------------------------------
#include <stdio.h>
#include <stdlib.h>

int main() {
    char source[100], dest[100];
    printf("Enter source file path: ");
    scanf("%s", source);
    printf("Enter destination file path: ");
    scanf("%s", dest);

    if (rename(source, dest) == 0) {
        printf("File moved successfully.\n");
    } else {
        perror("Failed to move file");
        exit(1);
    }

    return 0;
}
 
```
# 8. C Program: Copy File Using System Calls (open, read, write)

```c
#include <stdio.h>
#include <stdlib.h>
#include <fcntl.h>
#include <unistd.h>

int main() {
    char sfp[100], dfp[100];
    printf("Enter source file path: ");
    scanf("%s", sfp);
    printf("Enter destination file path: ");
    scanf("%s", dfp);

    int fd1 = open(sfp, O_RDONLY);
    int fd2 = open(dfp, O_RDWR | O_CREAT, 0640);

    if (fd1 < 0 || fd2 < 0) {
        printf("Error opening file.\n");
        exit(1);
    }

    char str[100];
    int x;
    while ((x = read(fd1, str, sizeof(str))) > 0) {
        write(fd2, str, x);
    }

    close(fd1);
    close(fd2);

    printf("File copied successfully using system calls.\n");
}

```
# 9. C Program: List All Files in the Current Directory

```c
#include <stdio.h>
#include <stdlib.h>
#include <dirent.h>

int main() {
    DIR *d;
    struct dirent *dir;

    
    d = opendir(".");
    if (d == NULL) {
        printf("Could not open current directory.\n");
        exit(1);
    }

    printf("Files in current directory:\n");

    
    while ((dir = readdir(d)) != NULL) {
        // Ignore "." and ".."
        if (dir->d_name[0] != '.')
            printf("%s\n", dir->d_name);
    }

    closedir(d);
    return 0;
}
```
# 10. C Program: Get File Size

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    char filename[50];
    printf("Enter file name: ");
    scanf("%s", filename);

    FILE *fp = fopen(filename, "r");
    if (fp == NULL) {
        printf("Error opening file.\n");
        exit(1);
    }

    // Move file pointer to end
    fseek(fp, 0, SEEK_END);

    // Get current position (file size)
    long size = ftell(fp);

    fclose(fp);

    printf("File size: %ld bytes\n", size);

    return 0;
}
--------------------------------------------------------
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <fcntl.h>
#include <sys/stat.h>

int main() {
    struct stat s;
    char filename[20];
    printf("Enter file name: ");
    scanf("%s", filename);

    int fd = open(filename, O_RDONLY);
    if (fd < 0) {
        perror("Failed open()\n");
        exit(1);
    }

    fstat(fd, &s);
    close(fd);

    printf("Size of file: %ld\n", s.st_size);
    return 0;
}


```
# 11. C Program: Check if a Directory Exists in the Current Directory

```c
#include <stdio.h>
#include <stdlib.h>
#include <dirent.h>
#include <string.h>

int main() {
    char dirname[50];
    printf("Enter directory name to check: ");
    scanf("%s", dirname);

    DIR *d = opendir(dirname);
    if (d) {
        printf("Directory '%s' exists in the current directory.\n", dirname);
        closedir(d);
    } else {
        printf("Directory '%s' does NOT exist in the current directory.\n", dirname);
    }

    return 0;
}
```
# 12. C Program: Create a Directory

```c
#include <stdio.h>
#include <stdlib.h>
#include <sys/stat.h>
#include <sys/types.h>

int main() {
    char ch[20];
    printf("Enter directory name to create: ");
    scanf("%s", ch);

    if (mkdir(ch, 0777) == -1) {
        printf("Error at creating directory.\n");
        exit(1);
    }

    printf("Directory created successfully\n");
    return 0;
}
```
# 13. C Program: List All Files and Directories in Current Directory

```c
#include <stdio.h>
#include <stdlib.h>
#include <dirent.h>

int main() {
    DIR *d;
    struct dirent *dir;

    // Open the current directory
    d = opendir(".");
    if (d == NULL) {
        printf("Could not open current directory.\n");
        exit(1);
    }

    printf("Files and directories in current directory:\n");

    // Read and print all entries
    while ((dir = readdir(d)) != NULL) {
        printf("%s\n", dir->d_name);
    }

    closedir(d);
    return 0;
}
```
# 14. C Program: Delete All Files in a Directory

```c
#include <stdio.h>
#include <stdlib.h>
#include <dirent.h>
#include <string.h>

int main() {
    char dirname[50];
    printf("Enter directory name: ");
    scanf("%s", dirname);

    DIR *d = opendir(dirname);
    if (d == NULL) {
        printf("Could not open directory.\n");
        exit(1);
    }

    struct dirent *dir;
    char filepath[100];

    while ((dir = readdir(d)) != NULL) {
       
        if (strcmp(dir->d_name, ".") == 0 || strcmp(dir->d_name, "..") == 0)
            continue;

        
        snprintf(filepath, sizeof(filepath), "%s/%s", dirname, dir->d_name);

       
        if (remove(filepath) == 0) {
            printf("Deleted: %s\n", filepath);
        } else {
            printf("Failed to delete: %s\n", filepath);
        }
    }

    closedir(d);
    return 0;
}
```
# 15. C Program: Count Number of Lines in a File Using `open()` and `close()`

```c
#include <stdio.h>
#include <stdlib.h>
#include <fcntl.h>
#include <unistd.h>

int main() {
    char filename[50];
    printf("Enter file name: ");
    scanf("%s", filename);

    int fd = open(filename, O_RDONLY);
    if (fd < 0) {
        printf("Error opening file.\n");
        exit(1);
    }

    char buffer[1024];
    int bytesRead;
    int lines = 0;

    while ((bytesRead = read(fd, buffer, sizeof(buffer))) > 0) {
        for (int i = 0; i < bytesRead; i++) {
            if (buffer[i] == '\n') {
                lines++;
            }
        }
    }

    close(fd);
    printf("Number of lines in file '%s' = %d\n", filename, lines);

    return 0;
}
```
# 15. C Program: Count Number of Lines in a File

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    char filename[50];
    printf("Enter file name: ");
    scanf("%s", filename);

    FILE *fp = fopen(filename, "r");
    if (fp == NULL) {
        printf("Error opening file.\n");
        exit(1);
    }

    int lines = 0;
    char ch;

    while ((ch = fgetc(fp)) != EOF) {
        if (ch == '\n') {
            lines++;
        }
    }

    fclose(fp);
    printf("Number of lines in file '%s' = %d\n", filename, lines);

    return 0;
}
```
# 16. C Program: Append Data at the End of a File

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    char filename[50];
    char newData[1000];

    printf("Enter file name: ");
    scanf("%s", filename);

    printf("Enter data to append at end: ");
    getchar(); 
    fgets(newData, sizeof(newData), stdin);

    newData[strlen(newData)-1]='\0';

    
    FILE *fp = fopen(filename, "a");
    if (fp == NULL) {
        printf("Error opening file.\n");
        exit(1);
    }

    fprintf(fp, "%s\n", newData);

    fclose(fp);

    printf("Data appended at the end successfully.\n");

    return 0;
}
```
# 16. C Program: Append Data at the End of a File Using System Calls

```c
#include <stdio.h>
#include <stdlib.h>
#include <fcntl.h>
#include <unistd.h>
#include <string.h>

int main() {
    char filename[50];
    char newData[1000];

    printf("Enter file name: ");
    scanf("%s", filename);

    printf("Enter data to append at end: ");
    getchar(); 
    fgets(newData, sizeof(newData), stdin);

    newData[strlen(newData)-1]='\0';


    int fd = open(filename, O_WRONLY | O_APPEND);
    if (fd < 0) {
        printf("Error opening file.\n");
        exit(1);
    }


    write(fd, newData, strlen(newData));
    write(fd, "\n", 1); // add newline at the end

    close(fd);

    printf("Data appended at the end successfully.\n");

    return 0;
}
```
# 17. C Program: Change Permissions of a File

```c
#include <stdio.h>
#include <stdlib.h>
#include <sys/stat.h>

int main() {
    char filename[50];
    mode_t mode;

    // Get file name from user
    printf("Enter file name: ");
    scanf("%s", filename);

    // Get permission in octal format (e.g., 0644)
    printf("Enter new permissions (octal, e.g., 0644): ");
    scanf("%o", &mode);

    // Change file permissions
    if (chmod(filename, mode) == 0) {
        printf("Permissions of '%s' changed successfully.\n", filename);
    } else {
        perror("chmod");
        exit(1);
    }

    return 0;
}
```
# 18. C Program: Change Ownership of a File

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/types.h>

int main() {
    char filename[50];
    uid_t owner;
    gid_t group;

   
    printf("Enter file name: ");
    scanf("%s", filename);

    
    printf("Enter new owner UID: ");
    scanf("%u", &owner);

 
    printf("Enter new group GID: ");
    scanf("%u", &group);

    
    if (chown(filename, owner, group) == 0) {
        printf("Ownership of '%s' changed successfully.\n", filename);
    } else {
        perror("chown");
        exit(1);
    }

    return 0;
}
```
# 19. C Program: Display Last Modification Time of a File

```c
#include <stdio.h>
#include <sys/stat.h>
#include <time.h>
#include <stdlib.h>

int main() {
    char ch[20];
    printf("Enter file name: ");
    scanf("%s", ch);

    struct stat st;
    if (stat(ch, &st) == -1) {
        printf("ERROR: Could not get file information.\n");
        exit(1);
    }

    printf("Last modification timestamp of file '%s': %s", ch, ctime(&st.st_mtime));

    return 0;
}

```
# 20. C Program: Create Temporary File and Write/Read Data

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    
    FILE *tmp = tmpfile();
    if (tmp == NULL) {
        printf("Error creating temporary file.\n");
        exit(1);
    }

    
    fprintf(tmp, "Hello world!\n");
    fprintf(tmp, "This is Balu.\n");

  
    fseek(tmp, 0, SEEK_SET);

    char buf[100];
    printf("Data in temporary file:\n");

    
    while (fgets(buf, sizeof(buf), tmp) != NULL) {
        printf("%s", buf);
    }

    fclose(tmp);

    return 0;
}
```
# 21. C Program: Check if Path is a File or Directory

```c
#include <stdio.h>
#include <stdlib.h>
#include <sys/stat.h>

int main() {
    char path[100];
    printf("Enter path: ");
    scanf("%s", path);

    struct stat st;
    if (stat(path, &st) == -1) {
        perror("stat");
        exit(1);
    }

    if (S_ISREG(st.st_mode)) {
        printf("'%s' is a regular file.\n", path);
    } else if (S_ISDIR(st.st_mode)) {
        printf("'%s' is a directory.\n", path);
    } else {
        printf("'%s' is neither a regular file nor a directory.\n", path);
    }

    return 0;
}

```
# 22. C Program to Create a Hard Link

This program creates a **hard link** named `hardlink.txt` to an existing file `source.txt` using the `link()` system call.

```c
#include <stdio.h>
#include <unistd.h>  // for link()
#include <errno.h>   // for errno
#include <string.h>  // for strerror()

int main() {
    const char *source = "source.txt";      // Original file
    const char *hardlink = "hardlink.txt";  // Name of the hard link to create

    // Create hard link
    if (link(source, hardlink) == 0) {
        printf("Hard link '%s' created successfully for '%s'.\n", hardlink, source);
    } else {
        printf("Failed to create hard link: %s\n", strerror(errno));
    }

    return 0;
}

NOTE:

Hard links cannot link directories.

Both files must be on the same filesystem.

source.txt and hardlink.txt share the same inode, so changes in one file affect the other.

Possible outputs:

Success: Hard link 'hardlink.txt' created successfully for 'source.txt'.

Failure: Failed to create hard link: No such file or directory or Failed to create hard link: File exists.
```
# 23. C Program to Read and Display Contents of a CSV File

This program reads a **CSV file** named `data.csv` and displays its contents line by line.

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    FILE *file;
    char line[1024];  

    
    file = fopen("data.csv", "r");
    if (file == NULL) {
        perror("Error opening file");
        return 1;
    }

    printf("Contents of data.csv:\n");

   
    while (fgets(line, sizeof(line), file)) {
        printf("%s", line);  // fgets includes the newline character
    }

   
    fclose(file);

    return 0;
}
-----------------------------------------------------------------------------------------
#include<stdio.h>
#include<fcntl.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<errno.h>
int main(){
        char csv[20];
        printf("Enter csv file name: ");
        scanf("%s",csv);

        int fd=open(csv,O_RDWR | O_CREAT|O_APPEND,0640);
        if(fd<0){
                printf("%s\n",strerror(errno));
                return 1;
        }

        char str[1024];
        while(1){
               write(1,"Enter Text: ",12);

               int n=read(0,str,1023);
               if(n<=0){
                       printf("read\n");
                       break;
               }
               str[n]='\0';
               str[strlen(str)-1]='\0';


               if(strcmp(str,"exit")==0){
                       break;
               }
               strcat(str,"\n");

               if(write(fd,str,strlen(str))<0){
                       printf("write");
                       break;
               }
        }
        close(fd);
}

```
# 24. C Program to Get the Absolute Path of the Current Working Directory

```c
#include <stdio.h>
#include <unistd.h>
#include <limits.h>
#include <stdlib.h>

int main() {
    char cwd[PATH_MAX];

    if (getcwd(cwd, sizeof(cwd)) != NULL) {
        printf("Current working directory: %s\n", cwd);
    } else {
        perror("getcwd() error");
        exit(1);
    }

    return 0;
}
```
# 25. C Program to Get the Size of a Directory Named "Documents"

```c
#include <stdio.h>
#include <stdlib.h>
#include <sys/stat.h>
#include <dirent.h>
#include <string.h>

long getDirectorySize(const char *path) {
    struct dirent *entry;
    struct stat fileStat;
    DIR *dp = opendir(path);
    long totalSize = 0;
    char fullPath[1024];

    if (dp == NULL) {
        perror("opendir");
        return 0;
    }

    while ((entry = readdir(dp)) != NULL) {
        if (strcmp(entry->d_name, ".") == 0 || strcmp(entry->d_name, "..") == 0)
            continue;

        snprintf(fullPath, sizeof(fullPath), "%s/%s", path, entry->d_name);

        if (stat(fullPath, &fileStat) == -1) {
            perror("stat");
            continue;
        }

        if (S_ISDIR(fileStat.st_mode)) {
            totalSize += getDirectorySize(fullPath);
        } else {
            totalSize += fileStat.st_size;
        }
    }

    closedir(dp);
    return totalSize;
}

int main() {
    const char *dirPath = "Documents";
    long size = getDirectorySize(dirPath);
    printf("Total size of directory '%s': %ld bytes\n", dirPath, size);
    return 0;
}
```









