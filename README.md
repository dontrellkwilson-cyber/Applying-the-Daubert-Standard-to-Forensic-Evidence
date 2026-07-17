# Applying the Daubert Standard to Forensic Evidence

<p align="center">
  <strong>Digital Forensics Investigation, Evidence Handling, Hash Validation, and Cross-Tool Verification</strong>
</p>

---

## Objective

The objective of this lab was to process digital evidence in a manner that supports integrity, repeatability, proper evidence handling, and courtroom admissibility under the **Daubert Standard**.

The investigation included reviewing a search warrant, documenting the chain of custody, examining a seized drive image, recovering suspicious files, generating cryptographic hashes, and comparing results across multiple forensic tools.

## Skills Demonstrated

- Digital evidence handling and chain-of-custody documentation
- Search-warrant scope review
- Forensic drive-image examination
- Recovery of deleted files from NTFS unallocated space and the Recycle Bin
- Evidence extraction with FTK Imager
- MD5 and SHA1 hash generation
- Cross-tool hash validation
- Email evidence examination
- Autopsy case creation and analysis
- Paraben E3 evidence processing
- Command-line hash verification
- Documentation of forensic findings

## Lab Environment

| Component | Details |
|---|---|
| Workstation | `vWorkstation` |
| Operating System | Windows Server 2019 |
| Evidence Image | `Evidence_drive1.001` |
| Case Number | `10001-BPD-CCD` |
| Primary File System | NTFS |

## Tools Used

| Tool | Purpose |
|---|---|
| **FTK Imager** | Loaded the forensic image, reviewed files, recovered deleted evidence, exported files, and generated MD5/SHA1 hash reports |
| **Paraben E3** | Processed the same evidence and independently verified hash values |
| **Autopsy** | Examined the forensic image and reviewed hash information for selected evidence |
| **Adobe Acrobat Reader** | Reviewed the search warrant and completed the chain-of-custody form |
| **Command Prompt** | Verified the forensic image hashes from the command line |

## Investigation Scenario

In this lab, I acted as a digital forensics specialist assisting the Boston Police Department Cyber Crimes Division. A hard drive had been seized during an investigation involving Beverly Gates, an HR manager suspected of participating in a drug-trafficking operation.

My responsibilities were to:

1. Review the search warrant and remain within its authorized scope.
2. Complete the chain-of-custody documentation.
3. Examine the seized drive image without altering the original evidence.
4. Locate and extract potentially incriminating files.
5. Generate MD5 and SHA1 hashes.
6. Validate evidence integrity with independent forensic tools.

---

# Section 1: Hands-On Demonstration

## Part 1: Chain-of-Custody Procedures

I created a case folder named:

```text
10001-BPD-CCD
```

I reviewed the search warrant to identify the authorized search scope and then completed the evidence transfer section of the chain-of-custody form.

The transfer record documented:

| Field | Entry |
|---|---|
| Transferred From | Brendan O'Rourke |
| Transferred To | Dontrell Wilson |
| Evidence Storage | `vWorkstation` |
| Evidence Protection | Windows BitLocker Encryption |

Proper chain-of-custody documentation helps establish who possessed the evidence, where it was stored, and how it was protected throughout the investigation.

### Search Warrant

<p align="center">
  <img src="https://github.com/user-attachments/assets/6821c16b-239c-4e48-9a39-7985e5cb0ec5"
       alt="Active Directory security verification"
       width="750">
</p>

### Completed Chain-of-Custody Form

<p align="center">
  <img src="https://github.com/user-attachments/assets/7f400d80-b816-4647-899b-06cab17cf55a"
       alt="Active Directory security verification"
       width="550">
</p>

---

## Part 2: Evidence Extraction and Hash Generation with FTK Imager

### Loading the Forensic Image

I opened FTK Imager and added the first segment of the seized forensic image:

```text
C:\Daubert Standard Evidence\Image1\Evidence_drive1.001
```

The segmented image was loaded as an image-file evidence source for read-only examination.

<p align="center">
  <img src="https://github.com/user-attachments/assets/523359dc-ea1d-4388-9c84-b265ecde4247"
       alt="Active Directory security verification"
       width="650">
</p>

### Examining NTFS Unallocated Space

I navigated to:

```text
Evidence_drive1.001
└── NONAME [NTFS]
    └── [unallocated space]
```

NTFS unallocated space can contain deleted data that is no longer visible through the normal Windows file system. During the examination, I located suspicious file `0002665`, reviewed its contents, exported it, and created a hash report.

<p align="center">
  <img src="https://github.com/user-attachments/assets/23ebff8b-9612-4431-95fd-6a089dc25715"
       alt="Active Directory security verification"
       width="750">
</p>

### Hash Report for File 0002665

FTK Imager generated a CSV report containing the evidence filename, file location, MD5 value, and SHA1 value.

