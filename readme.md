# Facturx-utils 0.4.0 End-User Guide

## Table Of Contents

- [What's New In 0.4.0](#whats-new-in-040)

**Getting started**

1. [What You Receive](#1-what-you-receive)
2. [Install Java](#2-install-java)
3. [Start The App](#3-start-the-app)
4. [Open An Invoice](#4-open-an-invoice)

**Using the app**

5. [Read The Summary](#5-read-the-summary)
6. [View A PDF Invoice](#6-view-a-pdf-invoice)
7. [Edit XML](#7-edit-xml)
8. [Check Peppol Participant Information](#8-check-peppol-participant-information)
9. [Validate The Invoice](#9-validate-the-invoice)
10. [Visualize Edited XML](#10-visualize-edited-xml)
11. [Convert Between UBL And CII](#11-convert-between-ubl-and-cii)
12. [Generate France CTC Flow 1 XML](#12-generate-france-ctc-flow-1-xml)
13. [Save Changes](#13-save-changes)

**Help and safe use**

14. [Troubleshooting](#14-troubleshooting)
15. [Safe Use Recommendation](#15-safe-use-recommendation)

## What's New In 0.4.0

This guide applies to the `facturx-utils-0.4.0.jar` desktop app.

- Convert UBL Invoice/CreditNote to CII and CII/Factur-X XML to UBL from `Conversion`.
- Generate Base or Full France CTC Flow 1 XML while retaining CII or UBL syntax.
- Check Peppol participant publication and exact invoice-type support separately in Production and Test.
- Use the dedicated two-layer France CTC validation route for Flux 2/CDAR documents.
- Review, edit, and save converted or generated XML in separate tabs without overwriting the source invoice.

## 1. What You Receive

The workbench is delivered as one runnable jar. That file contains the app plus the libraries it needs for PDF rendering, Factur-X extraction, XML editing, validation rules, UBL/CII conversion in both directions, and JavaFX desktop controls.

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
java -jar .\facturx-utils-0.4.0.jar
```

## 4. Open An Invoice

1. Click `Open invoice`.
2. Choose one of these file types:
   - `.pdf` for a Factur-X PDF invoice
   - `.xml` for a standalone invoice using CII or UBL syntax
3. Wait for the invoice to load.

For a PDF invoice, the app shows:

- `Summary`: main invoice fields such as invoice number, seller, buyer, totals, profile, and VESID.
- `PDF`: a rendered preview of the PDF pages.
- `XML`: the embedded Factur-X XML that can be edited.
- `Conversion`: UBL-to-CII, CII-to-UBL, and XML-to-PDF actions for the current editor content.
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
- Container format, XML syntax, guideline, profile, and VESID
- Seller and buyer Peppol electronic-address information

For a hybrid PDF, the format is detected from Factur-X/ZUGFeRD XMP metadata and the standard embedded XML filename. This distinction is important for EN 16931 because its BT-24 value can be identical in generic CII and Factur-X. If the XML is edited and still well-formed, the summary updates while retaining the opened PDF's container context.

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
- Click `Save as...` to write the current invoice to a new PDF or XML file without first choosing the overwrite/save-copy dialog.
- Use the editor scrollbars to move vertically or horizontally through long XML files.

When you edit the XML, the app marks the invoice as `Unsaved`.

## 8. Check Peppol Participant Information

The `Summary` tab shows a `Peppol participant information` area for the seller and buyer. The app reads only the standard electronic-address fields from the invoice and first checks locally whether each address has an eligible Peppol participant scheme. It does not substitute a tax ID, legal ID, generic party ID, or email address.

To check publication status:

1. Open the `Summary` tab.
2. Review the detected seller and buyer electronic addresses and schemes.
3. Click `Check Participant Information`.
4. Review the separate `Production` and `Test` results.
5. Review the invoice-type badge in each Production and Test row, then expand a result to inspect the discovery details. These can include the SML DNS zone, SMP URL, a single active Billing Invoice access-point URL, published document types, and optional Business Card company names, country, and registration date when the SMP provides them. Click `Refresh` to bypass the short-lived cache and run the checks again.

Production and Test have independent badges: `✔ Invoice type supported`, `⚠ Invoice type not supported`, or `❔ Invoice type unknown`. The comparison uses the invoice's exact Peppol document type identifier (syntax, Invoice/CreditNote type, customization/profile, and syntax version) against the document types published in that environment. Test support does not imply Production support.

Registration and invoice-type support are separate results. A participant can be registered while not publishing the exact document type required by the open invoice. Possible registration results are registered, not registered, and lookup failed. A missing address, missing or invalid `schemeID`, email address, or non-registrable scheme is explained without sending a lookup.

Peppol lookup is the only area that requires internet access. It queries Peppol SML/SMP discovery services only when you explicitly run a lookup; opening and validating an invoice do not automatically contact the network. A result describes discovery metadata at the time of the check and is not a guarantee that a future document transmission will succeed.

### Look up any Peppol participant

Use `Utilities` > `Peppol lookup` in the left sidebar when the participant is not taken from the open invoice.

1. Enter an identifier such as `0225:152857363`. The full `iso6523-actorid-upis::0225:152857363` form is also accepted.
2. Click `Look up participant` or press Enter.
3. Review the separate Production and Test results below the search box.
4. Expand the environment details to inspect its SML/SMP source, published document types, available Billing Invoice access-point URL, and optional Business Card.

The identifier is checked locally before any request is sent. Each lookup is user initiated, Production and Test stay independent, and a network failure remains distinct from a participant that is not registered.

## 9. Validate The Invoice

Choose a validation profile in the left sidebar:

- `Default`: general Factur-X / EN 16931 validation.
- `France CTC`: first validates the exact detected invoice schema/profile VESID, then runs the legacy 1.3.1 and latest bundled France CTC rule sets for CII/UBL EN 16931, CII/UBL Extended, UBL CreditNote, Factur-X, or CDAR. The legacy set is skipped if it is not bundled.

Click `Validate`.

For a saved Factur-X PDF, the default validation covers the complete hybrid document: Kaltblut checks the Factur-X/ZUGFeRD BR-HYBRID container and XMP rules, veraPDF checks PDF/A-3 conformance, and DDD/phive check the embedded XML schema and business rules. PDF/A findings include the veraPDF rule and PDF object location. Container and XMP checks also run when `France CTC` is selected, alongside that profile's XML validation route.

Before validation is run, the `Validation` tab says `Not validated yet`; opening an invoice does not imply that it passed. A `Compliance & format` strip repeats the detected format, XML syntax, profile, guideline, and VESID from Summary for quick reference before validation.

After validation, the tab shows an overall result and error, warning, and check/information counts. The headline identifies the validation set responsible for a failure when only one set has blocking errors. Results are grouped into user-facing sets such as invoice profile rules, France CTC, and the Factur-X PDF container; engine names such as PHIVE and Kaltblut remain available in technical details.

The table shows actionable errors and warnings, while passed checks and profile-detection messages remain summarized above. Select an issue to inspect its rule ID, description, element XPath, and expected or allowed values when the validator references a bundled code list. Expand `Technical details` for the validator test, engine/layer, rule artefact, source document, and Schematron role when supplied. The technical validator test is the XPath/Schematron condition evaluated by the validation engine; it is diagnostic logic, not usually a value to paste into the invoice.

- Severity
- Validation set
- Rule code
- Message
- Element/context, XPath test, source, and artefact in the detail panel

If there are no validation issues, the table reports an OK result. The validation table labels detected schema/profile findings as `PHIVE`, France CTC 1.3.1 findings as `FR CTC old`, and findings from the latest bundled France CTC version as `FR CTC`. Both France CTC versions run when 1.3.1 is available in Helger phive; if a future dependency release removes it, only the latest set runs. Warnings remain non-blocking; validator execution failures and error/fatal assertions block strict saving.

If the XML editor contains unsaved changes, validation checks the edited XML because those bytes are not yet embedded in the saved PDF. Save and reopen the PDF to validate the updated hybrid container and XMP metadata.

## 10. Visualize Edited XML

Open the `XML` tab and click `Visualize`.

The app generates a PDF preview from the current XML and opens the `Visualize invoice` tab.

Use:

- `<` and `>` to move between generated preview pages.
- `-` and `+` to zoom out and in.
- `Save as...` to export the generated PDF visualization.
- The scrollbars to move around pages larger than the view.

This preview helps you inspect how the XML invoice content could look as a readable document.

## 11. Convert Between UBL And CII

Select `Conversion` in the left sidebar. The screen uses the current XML editor content, including unsaved edits.

- Click `UBL to CII` for a UBL 2.1 Invoice or CreditNote. The app opens a `Converted CII` tab.
- Click `CII to UBL` for CII / Factur-X XML. The app opens a `Converted UBL` tab.
- Click `XML to PDF` to open a generated preview in the `Visualize invoice` tab.
- Click `FR CTC F1` to generate a France CTC Flow 1 XML document from supported CII or UBL input.

Each syntax-conversion button is enabled only when the current XML has the matching input syntax. CII input can be:

- A standalone `.xml` file using CII syntax.
- A Factur-X PDF whose embedded invoice XML is CII.

UBL input is a standalone `.xml` UBL Invoice or CreditNote. The original invoice is not overwritten.

In the result tab, you can:

- Edit or format the generated XML.
- Click `Save as...` and choose where to write the CII or UBL XML file.

The suggested output names follow this pattern:

- UBL `invoice.xml` becomes `invoice.cii.xml`.
- UBL `invoice.ubl.xml` becomes `invoice.cii.xml`.
- CII `invoice.xml` becomes `invoice.ubl.xml`.
- CII `invoice.pdf` becomes `invoice.ubl.xml`.

In these names, `cii` or `ubl` labels the generated XML syntax; the file extension remains `.xml`.

For France CTC compatibility, the app also cleans up converted UBL identifiers that have no `schemeID` in contexts where France CTC requires one. The app does not invent a scheme. Properly schemed identifiers are kept.

## 12. Generate France CTC Flow 1 XML

Open any supported CII or UBL invoice, then select `Conversion` and click `FR CTC F1`.

1. Choose `Base` or `Full`. `Base` is selected by default.
2. Click `Generate`.
3. Review the generated `France CTC F1 Base (...)` or `France CTC F1 Full (...)` tab.
4. Use `Format XML` or edit the XML if needed.
5. Click `Save as...` and choose an output location.

The generator keeps the source syntax: CII input produces CII F1 XML, while UBL invoice or credit-note input produces UBL F1 XML. It follows the XP Z12-012 v1.4 field subset and mapping rules. Ordinary invoices open one generated tab. B8/S8/M8 multi-seller invoices open one tab per unit-invoice number, and B9/S9/M9 bidirectional invoices open the two required directions.

The source invoice is not overwritten. The suggested filename is prefixed with the profile; split outputs also include the unit-invoice identifier.

## 13. Save Changes

Click `Save changes`.

The app shows the complete source path and lets you either overwrite the opened invoice or use `Save a copy...`. Saving a copy stages the new file before replacing the selected destination, so the original invoice remains unchanged.

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
java -jar .\facturx-utils-0.4.0.jar
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
- `.xml` using CII or UBL syntax

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
