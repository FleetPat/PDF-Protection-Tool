# PDF-Protection-Tool
Overview and Purpose
This script, protection.py, is a command-line utility built in Python. Its sole purpose is to take an existing, unencrypted PDF file and create a new, identical copy of it that is protected with a password. The resulting output file cannot be opened or viewed by anyone without first supplying the correct password.

It's designed for users who need a quick, scriptable way to secure sensitive documents from a terminal or command prompt, without needing to open a graphical PDF editing application. The script is non-destructive, meaning it never modifies or deletes the original input file; it only reads from it and generates a new, separate output file.

Core Functionality
The script's logic is split into two primary functions: main() and create_password_protected_pdf().

create_password_protected_pdf(input_pdf, output_pdf, password)
This function is the heart of the program and performs all the PDF manipulation.

Opening the Source PDF: It first attempts to open the input_pdf file path in binary read mode ('rb'). This is crucial for handling files like PDFs, which are not simple text.

Initializing Reader and Writer: It uses the PyPDF2 library to create two objects:

PyPDF2.PdfReader(pdf_file): This object reads the contents of the original PDF.

PyPDF2.PdfWriter(): This object is an in-memory, blank PDF that will be used to build the new, encrypted file.

Cloning the PDF: The script does not modify the original file in place. Instead, it "clones" it by iterating through every single page of the pdf_reader (from page 0 to the end). For each page, it calls pdf_writer.add_page() to add a copy of that page to the pdf_writer. This ensures the new file has the exact same content and page order as the original.

Applying Encryption: Once all pages have been copied into the pdf_writer object, the script executes the most critical command: pdf_writer.encrypt(password). This method applies standard PDF encryption to the entire document held in the writer's memory, using the password string provided by the user.

Saving the New File: Finally, the script opens the output_pdf file path in binary write mode ('wb') and calls pdf_writer.write() to save the encrypted, in-memory PDF data to disk.

Success Message: If all steps complete, it prints a confirmation message like "Password-protected PDF saved as output_file.pdf".

Command-Line Interface and Execution
The script is not meant to be run by double-clicking; it must be executed from a terminal.

main() Function
This function serves as the entry point and handles the user's command-line input.

Argument Validation: It uses sys.argv, which is a list containing the script's name followed by any arguments passed to it. The script checks if len(sys.argv) is exactly 4 (the script name, the input file, the output file, and the password).

Usage Instructions: If the user provides too few or too many arguments, the script prints a Usage: message explaining the correct format: python3 script.py <input_pdf> <output_pdf> <password>. It then exits with a non-zero status code (sys.exit(1)) to signal that an error occurred.

Variable Assignment: If the argument count is correct, it assigns the command-line arguments to variables:

sys.argv[1] becomes input_pdf

sys.argv[2] becomes output_pdf

sys.argv[3] becomes password

Execution: It then calls the create_password_protected_pdf() function, passing in these three variables to start the encryption process.

if __name__ == "__main__":
This is a standard Python construct that ensures the main() function is only called when the script is executed directly (e.g., python3 protection.py ...), and not when it's imported as a module into another Python script.

Error Handling
The script includes a robust try...except block to manage common problems gracefully instead of crashing.

FileNotFoundError: Catches cases where the specified input_pdf does not exist at the given path.

PyPDF2.utils.PdfReadError: Catches errors if the input_pdf is corrupted, not a valid PDF format, or is perhaps an empty file.

Exception as e: A general "catch-all" for any other unexpected errors, such as not having write permissions for the output_pdf location.

In all error cases, it will print a user-friendly error message to the console.

Dependencies
The script relies on one external library:

PyPDF2: This is the core library used for all PDF reading, writing, and encryption operations. A user must install this library (e.g., via pip install PyPDF2) before the script will function.

sys: This is a built-in Python library and requires no special installation.
