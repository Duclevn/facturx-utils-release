# Facturx-utils End-User Guide

This guide is for people who receive the desktop jar:

```text
facturx-utils-0.2.0.jar
```

Facturx-utils is a desktop app for opening, viewing, editing, validating, and saving Factur-X invoice files. It supports Factur-X PDF invoices and standalone XML-style invoice files such as `.xml`(cii and ubl).

## What's New In Version 0.2.0

- Check whether the seller and buyer electronic addresses are published as Peppol participants in the Production and Test networks.
- Inspect published Peppol document types and active Billing endpoints.
- Generate France CTC Flow 1 (`F1`) XML in the Base or Full profile from the current CII or UBL invoice.
- Review, edit, validate, and save converted UBL and generated France CTC F1 XML in dedicated tabs.
- Benefit from a clearer Summary layout, selectable result text, more responsive background processing, and assorted stability fixes.

## 1. What You Receive

The workbench is delivered as one runnable jar. That file contains the app plus the libraries it needs for PDF rendering, Factur-X extraction, XML editing, validation rules, CII-to-UBL conversion, and JavaFX desktop controls.

Because those libraries are bundled, the jar can be around 70-80 MB. This is normal for the full offline workbench. Users do not need Maven or the source code; they only need Java 21 or newer and the correct jar for their operating system and CPU architecture.

## 2. Install Java

Facturx-utils requires Java 21 or newer.

For most users, install the Oracle JDK. The JDK includes the Java runtime needed to run the app.

Java and JavaFX are related, but they are not the same thing. Since Java 11, JavaFX is no longer included in the standard JDK. This app's runnable jar is expected to include JavaFX, but JavaFX uses native files that must match the user's operating system and CPU architecture.

Recommended download page:

- Oracle Java downloads: https://www.oracle.com/java/technologies/downloads/

For Windows users:

1. Open the Oracle Java downloads page.
2. Select:
   - Java version: `21`
   - Operating System: `Windows`
   - Architecture: usually `x64`
3. Download the Windows `.msi` installer for the JDK.
4. Run the installer and keep the default options.
5. Open a new PowerShell or Command Prompt window and check:

```powershell
java -version
```

The command should print version `21` or newer. Java `21`, `22`, `23`, `24`, `25`, or newer is fine.

For macOS users:

1. Open Terminal.
2. Check the Mac CPU architecture:

```sh
uname -m
```

The result tells you which kind of Mac you have:

- `arm64`: Apple Silicon, such as M1, M2, M3, M4, or M5. Use a macOS Apple Silicon / AArch64 Java build and a jar built for macOS Apple Silicon.
- `x86_64`: Intel Mac. Use a macOS Intel / x64 Java build and a jar built for macOS Intel.

3. Download a macOS JDK 21 or newer for the matching architecture.
4. Open a new Terminal window and check:

```sh
java -version
```

## 3. Start The App

Put the jar in a folder you can find, for example `Downloads` or `Documents`.

Open PowerShell in that folder and run:

```powershell
java -jar .\facturx-utils-0.2.0.jar
```

If the jar is in another folder, use the full path:

```powershell
java -jar "C:\Users\YourName\Downloads\facturx-utils-0.2.0.jar"
```

Double-clicking the jar may not show useful error messages on some Windows machines, so PowerShell is recommended for the first launch.

On macOS, open Terminal in the folder containing the jar and run:

```sh
java -jar ./facturx-utils-0.2.0.jar
```

Important: because JavaFX uses operating-system-specific and CPU-specific files, use a jar built for the same platform as the user. For example, share the Windows x64 jar with Windows x64 users, the macOS Intel jar with Intel Mac users, and the macOS Apple Silicon jar with M-series Mac users.

## 4. Open An Invoice

1. Click `Open invoice`.
2. Choose one of these file types:
   - `.pdf` for a Factur-X PDF invoice
   - `.xml`
3. Wait for the invoice to load.

For a PDF invoice, the app shows:

- `Summary`: main invoice fields such as invoice number, seller, buyer, totals, profile, and VESID.
- `PDF`: a rendered preview of the PDF pages.
- `XML`: the embedded Factur-X XML that can be edited.
- `Visualize invoice`: a generated invoice preview from the XML.
- `Validation`: validation messages and warnings.

