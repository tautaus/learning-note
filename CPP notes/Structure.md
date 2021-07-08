# Structure of a Program

## Building and Running the program

In the circle of development, we first start in editing the *input file*, who has a more common name "code". After the edit, the *development toolchain* will compile the file and if the compile succeeds, *executeable file* will be outputed. When we want to run the program, the *O/S loader* will run the program. 

### Input Files

The input files in cpp include:
- User-Defined Code
  - Header files (like .h, .hpp), files that mostly define the structure
  - Source files (like .c, .cpp), files that mostly define the behaviour
  - Resource files(like .res, .qrc), files that contain things like icon and translations which don't contribute to the execution of the program
- Dependencies (libraries)
  - Heared files (<vector> ..)
  - Precompiled files, files that have objects or executeable preocompiled from libraries
  - Source files, files are provided by other libraries. The build will happen in the build of our own program.
  - Resources

### Development toolchain

During the developments, we use various tools:
- Source code tools, the tools we use to edit the source code like text editor or IDE
- Build tools, compilers
- Dependency tools, the tools we use to manage the dependency.