<p align="center">
  <img src="https://github.com/user-attachments/assets/336119fe-06fb-4cf2-af32-6d840904e9c4"
       alt="Active Directory security verification"
       width="750">
</p>

### Recovering Evidence from the Recycle Bin

I examined the following location:

```text
Evidence_drive1.001
└── NONAME [NTFS]
    └── [root]
        └── $RECYCLE.BIN
```

The Recycle Bin contained a Windows `$I` metadata file and its corresponding `$R` deleted-content file.

The metadata showed that the deleted file was originally named:

```text
MyRussianMafiaBuddies.txt
```

The file contained names and addresses that were potentially relevant to the investigation. I exported both Recycle Bin files and generated the `RecycleBinEvidence_hash.csv` report.

<p align="center">
  <img src="https://github.com/user-attachments/assets/e0884e08-5633-4c4f-b558-4b69b2e560eb"
       alt="Active Directory security verification"
       width="49%">
  <img src="https://github.com/user-attachments/assets/bf512a26-6c0f-4238-a9f0-f2e038a256d5"
       alt="Active Directory security verification"
       width="49%">
</p>

### Locating Original Copies of Deleted Files

I continued examining the root directory and located original copies of suspicious files inside the `Temp Folder`:

```text
MyRussianMafiaBuddies.txt
Nice Guys.png
```

I reviewed both files, exported them to the case folder, and generated separate FTK Imager hash reports.

<p align="center">
  <img src="https://github.com/user-attachments/assets/92a833a1-a90e-4a53-b73b-ab0e98e49853"
       alt="Active Directory security verification"
       width="750">
</p>

### FTK Imager Hash Reports

#### MyRussianMafiaBuddies.txt

<p align="center">
  <img src="https://github.com/user-attachments/assets/1b327a48-7020-444e-95cc-0d11587b65ed"
       alt="Active Directory security verification"
       width="750">
</p>

#### Nice Guys.png

<p align="center">
  <img src="https://github.com/user-attachments/assets/8aad3b34-56f1-4755-964f-df24038d6842"
       alt="Active Directory security verification"
       width="750">
</p>

---

## Part 3: Hash Verification with Paraben E3

I imported the same forensic image into Paraben E3 and located the previously identified evidence. I then compared the MD5 and SHA1 values generated by E3 with the values produced by FTK Imager.

### Verification Result

The MD5 and SHA1 values generated by E3 matched the values generated by FTK Imager for both incriminating files.

E3 displayed the hashes in uppercase while FTK Imager displayed them in lowercase. Letter capitalization does not change a hexadecimal hash value, so the results were identical.

This confirmed that:

- Both tools examined the same file contents.
- The evidence remained unchanged between examinations.
- The extraction and validation process produced repeatable results.
- The hash comparison supported the integrity of the evidence.

### MyRussianMafiaBuddies.txt Hashes in E3

<p align="center">
  <img src="https://github.com/user-attachments/assets/4b3ac30f-8e17-4044-9270-c89a2c42f005"
       alt="Active Directory security verification"
       width="750">
</p>

### Nice Guys.png Hashes in E3

<p align="center">
  <img src="https://github.com/user-attachments/assets/bb35a630-0744-4227-b9d8-c69a0121a43f"
       alt="Active Directory security verification"
       width="750">
</p>

---

# Section 2: Applied Learning

## Part 1: Suspicious Email Examination with FTK Imager

I reopened the forensic image and searched the suspect's files for email evidence. I located a suspicious `.eml` file, reviewed its contents, exported it, and generated hash values with FTK Imager.

I also modified a copy of the evidence file and generated a new hash. The original and modified files produced different values, demonstrating that even a small change to a file changes its cryptographic hash.

<p align="center">
  <img src="https://github.com/user-attachments/assets/32fd5ba9-18c4-4837-b5a7-4eb82430c9a6"
       alt="FTK Imager forensic evidence verification"
       width="750">
</p>

### FTK Imager Email Hash Comparison

| Evidence Object | MD5 |
|---|---|
| FTK Imager value 1 | `b0eca56883fd0a95d5a1885162b278ec` |
| FTK Imager value 2 | `a3daacbbf684478785cc3fef257fc727` |

Because the hashes differ, the two files do not contain identical data.

<p align="center">
  <img src="https://github.com/user-attachments/assets/73be6ed3-b8ff-4566-b80c-d386fb9fcb39"
       alt="FTK Imager forensic evidence verification"
       width="800">
</p>

---

## Part 2: Email Evidence Review with Autopsy

I created an Autopsy case, added `Evidence_drive1.001` as the data source, and reviewed the selected evidence in the Result Viewer.

Autopsy displayed the following MD5 value for the selected item:

```text
5b37cf5bae05a7770f904184bb7cdca
```

This value did not match the FTK Imager `.eml` hashes because the selected Autopsy result appeared to represent the extracted `Stardust` image attachment rather than the complete email file.

