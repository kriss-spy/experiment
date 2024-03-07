# dir.h
`dir.h` 是一个在较老版本的 C 编译器（如 Turbo C 和 Borland C++）中常见的头文件，它提供了用于目录操作的函数和宏
这些函数和宏允许程序员在程序中执行基本的目录遍历、创建和删除等操作
在 `dir.h` 中，你可能会找到以下一些函数和宏：

1. `mkdir(const char *pathname)`: 创建一个新目录。
2. `rmdir(const char *pathname)`: 删除一个空目录。
3. `findfirst()`: 查找指定路径下的第一个文件或目录，并返回一个搜索句柄。
//21级代码至少用了chdir()和rmdir()
这些函数和宏通常与特定的系统调用相对应，以执行底层的文件系统操作
例如，`mkdir` 函数可能会调用操作系统的底层 API 来创建目录

如果你正在使用 Turbo C 或类似的老旧编译器，并且需要处理目录操作，那么 `dir.h` 可能是你唯一的选择
## 函数分析
```c
1. int _CType chdir(const char _FAR *__path)
功能：更改当前工作目录到指定的路径。
参数：__path - 要切换到的目录的路径字符串。
返回值：如果成功，返回0；如果失败，返回-1。

2. int _CType _FARFUNC findfirst(const char _FAR *__path, struct ffblk _FAR *__ffblk, int __attrib)
功能：在指定目录中查找第一个匹配的文件或子目录。
参数：
__path - 要搜索的目录路径。
__ffblk - 指向ffblk结构的指针，用于存储找到的文件或目录的信息。
__attrib - 文件属性掩码，用于指定要查找的文件类型。
返回值：如果找到匹配项，返回0；如果没有找到匹配项或发生错误，返回-1。

3. int _CType _FARFUNC findnext(struct ffblk _FAR *__ffblk)
功能：在之前的findfirst调用之后，继续查找下一个匹配的文件或子目录。
参数：__ffblk - 指向ffblk结构的指针，用于存储找到的文件或目录的信息。
返回值：如果找到匹配项，返回0；如果没有找到匹配项或发生错误，返回-1。

4. void _CType _FARFUNC fnmerge(char _FAR *__path, const char _FAR *__drive, const char _FAR *__dir, const char _FAR *__name, const char _FAR *__ext)
功能：合并磁盘驱动器、目录、文件名和扩展名，生成完整的文件路径。
参数：
__path - 指向存储完整文件路径的缓冲区的指针。
__drive - 磁盘驱动器的名称（例如"A:"、"C:"等）。
__dir - 目录的路径。
__name - 文件名。
__ext - 文件扩展名。
返回值：无。

//fnsplit与_funsplit区别？
int _CType _FARFUNC _fnsplit(const char _FAR *__path, char _FAR *__drive, 
char _FAR *__dir, char _FAR *__name, char _FAR *__ext)
和 int _CType _FARFUNCfnsplit(const char _FAR *__path, char _FAR *__drive,
char _FAR *__dir, char _FAR *__name, char _FAR *__ext)
功能：将一个完整的文件路径拆分为磁盘驱动器、目录、文件名和扩展名。
参数：
__path - 要拆分的完整文件路径。
__drive - 存储磁盘驱动器名称的缓冲区。
__dir - 存储目录路径的缓冲区。
__name - 存储文件名的缓冲区。
__ext - 存储文件扩展名的缓冲区。
返回值：如果成功，返回0；如果发生错误，返回-1。

6. int _Cdecl getcurdir(int __drive, char _FAR *__directory)
功能：获取当前工作目录的路径。
参数：
__drive - 磁盘驱动器的编号。
__directory - 存储当前工作目录路径的缓冲区。
返回值：如果成功，返回当前目录的字符数；如果发生错误，返回-1。

7. char _FAR * _Cdecl _FARFUNC getcwd(char _FAR *__buf, int __buflen);
功能：获取当前工作目录的完整路径。
参数：__buf 是一个字符数组，用于存储路径，__buflen 是该数组的大小。
返回值：返回当前工作目录的字符串指针。

8. int _Cdecl getdisk(void);
功能：获取当前默认磁盘驱动器号。
返回值：返回当前默认磁盘的驱动器号。

9. int _Cdecl mkdir(const char _FAR *__path);
功能：创建一个新目录。
参数：__path 是要创建的目录的路径。
返回值：成功时返回0，失败时返回-1。

10. char _FAR * _Cdecl _FARFUNC mktemp(char _FAR *__template);
功能：创建一个唯一的临时文件名。
参数：__template 是一个包含临时文件模板的字符串。
返回值：返回生成的唯一临时文件名的字符串指针。

11. int _Cdecl rmdir(const char _FAR *__path);
功能：删除一个空目录。
参数：__path 是要删除的目录的路径。
返回值：成功时返回0，失败时返回-1。

12. char _FAR * _CType _FARFUNC searchpath(const char _FAR *__file);
功能：在指定的目录中搜索文件。
参数：__file 是要搜索的文件名。
返回值：如果找到文件，则返回文件的完整路径；否则返回NULL。

13. int _Cdecl setdisk(int __drive);
功能：设置默认磁盘驱动器。
参数：__drive 是要设置为默认的驱动器号。
返回值：成功时返回0，失败时返回-1。
```

