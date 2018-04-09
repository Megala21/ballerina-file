### ballerina/file Package
The file package provides a bridge to perform file I/O related tasks by exposing the channels of the files. 

Following is a sample code snippet that can be used to write to a file,

```
file:Path filePath = file:getPath(pathValue);
io:ByteChannel channel = check file:newByteChannel(filePath,accessMode);
var result = channel.write(content,0);
var closeResult = channel.close();
```


In this code, the byteChannel of file is provided by newByteChannel function. This provides a way for ballerina/io package to continue the file write operation with the provided channel. Hence file package acts as mediation to solve the file I/O use cases.

All the CRUD(i.e. Create, Read, Update, Delete), copy and move operations involving files residing in local file systems can be accomplished using the functions exposed by this package.  In-addition to that, directory listening capabilities are provided out-of-box to facilitate the additional tasks that need to be done in the event of create/update/delete of a file within the respective directory. 

Path is a unique identifier of a file. Path can be either absolute or relative.  Absolute path refers to complete path information of a file where the information indicates the location of a file from the root of the file system. Relative path indicates the location of a file relative to the current location of the execution. All the functions in this package expects, file:Path type as a input parameter.  Retrieving file:Path of a file is simplified by getPath function.

Validations play vital role to achieve a smooth flow of a program. isDirectory, isExists are some of the functions which are mainly focused towards pre-validation. In addition to that some other utility functions are supported out-of box.  

File and Directories are protected entities. Depending on the OS, CRUD operations of a file could be restricted in various ways. Depending on the restrictions imposed, some functions in this package may fail and will return AccessDenied error. In-order to reduce the functions returning errors due to access restrictions, isReadable, isWritable functions can be used to check restrictions, prior to performing any operations.   Depending on the file state, functions may return errors in some other scenarios as well. One such scenario is trying to read a non-existing file. 