For a standalone XML invoice, the PDF tab is disabled and the XML/summary/validation views are used.

## 5. Read The Summary

Open the `Summary` tab to quickly inspect the invoice:

- Invoice number
- Type code
- Issue date
- Seller and buyer
- Currency
- Line count
- Tax total
- Grand total
- Due payable
- Guideline, syntax, profile, and VESID
- Seller and buyer Peppol electronic-address information

If the XML is edited and still well-formed, the summary updates from the edited XML.

## 6. View A PDF Invoice

Open the `PDF` tab.

Use:

- `<` and `>` to move between pages.
- `-` and `+` to zoom out and in.
- The scrollbars to move around the page when it is larger than the view.

The PDF page is centered in the view with padding around it. The PDF tab is only available when the opened invoice is a PDF.

## 7. Edit XML

Open the `XML` tab.

Available actions:

- Edit the XML directly in the editor.
- Click `Format XML` to pretty-print the XML.
- Press `Ctrl+F` to search in the XML.
- Use `Previous`, `Next`, and `Close` in the search bar.
- Click `Visualize` to generate a PDF preview from the current XML.
- Click `Convert to UBL` to convert a CII invoice XML into a UBL XML file.
- Use the editor scrollbars to move vertically or horizontally through long XML files.

When you edit the XML, the app marks the invoice as `Unsaved`.

## 8. Check Peppol Participant Status

The `Summary` tab shows a `Peppol Status` area for the seller and buyer. The app reads the standard electronic-address fields from the invoice and first checks locally whether each address has an eligible Peppol participant scheme.

To check publication status:

1. Open the `Summary` tab.
2. Review the detected seller and buyer electronic addresses and schemes.
3. Click `Check Participant Information`.
4. Review the separate `Production` and `Test` results.
5. Expand a result to inspect published document types and Billing endpoint details. Click `Refresh` to run the checks again.

Possible results include registered with Billing support, registered without a published Billing capability, not registered, and lookup failed. A missing address, missing or invalid `schemeID`, email address, or non-registrable scheme is explained without sending a lookup.

This is the only feature that requires internet access. It queries Peppol SML/SMP discovery services only when you click the button; opening and validating an invoice does not automatically perform a Peppol lookup. A result describes discovery metadata at the time of the check and is not a guarantee that a future document transmission will succeed.

## 9. Validate The Invoice

Choose a validation profile in the left sidebar:

- `Default`: general Factur-X / EN 16931 validation.
- `France CTC`: France CTC-focused validation.

Click `Validate`.

The app opens the `Validation` tab and shows issues with:

- Severity
- Layer
- Code
- Message
- Context

If there are no validation issues, the table reports an OK result.

For PDF invoices, validation checks the saved PDF. If the XML editor contains unsaved changes, the app also validates the edited XML and combines the results for display.

## 10. Visualize Edited XML

Open the `XML` tab and click `Visualize`.

The app generates a PDF preview from the current XML and opens the `Visualize invoice` tab.

Use:

- `<` and `>` to move between generated preview pages.
- `-` and `+` to zoom out and in.
- The scrollbars to move around pages larger than the view.

This preview helps you inspect how the XML invoice content could look as a readable document.

## 11. Convert CII To UBL

Open the `XML` tab and click `Convert to UBL`.

The button is enabled only when the opened invoice uses CII syntax. This can be:

- A standalone CII XML file, such as `.xml` or `.cii`.
- A Factur-X PDF whose embedded invoice XML is CII.

The app converts the current XML content and opens the result in a `Converted UBL` tab. The original invoice is not overwritten.

In the result tab, you can:

- Edit or format the generated XML.
- Validate it using the currently selected `Default` or `France CTC` validation profile.
- Click `Save` and choose where to write the UBL XML file.

The suggested output names follow this pattern:

- `invoice.xml` becomes `invoice.ubl.xml`.
- `invoice.cii` becomes `invoice.ubl.xml`.
- `invoice.pdf` becomes `invoice.ubl.xml`.

