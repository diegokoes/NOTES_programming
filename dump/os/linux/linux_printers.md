## 1 **Viewing Available Printers**

bash

Copy code

`lpstat -p`

- Lists all configured printers and their statuses.
- Works on most distributions with CUPS installed.

## 2 **Setting a Default Printer**

bash

Copy code

`lpoptions -d PRINTER_NAME`

- Replace `PRINTER_NAME` with the name of your printer.

## 3 **Checking the Print Queue**

bash

Copy code

`lpstat -o`

- Shows the current print jobs in the queue.

## 4 **Cancelling a Print Job**

bash

Copy code

`cancel JOB_ID`

- Replace `JOB_ID` with the ID of the print job you want to cancel.
- Use `lpstat -o` to find the job ID.

## 5 **Cancelling All Print Jobs**

bash

Copy code

`cancel -a PRINTER_NAME`

- Replace `PRINTER_NAME` with the name of the printer.
- This command may require root privileges (`sudo`).

## 6 **Printing a File**

bash

Copy code

`lp filename`

- Sends `filename` to the default printer.

## 7 **Checking Printer Details**

bash

Copy code

`lpstat -l -p PRINTER_NAME`

- Provides detailed information about a specific printer.

## 8 **Viewing Printer Configuration**

bash

Copy code

`lpoptions -p PRINTER_NAME`

- Displays the current settings of the specified printer.

## 9 **Checking Printer Ink Levels**

- Not all printers support ink-level reporting through Linux. Some methods include:
    - **Using `ippfind` (if supported by your printer)**:
        
        bash
        
        Copy code
        
        `ippfind`
        
        Then use the found IPP URI to query details.
    - **Printer-specific tools**: Install the manufacturer's Linux utilities or drivers (e.g., `hp-toolbox` for HP printers from the **HPLIP** package).

## 10 **Restarting the CUPS Service**

bash

Copy code

`sudo systemctl restart cups`

- Restarts the CUPS daemon, which may resolve printer-related issues.

## 11 **Viewing CUPS Logs**

bash

Copy code

`cat /var/log/cups/error_log`

- Useful for debugging printer issues.

## 12 Cups interface
1. Use the CUPS web interface: point your browser at `http://localhost:631/jobs/` and proceed from there