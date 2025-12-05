# Chapter 14 - Tech Support System with File I/O

## Project Overview

This project extends a technical support system to read responses and default messages from external files instead of using hardcoded values. The system simulates an automated customer support chat bot that responds to user queries with pre-defined responses based on keywords.

## Assignment Requirements

### Task 1: Enhanced Default Responses Reading
- **Objective**: Modify the `fillDefaultResponses` method to read multiline entries from `default.txt`
- **File Format**: 
  - Consecutive lines are appended together to form complete responses
  - Blank lines mark the end of each response entry
  - Multiple consecutive blank lines trigger a checked exception

### Task 2: Dynamic Response Map Population
- **Objective**: Change `fillResponseMap` method to read from `responses.txt` instead of hardcoded values
- **File Format**:
  - First line contains comma-separated keywords
  - Following non-blank lines constitute the response text
  - Blank lines separate different key-value pairs

## Implementation Details

### Custom Exception Handling
Created a custom checked exception `MultipleBlankLinesException` that is thrown when two or more consecutive blank lines are encountered in either file. This provides better error handling and helps identify potential file formatting issues.

```java
class MultipleBlankLinesException extends Exception {
    public MultipleBlankLinesException(String message) {
        super(message);
    }
}
```

### File Reading Logic

#### Default Responses (`default.txt`)
The enhanced `fillDefaultResponses` method:
- Reads line by line from `default.txt`
- Concatenates consecutive non-blank lines with newlines
- Treats blank lines as response separators
- Monitors for consecutive blank lines and throws exception if found
- Gracefully handles missing files with fallback responses

#### Response Map (`responses.txt`)
The updated `fillResponseMap` method:
- Parses comma-separated keywords on the first line of each entry
- Concatenates subsequent non-blank lines with spaces to form responses
- Maps each keyword to the same response text
- Separates entries with blank lines
- Includes the same blank line validation as default responses

### File Format Examples

#### `default.txt` Format:
```
That sounds odd. Could you 
describe that problem in more detail?

No other customer has ever complained
about this before. What is your
system configuration?

That sounds interesting. Tell me more...
```

#### `responses.txt` Format:
```
crash, crashes
Well, it never crashes on our system. It must have something
to do with your system. Tell me more about your configuration.

slow
I think this has to do with your hardware. Upgrading your processor
should solve all performance problems. Have you got a problem with
our software?

bug, buggy
Well, you know, all software has some bugs. But our software engineers
are working very hard to fix them. Can you describe the problem a bit
further?
```

## Key Features

### Robust Error Handling
- **File Not Found**: System continues with default fallback responses
- **I/O Errors**: Graceful error reporting without system crash
- **Blank Line Detection**: Custom exception for formatting validation
- **Empty Files**: Automatic fallback to hardcoded defaults

### Multiline Text Support
- **Preserves formatting** for default responses using newlines
- **Concatenates with spaces** for response map entries
- **Handles trailing content** after the last blank line separator

### Backward Compatibility
- System functions correctly even if external files are missing
- Maintains original functionality while adding file-based configuration
- No changes required to existing client code

## Testing Results

The implementation was thoroughly tested with:
- ✅ All keywords from `responses.txt` correctly mapped
- ✅ Multiline default responses properly loaded and formatted
- ✅ Exception handling for consecutive blank lines
- ✅ Graceful fallback when files are missing
- ✅ Unknown keywords properly handled with default responses

## Files Modified

### `Responder.java`
- Added `MultipleBlankLinesException` class
- Enhanced `fillDefaultResponses()` method for multiline file reading
- Completely rewrote `fillResponseMap()` to read from external file
- Improved error handling and edge case management

### External Files Used
- **`default.txt`**: Contains multiline default responses
- **`responses.txt`**: Contains keyword-response mappings in CSV format

## Technical Implementation Notes

### Memory Efficiency
- Uses `StringBuilder` for efficient string concatenation
- Reads files line by line to minimize memory footprint
- Proper resource management with try-with-resources

### Code Quality
- Comprehensive error handling for all failure scenarios
- Clear separation of concerns between file parsing and data storage
- Maintains existing public API for seamless integration

## Usage

The system works exactly as before from a user perspective:
1. Compile all Java files: `javac *.java`
2. Run the support system through `SupportSystem` class
3. Enter queries and receive automated responses
4. Type "bye" to exit the system

The enhanced system now reads all responses from external files, making it easy to update responses without recompiling the code.
