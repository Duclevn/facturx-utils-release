# Facturx-utils

Facturx-utils is a desktop application for inspecting, editing, validating, converting, and saving electronic invoices. It works with Factur-X PDF invoices as well as standalone CII and UBL invoice files.

All invoice processing takes place locally on your computer.

## Features

- Open Factur-X PDFs and standalone `.xml`, `.cii`, and `.ubl` invoices.
- Preview every page of an opened PDF with page navigation and zoom controls.
- View and edit the embedded or standalone invoice XML.
- See a readable summary containing the invoice number, dates, seller, buyer, currency, totals, syntax, and profile.
- Format XML and search within it using `Ctrl+F`.
- Validate invoices against the detected Factur-X, ZUGFeRD, EN 16931, XRechnung, Peppol, or France CTC rules.
- Generate a readable PDF preview from the current XML.
- Convert CII invoice XML to UBL.
- Re-embed edited XML into a Factur-X PDF.

## Requirements

- Java 21 or newer
- A release compatible with your operating system and CPU architecture

Check your installed Java version in PowerShell, Command Prompt, or Terminal:

```text
java -version
```

If Java is not installed, download a Java 21 or newer JDK from the [Oracle Java Downloads](https://www.oracle.com/java/technologies/downloads/) page.

## Download and start

1. Open this repository's **Releases** page.
2. Download the appropriate application JAR.
3. Put the downloaded JAR in a folder you can easily find.
4. Open PowerShell, Command Prompt, or Terminal in that folder.
5. Start the application:

```text
java -jar facturx-utils-<version>.jar
```

Replace `<version>` with the version in the downloaded filename.

If several downloads are offered, choose the one matching your system. A universal release supports Windows x64, Linux x64/ARM64, and macOS Intel/Apple Silicon.

## User guide

### Open an invoice

1. Select **Open invoice**.
2. Choose a `.pdf`, `.xml`, `.cii`, or `.ubl` file.
3. Wait for the invoice to load.

For a Factur-X PDF, the PDF preview and embedded XML are displayed. For a standalone XML invoice, the XML, summary, and validation views are available without a PDF preview.

### Inspect an invoice

Use the tabs in the workbench:

- **Summary** shows the main invoice details, parties, totals, syntax, profile, and validation identifier.
- **PDF** shows the original PDF pages. Use `<` and `>` to change pages and `-` and `+` to zoom.
- **XML** shows the editable embedded or standalone invoice XML.
- **Visualize invoice** shows a generated PDF preview of the current XML.
- **Validation** lists validation findings with their severity, rule layer, code, message, and context.

### Edit XML

1. Open the **XML** tab.
2. Edit the XML directly in the editor.
3. Select **Format XML** to make well-formed XML easier to read.
4. Press `Ctrl+F` to search, then use **Previous** and **Next** to move between matches.

The application marks the invoice as **Unsaved** after a change. The summary refreshes whenever the edited XML remains well-formed.

### Validate an invoice

1. Choose a validation profile:
   - **Default** for normal invoice validation.
   - **France CTC** for France CTC-specific rules.
2. Select **Validate**.
3. Review the findings in the **Validation** tab.

For an opened PDF, the application validates the PDF and its invoice data. Unsaved XML edits are also validated and included in the displayed results.

### Visualize XML

From the **XML** tab, select **Visualize**. The application creates a readable PDF preview from the current XML and opens it in the **Visualize invoice** tab.

This preview is for inspection; it does not replace the original invoice PDF.

### Convert CII to UBL

Open a CII invoice and select **Convert to UBL** in the **XML** tab. The converted file is written beside the opened invoice:

```text
invoice.xml  ->  invoice.ubl.xml
invoice.cii  ->  invoice.ubl.xml
invoice.pdf  ->  invoice.ubl.xml
```

If that filename already exists, the application creates a new name such as `invoice.ubl-2.xml`. The opened invoice is not overwritten by conversion.

### Save changes

Select **Save invoice** and confirm the overwrite warning.

- A standalone XML invoice is validated and then written back to the opened file.
- For a Factur-X PDF, the edited XML is validated, re-embedded, and the rewritten PDF is validated before it replaces the opened file.
- If fatal validation errors are found, the application warns you before allowing an invalid invoice to be saved.

> **Important:** Saving overwrites the opened invoice and does not create a backup. Make a copy before editing any production or customer invoice.

After saving, the application reopens the saved invoice and displays its validation result.

## Troubleshooting

### `java` is not recognized

Install Java 21 or newer, close the terminal, open a new one, and run `java -version` again.

### The application does not start

Launch it from a terminal with `java -jar ...` so that any error is visible. Confirm that:

- Java 21 or newer is installed.
- The download completed successfully.
- The JAR matches your operating system and CPU architecture.

### A PDF cannot be opened

The PDF must be a Factur-X PDF containing embedded invoice XML. Ordinary PDF invoices without embedded Factur-X data are not supported.

### Saving fails

Check that the XML is well-formed, close any other application using the invoice, and verify that you can write to its folder. Review the validation messages for the specific problem.

### macOS reports a JavaFX or `QuantumRenderer` error

Use matching architectures for Java, the application JAR, and your Mac:

- Apple Silicon Macs require ARM64/AArch64 builds.
- Intel Macs require x64 builds.