## 具体内容
```c title:dir.h
/*  dir.h

    Defines structures, macros, and functions for dealing with
    directories and pathnames.

    Copyright (c) 1987, 1992 by Borland International
    All Rights Reserved.
*/

#if !defined(__DIR_H)
#define __DIR_H

#if !defined(___DEFS_H)
#include <_defs.h>
#endif

#ifndef _FFBLK_DEF
#define _FFBLK_DEF
struct  ffblk   {
    char        ff_reserved[21];
    char        ff_attrib;
    unsigned    ff_ftime;
    unsigned    ff_fdate;
    long        ff_fsize;
    char        ff_name[13];
};
#endif

#define WILDCARDS 0x01
#define EXTENSION 0x02
#define FILENAME  0x04
#define DIRECTORY 0x08
#define DRIVE     0x10

#define MAXPATH   80
#define MAXDRIVE  3
#define MAXDIR    66
#define MAXFILE   9
#define MAXEXT    5

#ifdef __cplusplus
extern "C" {
#endif

int         _CType chdir( const char _FAR *__path );
int         _CType _FARFUNC findfirst( const char _FAR *__path,
                              struct ffblk _FAR *__ffblk,
                              int __attrib );
int         _CType _FARFUNC findnext( struct ffblk _FAR *__ffblk );
void        _CType _FARFUNC fnmerge( char _FAR *__path,
                            const char _FAR *__drive,
                            const char _FAR *__dir,
                            const char _FAR *__name,
                            const char _FAR *__ext );
int _CType _FARFUNC _fnsplit(const char _FAR *__path,
                            char _FAR *__drive,
                            char _FAR *__dir,
                            char _FAR *__name,
                            char _FAR *__ext );
int _CType _FARFUNC fnsplit( const char _FAR *__path,
                            char _FAR *__drive,
                            char _FAR *__dir,
                            char _FAR *__name,
                            char _FAR *__ext );
int         _Cdecl getcurdir( int __drive, char _FAR *__directory );
char _FAR * _Cdecl _FARFUNC getcwd( char _FAR *__buf, int __buflen );
int         _Cdecl getdisk( void );
int         _Cdecl mkdir( const char _FAR *__path );
char _FAR * _Cdecl _FARFUNC mktemp( char _FAR *__template );
int         _Cdecl rmdir( const char _FAR *__path );
char _FAR * _CType _FARFUNC searchpath( const char _FAR *__file );
int         _Cdecl setdisk( int __drive );
#ifdef __cplusplus
}
#endif

#endif  /* __DIR_H */

```
# mkdir()
`mkdir` 是`dir.h`中定义的一个函数，用于在文件系统中创建一个新的目录（文件夹）。

`mkdir` 函数的原型如下：
```c
int mkdir(const char *pathname);
```

`pathname` 参数是一个指向将要创建的目录名称的字符串的指针。

这个函数返回一个整数值：

- 如果目录创建成功，函数返回 0。
- 如果目录创建失败，函数返回 -1，并设置全局变量 `errno` 以指示错误类型
- 可以使用 `perror` 函数来打印出错误消息
 
## 创建目录的示例程序：