For France CTC compatibility, the app also cleans up converted UBL identifiers that have no `schemeID` in contexts where France CTC requires one. The app does not invent a scheme. Properly schemed identifiers are kept.

## 12. Generate France CTC Flow 1 XML

Open any supported CII or UBL invoice, then select `FR CTC F1` in the `XML` tab.

1. Choose `Base` or `Full`. `Base` is selected by default.
2. Click `Generate`.
3. Review the generated `France CTC F1 Base (...)` or `France CTC F1 Full (...)` tab.
4. Use `Format XML`, `Validate`, or edit the XML if needed.
5. Click `Save` and choose an output location.

The generator keeps the source syntax: CII input produces CII F1 XML, while UBL invoice or credit-note input produces UBL F1 XML. It extracts the fields required by the chosen France CTC F1 profile and applies the supported Flow 1 management mappings. Validation in the generated tab checks F1 completeness for the selected Base or Full profile.

The source invoice is not overwritten. The suggested filename is prefixed with the profile, for example `Base_invoice.xml` or `Full_invoice.xml`. Review validation findings before using the result in a production reporting workflow.

## 13. Save Changes

Click `Save invoice`.

The app asks for confirmation because saving overwrites the currently opened invoice file.

Important:

- The app does not create a backup copy.
- Make your own copy of the invoice before editing if you need to keep the original.
- For standalone XML files, the app validates the edited XML before overwriting the XML file.
- For PDF files, the app validates the edited XML, re-embeds it into the PDF, validates the rewritten PDF, and then replaces the original PDF.
- If validation finds fatal errors, the app shows those errors and asks whether you want to save the invalid XML anyway. Choose this only when you intentionally need an invalid file for testing.

After saving, the app reopens the saved file and shows the validation result.

## 14. Troubleshooting

### `java` is not recognized

Java is not installed or the terminal was opened before Java was installed.

Fix:

1. Install Java 21 or newer.
2. Close PowerShell or Command Prompt.
3. Open a new PowerShell or Command Prompt window.
4. Run:

```powershell
java -version
```

### The app does not start

Run it from PowerShell so you can see the error:

```powershell
java -jar .\facturx-utils-0.2.0.jar
```

Check that:

- Java version is `21` or newer.
- The jar file was fully downloaded.
- The jar was built for your operating system and CPU architecture.

### macOS JavaFX or `QuantumRenderer` crash

If the app crashes at startup and the Terminal output mentions JavaFX, native libraries, `QuantumRenderer`, or the graphics pipeline, the most likely cause is a mismatch between the jar's bundled JavaFX files and the Mac's architecture.

Check the Mac architecture:

```sh
uname -m
```

Then use the matching app jar and Java build:

- `arm64`: Apple Silicon / M-series Mac, including M1, M2, M3, M4, and M5. Use the macOS Apple Silicon / AArch64 jar and JDK.
- `x86_64`: Intel Mac. Use the macOS Intel / x64 jar and JDK.

Installing a JDK distribution that includes JavaFX can work around some JavaFX startup problems, but it is not a universal fix. The JavaFX runtime still needs to match the Mac architecture.

### The app cannot open a file

Check that the file is one of:

- `.pdf`
- `.xml`
- `.cii`
- `.ubl`

For PDF files, the PDF must contain embedded Factur-X XML.

### Save fails

Saving can fail when:

- The edited XML is not well-formed.
- The edited XML does not pass validation.
- The PDF cannot be rewritten safely.
- The file is open in another app.
- The folder is read-only.

Fix the reported issue, close other apps that may be using the invoice, and try again.

### Peppol lookup fails

Check your internet connection and whether a firewall, proxy, or DNS policy blocks Peppol discovery. A timeout or lookup failure does not mean the participant is unregistered; it means the app could not obtain a reliable result. Click `Refresh` to try again.

## 15. Safe Use Recommendation

Before editing a real customer or production invoice, make a copy of the file and test the workflow on the copy:

```powershell
Copy-Item .\invoice.pdf .\invoice-working-copy.pdf
```

Open and edit the working copy first. Only replace the original after you have validated the result.
