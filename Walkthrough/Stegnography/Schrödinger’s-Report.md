# 📄 CTF Forensics – PDF Polyglot Analysis

## 🧩 Solution Walkthrough

---

## Step 1: Initial Inspection

After downloading **`company_report.pdf`**, the file was opened using a standard PDF viewer (Adobe Reader / Chrome).  
The document appeared to be a normal PDF containing generic text about a **“Confidential Report”**.

### Observations
- No visible hints or suspicious text  
- No hidden layers or interactive elements  
- Looks completely harmless at first glance  

This is a common tactic in **CTF forensics** challenges.

---

## Step 2: File Analysis

Next, we analyze the file from the terminal to understand its true structure.

### 1️⃣ Identify the file type
```bash
$ file company_report.pdf
company_report.pdf: PDF document, version 1.3
The file is identified as a valid PDF.

2️⃣ Scan for embedded data using Binwalk

In CTFs, it is standard practice to check for appended or embedded files.
We use binwalk to inspect the binary structure.

$ binwalk company_report.pdf


Output

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             PDF document, version 1.3
715           0x2CB           Zip archive data, at least v2.0 to extract
868           0x364           End of Zip archive, footer length: 22

Analysis

Offset 0: Valid PDF header (%PDF)

Offset 715: ZIP archive header detected

The ZIP archive contains a file named flag.txt

Conclusion:
The file is a Polyglot — a single binary that is valid as both a PDF and a ZIP file.

Step 3: Extraction

ZIP files store their Central Directory at the end of the file, which allows extraction even if extra data exists at the beginning.

We can directly unzip the PDF file:

$ unzip company_report.pdf


Output

Archive:  company_report.pdf
warning [company_report.pdf]: 715 extra bytes at beginning or within zipfile
(attempting to process anyway)
extracting: flag.txt


The warning is expected and can be safely ignored.

Alternative Method
$ binwalk -e company_report.pdf

Step 4: Flag Retrieval

Read the extracted file:

$ cat flag.txt


Output

SGCTF{p0lygl0ts_ar3_mUlt1_t4l3nt3d}