```c
#include <stdio.h>  
#include <stdlib.h>  
#include <dir.h>    
#include <dirent.h
int main() 
{      
char *newDir = "C:\\MyNewDirectory";        // 尝试创建新目录
//注意，实际上bc命名最多8字符
if (mkdir(newDir) == 0) 
{          
	printf("Directory '%s' created successfully.\n", newDir);      
} 
else 
{          
	perror("Error creating directory");          
	exit(EXIT_FAILURE);      
}        
return 0;  
}
```
# 遍历目录
```c
#include <stdio.h>  
#include <dir.h>  
#include<dirent.h>
#include <string.h>  
#include <dos.h>  

void listFilesAndFolders(const char *path) {  
    struct dirent *de;  // Directory entry structure  
    DIR *dr = opendir(path);  // Open directory  
    if (dr == NULL) {  
        printf("Could not open directory\n");  
        return;  
    }  
  
    while ((de = readdir(dr)) != NULL) {  // Read directory entries  
        if (strcmp(de->d_name, ".") == 0 || strcmp(de->d_name, "..") == 0)  
            continue;  // Skip '.' and '..'  
  
        printf("%s\n", de->d_name);  
  
        // If you want to recursively list subdirectories,  
        // you would need to implement that logic here.  
        // However, Turbo C does not have built-in support for recursive directory traversal.  
    }  
    closedir(dr);  // Close directory  
}  
  
int main() {  
    const char *folderPath = "C:\\path\\to\\your\\folder";  // Replace with your directory path  
    listFilesAndFolders(folderPath);  
    return 0;  
}
```
## 创建目录后创建子文件
下面是一个适用于 Turbo C 的示例，用于根据用户输入的文件夹名称来创建文件夹和文件：
```c
#include <stdio.h>  
#include <stdlib.h>  
#include <dir.h>  
#include <string.h>  
  
int main() {  
    // 假设用户输入的文件夹名称  
    char userDirName[100];  
    printf("input new folder name: ");  
    scanf("%s", userDirName); // 读取用户输入，不限制长度（注意：这可能导致缓冲区溢出）  
  
    // 基础目录路径，确保以斜杠结尾  
    char baseDirPath[] = "path\\to\\your\\base\\directory\\";  
    // 新建文件的名称  
    char fileName[] = "newfile.txt";  
  
    // 创建完整的文件夹路径  
    char dirPath[100];  
    strcpy(dirPath, baseDirPath);  
    strcat(dirPath, userDirName);  
  
    // 创建文件夹  
    if (mkdir(dirPath) != 0)
    {  
        perror("Error creating directory");  
        return 1;  
    }  

    // 假设用户输入的文件名称，需要输入拓展名
    char userfileName[100];
    printf("input new file name;");
    scanf("%s",userfileName);
     
    // 创建完整的文件路径  
    char filePath[100];  
    strcpy(filePath, dirPath);  
    strcat(filePath, "\\");  
    strcat(filePath, userfileName);  
  
    // 使用fopen()函数创建并打开文件  
    FILE *file = fopen(filePath, "w");  
    if (file == NULL) {  
        perror("Error creating file");  
        return 1;  
    }  
  
    // 在这里可以向文件中写入内容  
    fprintf(file, "This is a new file in the user-specified directory.\n");  
  
    // 关闭文件  
    fclose(file);  
  
    printf("File created successfully in directory '%s'.\n", userDirName);  
  
    return 0;  
}
```
# 简单复制jpg
```c title:copy.c
#include<stdio.h>
#include<stdlib.h>
#include<dos.h>
#include<dir.h>
#include<string.h>
//文心一言建议用 off_t file_size，但其实ftell返回long，off_t没定义？
int file_size(FILE *fp)
 {
	int curpos, count;
	curpos=ftell(fp);
	fseek(fp,0,SEEK_END);
	count=ftell(fp);
	rewind(fp);
	//rewind()和fseek( , SEEK_SET, 0)差不多，但rewind()会跳过异常字符
	//fseek则更通用，可以用于二进制文件
	fseek(fp,curpos,SEEK_SET);
	return count;
 }
int main()
{
	FILE *f1, *f2;
	char buffer[256];
	char srcName[]="test.jpg";
   //	char destName[]="dest.txt";
	char destName[20];
	printf("input new png file name:");
	scanf("%s",destName);
	strcat(destName,".jpg");

	if((f1=fopen(srcName,"rb"))==NULL)
	{
		printf("error opening f1");
		return 1;
	}
	if((f2=fopen(destName,"wb"))==NULL)
	{
		printf("error opening f2");
		return 1;
	}
	while(!feof(f1))
	{
		size_t bytesRead = fread(buffer,1,sizeof(buffer),f1);
		fwrite(buffer,1,bytesRead,f2);
	}
	/*
	char *temp;
	temp=(char*)malloc(file_size(f1)*sizeof(char));
	fread(temp,1,file_size(f1),f1);
	fwrite(temp,1,file_size(f1),f2);
	*/
	
	//remove(f1)
	fclose(f1);
	fclose(f2);
	
	printf("\nend!");
	return 0;
}
```