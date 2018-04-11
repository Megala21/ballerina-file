The `ballerina/file` package provides local file system operations. 

`Path` is a unique identifier of a file. `Path` can be either absolute or relative.  Absolute path refers to complete path information of a file where the information indicates the location of a file from the root of the file system.  Relative path indicates the location of a file relative to the current location of the execution.
```ballerina
string relativePathValue = “./doc”;
file:Path relativePath = new(relativePathValue);

string absolutePathValue = “/home/user/ballerina/doc”;
file:Path absolutePath = new(absolutePathValue);
```
The function `toAbsolutePath()` gives the absolute path of a file.
```ballerina
file:Path absolutePath = relativePath.toAbsolutePath();
```
For a given relative path, absolute path may differ depending on underlying OS (Operating System). 

For example,
```ballerina
string relativePathValue = “./examples”;
file:Path relativePath = new(relativePathValiue);
string absolutePathValue = relativePath.toAbsolutePath().getPathValue();
```
If above code is executed in unix based OS, absolutePathValue will hold a string similar to the one  below,
 `/home/user/ballerina/examples`

However, execution of same code in windows OS will make absolutePathValue to hold value like below,
`C:\windows\user\ballerina\examples`

By using `Path`,file I/O operations such as writing, updating, copying and moving can be achieved in conjunction with `ballerina/io` package. 

For example to write to a file the following could be done:
```ballerina
String accessMode = “w”;
file:Path filePath = new(filePathValue)
io:ByteChannel channel = io:openFile(filePath,accessMode);
var result = channel.write(content,0);
var closeResult = channel.close();
```
The constructor `new()` creates a `Path`. File write operation can be completed using `openFile()` and `write()` functions exposed by `ballerina/io` package. `openFile()` function creates a streaming channel to a local file. Channels provide read / write access to different resources, in this case files. 

The `ballerina/file` package provides functions for creating, deleting files. The package provides directory listening that triggers events when a file is modified. 

To listen to file creation event:,
```ballerina
endpoint file:Listener localFolder {
    path:"target/fs"
};
service fileSystem bind localFolder {
    onCreate (file:FileEvent m) {
    }
}
```
The `onCreate()` resource method will get invoked in the event of file creation inside `“target/fs”` folder. `onDelete()` and `onModify()` methods can be used to listen delete, modify events respectively.

Validation is a process of verifying whether all the pre-conditions and post-conditions of an operation is satisfied. The package supports pre-validation and post-validation actions through functions such as `exists()` and `isDirectory()`.

To create directory and add a file to directory:,
```ballerina
file:Path directoryPath = new(directoryPathValue);
file:Path filePath = new(filePathValue);
var isExists = file:exists(directoryPath);
if (!isExists) {
  var result = file:createDirectory(directoryPath);
}
If (file:isDirectory(directoryPath)) {
  var createFileResult = file:createFile(filePath);
}
```
File and directories are protected entities. Depending on the OS, file operations can be restricted in various ways. Functions that access a protected or non-existant entity will return an `AccessDenied` error. 
