# Background

This directory contains some code to check compliance of proposals submitted to NASA [ROSES](https://science.nasa.gov/researchers/roses-blogs) calls.

Code author: Megan Ansdell [@mansdell](https://github.com/mansdell)

# Setup

### Required Non-standard Packages

[PyMuPDF](https://pymupdf.readthedocs.io/en/latest/): a useful package for importing PDF text (which confusingly is imported as "import fitz")

# Description
  
### check_dapr_single.py

This code reads in an anonymized proposal submitted to a ROSES program that follows Dual-Anonymous Peer Reivew (DAPR). It attempts to find the different sections of the proposal (STM, References, Budget) and then checks a variety of things to make sure it is DAPR compliant. 

The code requires two inputs:

1) Path to the anonymized proposal PDF (PDF_Anon_Path). This is *not* the full proposal generated by NSPIRES.
2) Path to a file with the team member information (Team_Info_Path). There are two options for this: a) CSV file with first names, last names, institutions, and cities (an example is provided in this repo) or b) the NSPIRES-generated cover pages with the team member info (usually the first 2-3 pages of the full, non-anonymized NSPIRES-generated proposal, depending on the team size).

The code outputs the following:

* Page ranges for the STM and References sections
  - These assume the following order: STM, References, Other (e.g., budget)
  - They're usually correct, but sometimes they're not. This only really matters for searching for the team member names: if the code got the references section wrong, it'll probably incorrectly flag the team member names as being in the main proposal text and/or miss DAPR violations in the budget section if the budget was improperly or not fully redacted.
  
* Reference format
  - DAPR proposals are supposed to use bracketed number references, rather than "et al." references
  - The code reports the number of brackets found in the proposal and number of "et al." usages in proposal (the former number should be high, the should be zero)
  
* Forbidden DAPR words
  - DAPR proposal shouldn't include any identifying team member information (names, institutions, cities, genders)
  - The code reports number of times such things are found and the page numbers on which they are found
  - Note that if you use the NSPIRES option for inputting team member names, cities are not included as that info is not in the NSPIRES cover pages.

### check_proposals.py

This code reads in a full, non-anonymized NSPIRES-generated PDF submitted to a NASA ROSES call, attempts to find the "Scientific / Technical / Management" section (hereafter "the proposal"), and then checks ROSES formatting requirements (font size, lines-per-inch, characters-per-inch). Please make sure to read the ROSES solicitation and NASA Guidebook for Proposers carefully, as formatting requirements may be different than those flagged below. 

The code requires one input, the full, non-anonymized NSPIRES-generated PDF. You'll want to change the "PDF_Full_Path" variable to the desired location and name of the NSPIRES-generated PDF before running the code.

The code outputs the following:

* PI name and proposal number
  - These are taken from the cover page of the NSPIRES-formatted PDF
  
* Font size 
  - The median font size used in the proposal is calculated and output to the terminal
  - A histogram of the font sizes is saved to the current directory (the gray horizontal line indicates ~12-point font size)
  
* Lines per inch (LPI) and counts per inch (CPI)
  - LPI is calculated per page and for pages with LPI > 5.5, the page number of the violation and the LPI value is provided.
  - CPI is calculated per line and the number of pages for which CPI > 16.0 is provided along with snippets of the line text
  - Note that PDF formats are weird and not inherently machine readable, so these calculations are not exact and results should be checked carefully. The limits for LPI and CPI used in the code are purposefully lenient compared to the current ROSES requirements for this reason, thus the code will only report blatant violations (or weird PDF formats that could not be read properly).
 
# Disclaimer

This is not an official NASA product. 
