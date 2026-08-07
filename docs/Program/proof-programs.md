# PROOF Programs

## Program
A Program is a reusable set of files and executables.
Generic Programs can be used in multiple Blocks with different Block descriptions and interfaces.
A Program consists of at least one standalone executable file performing at runtime.
Further dependencies for running the executable, e.g., configuration files, solvers, libraries or licenses, are
integrated as attachments.

## Structure of a Program

A *Program* consists of:

1. **Entry Point**: The main executable file (e.g., Python script, Java program, or MATLAB script) that contains the logic to be executed
2. **Attachments**: Optional additional files and dependencies required for the program's execution
3. **Configuration**: Metadata defining inputs, outputs, and program attributes

## Supported Program Types

### Python Programs
Python is the primary language supported for PROOF programs:
- Programs must implement the BaseWrapper interface
- Requires implementation of `init()`, `step()`, and `finalize()` methods
- Automatic Docker environment setup with required Python libraries

### Java Programs (Version 1.3.0+)
Java-based programs are now supported through the Java Wrapper implementation:
- Supports standalone JAR files and individual Java classes
- Java wrapper handles proper command parsing and execution
- Suitable for integrating Java-based simulation models
- Requires proper JAR manifest and classpath configuration

### Other Languages
Support for MATLAB and other languages is planned for future releases.

## Steps to Create a New Program

1. **Develop the Executable**: Create the main executable file (Python script, Java program, etc.)
2. **Prepare Attachments**: Gather all dependencies and supporting files
3. **Test Independently**: Ensure the program works correctly outside of PROOF
4. **Create Program in PROOF UI**:
   - Navigate to the *Config* menu
   - Click on the *Programs* button
   - Click the "**+ ADD**" button to create a new *Program*
   - Fill in the required fields: ID, label, description
   - Select and attach the previously created *Attachments*
   - Specify which *Attachment* is the entry point (the main executable)
   - Save the *Program*

After creating a Program, you can create Block Templates that use this program. See [Creating new Block Templates](../Block/creating-new-templates.md) for more details.
