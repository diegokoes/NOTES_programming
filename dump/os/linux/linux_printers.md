# LINUX > PRINTERS

## SUMMARY
> [!summary]
> CUPS printer management commands and troubleshooting

## THEORY

### CUPS (COMMON UNIX PRINTING SYSTEM)
CUPS is the printing system used by most Linux distributions. It provides a web interface and command-line tools for managing printers.

### COMMON COMMANDS

**Printer Status:**
```bash
# List all configured printers
lpstat -p

# Check print queue
lpstat -o

# Get detailed printer information
lpstat -l -p PRINTER_NAME
```

**Print Management:**
```bash
# Print a file
lp filename

# Set default printer
lpoptions -d PRINTER_NAME

# View printer configuration
lpoptions -p PRINTER_NAME
```

**Queue Management:**
```bash
# Cancel specific job
cancel JOB_ID

# Cancel all jobs for a printer
cancel -a PRINTER_NAME
```

**System Management:**
```bash
# Restart CUPS service
sudo systemctl restart cups

# View CUPS logs
cat /var/log/cups/error_log
```

### CUPS WEB INTERFACE
Access the web interface at: `http://localhost:631/jobs/`

### INK LEVEL MONITORING
Most printers don't support ink level reporting through Linux. Options include:
- `ippfind` (if supported)
- Manufacturer-specific tools (e.g., `hp-toolbox` for HP printers from HPLIP package)

## QUESTIONS

> [!tip]- How do I access the CUPS web interface?
> Open your browser and go to `http://localhost:631/jobs/`

> [!warning]- Why can't I see ink levels?
> Most printers don't support ink level reporting through Linux. You may need manufacturer-specific tools.

- - -
#linux

- Lists all configured printers and their statuses.
- Works on most distributions with CUPS installed.

## 2 **SETTING A DEFAULT PRINTER**

bash

Copy code

`lpoptions -d PRINTER_NAME`

- Replace `PRINTER_NAME` with the name of your printer.

## 3 **CHECKING THE PRINT QUEUE**

bash

Copy code

`lpstat -o`

- Shows the current print jobs in the queue.

## 4 **CANCELLING A PRINT JOB**

bash

Copy code

`cancel JOB_ID`

- Replace `JOB_ID` with the ID of the print job you want to cancel.
- Use `lpstat -o` to find the job ID.

## 5 **CANCELLING ALL PRINT JOBS**

bash

Copy code

`cancel -a PRINTER_NAME`

- Replace `PRINTER_NAME` with the name of the printer.
- This command may require root privileges (`sudo`).

## 6 **PRINTING A FILE**

bash

Copy code

`lp filename`

- Sends `filename` to the default printer.

## 7 **CHECKING PRINTER DETAILS**

bash

Copy code

`lpstat -l -p PRINTER_NAME`

- Provides detailed information about a specific printer.

## 8 **VIEWING PRINTER CONFIGURATION**

bash

Copy code

`lpoptions -p PRINTER_NAME`

- Displays the current settings of the specified printer.

## 9 **CHECKING PRINTER INK LEVELS**

- Not all printers support ink-level reporting through Linux. Some methods include:
    - **Using `ippfind` (if supported by your printer)**:
        
        bash
        
        Copy code
        
        `ippfind`
        
        Then use the found IPP URI to query details.
    - **Printer-specific tools**: Install the manufacturer's Linux utilities or drivers (e.g., `hp-toolbox` for HP printers from the **HPLIP** package).

## 10 **RESTARTING THE CUPS SERVICE**

bash

Copy code

`sudo systemctl restart cups`

- Restarts the CUPS daemon, which may resolve printer-related issues.

## 11 **VIEWING CUPS LOGS**

bash

Copy code

`cat /var/log/cups/error_log`

- Useful for debugging printer issues.

## 12 CUPS INTERFACE
1. Use the CUPS web interface: point your browser at `http://localhost:631/jobs/` and proceed from there