<p align="center">
  <img src="https://github.com/user-attachments/assets/60a74587-f07b-4cf7-8952-fd4b26ef57f9"
       alt="FTK Imager forensic evidence verification"
       width="800">
</p>

---

## Part 3: Email Hash Review with Paraben E3

Paraben E3 displayed the following MD5 value for the selected `.eml` evidence:

```text
57581DE091C9C9C01F259107F2BC2437
```

The value did not match the FTK Imager hash for the edited `.eml` file or the Autopsy value associated with the selected image attachment.

This comparison reinforced an important forensic principle: hashes should only be compared when the tools are hashing the exact same evidence object and the exact same file contents.

<p align="center">
  <img src="https://github.com/user-attachments/assets/20fc2377-f586-4b05-8c30-ddd0b5c0421e"
       alt="FTK Imager forensic evidence verification"
       width="800">
</p>

---

# Section 3: Challenge and Analysis

## Part 1: Command-Line Hash Verification

I used the FTK Imager command-line utility to generate MD5 and SHA1 values for:

```text
Evidence_drive1.001
```

The command-line results provided an additional method for confirming the integrity of the forensic image outside the graphical interface.

> Replace the placeholder below with the exact command used in the lab.

```cmd
REM Enter the FTK Imager command used to hash Evidence_drive1.001
```

<p align="center">
  <img width="900" alt="Command-line MD5 and SHA1 verification for Evidence_drive1.001" src="PASTE-COMMAND-LINE-HASH-IMAGE-URL-HERE" />
</p>

---

## Part 2: Locating Additional Evidence

I examined additional deleted files and identified their original filenames and locations.

| Recycle Bin File | Original Filename | Original File Path |
|---|---|---|
| `$R354ELH.xlsx` | `DrugSales.xlsx` | `G:\VIP Info\2021\DrugSales.xlsx` |
| `$RBOE0TL.doc` | `manual-testing-fresher-resume-1.doc` | `G:\Students\manual-testing-fresher-resume-1.doc` |
| `$RX3177E.pdf` | `hr letter for visa.pdf` | `G:\Work Doc\hr letter for visa.pdf` |

<p align="center">
  <img width="900" alt="Additional deleted evidence and original file paths" src="PASTE-ADDITIONAL-EVIDENCE-IMAGE-URL-HERE" />
</p>

---

# Key Findings

- The search warrant and chain-of-custody form established the legal and procedural boundaries for handling the evidence.
- FTK Imager located suspicious data in NTFS unallocated space, the Recycle Bin, and the `Temp Folder`.
- Deleted-file metadata helped identify original filenames and locations.
- MD5 and SHA1 values from FTK Imager and Paraben E3 matched when the exact same files were examined.
- Modified files produced different hash values.
- Autopsy and E3 comparisons showed the importance of confirming that each tool is hashing the same evidence object before interpreting a mismatch.
- Command-line hashing provided another repeatable method for validating the forensic image.

# Relationship to the Daubert Standard

This lab demonstrated several practices that support the reliability of digital forensic evidence:

- **Testing and repeatability:** Hash values were generated and compared using multiple tools.
- **Known methodology:** Standard forensic applications and cryptographic hashing methods were used.
- **Standards and controls:** Evidence handling was documented through a chain-of-custody form.
- **Integrity verification:** Matching hashes demonstrated that evidence had not changed.
- **Reproducibility:** Another examiner using the same evidence and method should be able to reproduce the same hash results.

# Conclusion

This investigation demonstrated a complete digital evidence workflow, beginning with legal authorization and chain-of-custody documentation and continuing through evidence recovery, extraction, hashing, cross-tool validation, and reporting.

The lab showed that forensic findings must be supported by repeatable methods and careful documentation. Matching hashes can support evidence integrity, while mismatched hashes require the examiner to determine whether the file was modified or whether different evidence objects were selected.

---

## Repository Contents

```text
.
├── README.md
├── Images/
│   ├── search-warrant.png
│   ├── chain-of-custody.png
│   ├── ftk-evidence-image.png
│   ├── unallocated-space-evidence.png
│   ├── recycle-bin-evidence.png
│   ├── ftk-hash-reports.png
│   ├── e3-hash-validation.png
│   ├── suspicious-email.png
│   ├── autopsy-hash.png
│   ├── e3-email-hash.png
│   ├── command-line-hashes.png
│   └── additional-evidence.png
└── Lab-Documentation/
    └── Applying-the-Daubert-Standard-to-Forensic-Evidence.pdf
```

## Lab Documentation

A complete version of the lab report can be included in the `Lab-Documentation` folder.

```markdown
[View the Complete Lab Report](Lab-Documentation/Applying-the-Daubert-Standard-to-Forensic-Evidence.pdf)
```

---

> **Note:** This repository documents work completed in an authorized educational lab environment. The names, evidence, and investigative scenario are fictional and are used only for cybersecurity training.
