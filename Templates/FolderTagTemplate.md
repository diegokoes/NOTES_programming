<%*
// Get current folder name
const folder = tp.file.folder(true);
let output = "";
let folderName = "";

if (folder) {
    // Extract last part of folder path
    folderName = folder.split("/").pop();
    if (folderName) {
        // Format content
        output = `# ${folderName} > \n\n`;
    }
}

// If in root folder, just create separator
if (!folder || !output) {
    output = "\n\n- - -";
    folderName = "Note"; // Default name for root folder
}

tR += output;

// Rename ONLY if file is new/untitled (manual creation)
const currentTitle = tp.file.title;
const isManualCreation = /^Untitled( \d+)?$/.test(currentTitle);

if (folderName && isManualCreation) {
    await tp.file.rename(`${folderName}_`);
}

// Place cursor at beginning of file
await tp.file.cursor(0);
%>
