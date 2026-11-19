# 🔓 PDF Password Cracking — Practical Lab

**Networkwalks Academy — Cybersecurity & Ethical Hacking Practical**
Date completed: November 18, 2026

A hands-on lab in auditing password-protected PDF files by extracting a crackable hash and running a dictionary (wordlist) attack against it — using both **John the Ripper** (via its **Johnny** GUI) and Networkwalks' online hash-extraction/cracking tools.

> ⚠️ **Ethical note:** This was performed in a controlled training environment against sample files provided for that purpose. Only crack passwords on files/systems you own or are explicitly authorized to test.

---

## 🧰 Tools & Environment

| Tool | Purpose |
|---|---|
| [John the Ripper (JtR)](https://www.openwall.com/john/) | Offline password-cracking engine |
| **Johnny** | GUI front-end for John the Ripper |
| `pdf2john` | Extracts a crackable hash from an encrypted PDF |
| Networkwalks PDF Hash Calculator | Online hash extraction (parses PDF locally in-browser) |
| Networkwalks Password Cracker | Online dictionary-attack tool |
| Microsoft Edge (PDF viewer) | Opening/verifying unlocked PDFs |

---

## 🧪 Methodology

### Part A — Cracking PDF1, PDF2, PDF3 with John the Ripper

1. Extracted the PDF's hash (`$pdf$4*4*128*...`) and loaded it into **Johnny**.
2. Ran a wordlist-based dictionary attack (**Start new attack**).
3. JtR returned a match:

   ```
   [+] MATCH password ✓ → good-luck
   ```

4. Used `good-luck` to unlock `My Locked PDF1.pdf`, `My Locked PDF2.pdf`, and `My Locked PDF3.pdf`.
5. Each file revealed a unique flag.

<p align="center">
  <img src="screenshots/01-johnny-cracked-goodluck.jpeg" width="420"/>
  <img src="screenshots/02-pdf-password-prompt.jpeg" width="420"/>
</p>

### Part B — Cracking the WK3-PM2 lab PDF with the Networkwalks online tools

1. Uploaded `My Locked PDF1 (for NW Tools Lab WK3-PM2).pdf` to the **Networkwalks PDF Hash Calculator** → extracted hash (PDF Revision R4, V4, 128-bit key).
2. Submitted the hash to the **Networkwalks Password Cracker**, which dictionary-attacked common passwords (`service`, `canada`, `hockey`, `killer`, `george`, `asdfgh`, `zxcvbn`, `qwertyuiop`, `111222`...).
3. Match found at attempt 91/100:

   ```
   [+] MATCH password1 ✓
   ```

4. Used `password1` to unlock the PDF and reveal its flag.

<p align="center">
  <img src="screenshots/06-hash-extractor-tool.jpeg" width="420"/>
  <img src="screenshots/07-online-cracker-password1.jpeg" width="420"/>
</p>

---

## 📊 Results

| # | File | Method | Password Recovered | Flag Captured |
|---|------|--------|---------------------|----------------|
| 1 | `My Locked PDF1.pdf` | John the Ripper (Johnny GUI) | `good-luck` | `nw{cybersecurity_flag_captured_2608}` |
| 2 | `My Locked PDF2.pdf` | John the Ripper (Johnny GUI) | `good-luck` | `nw{networkwalks_persistence_jtr_270521}` |
| 3 | `My Locked PDF3.pdf` | John the Ripper (Johnny GUI) | `good-luck` | `nw{networkwalks_flag_260821_1}` |
| 4 | `My Locked PDF1 (for NW Tools Lab WK3-PM2).pdf` | Networkwalks online hash extractor + cracker | `password1` | `nw{networkwalks_flag1_jtr_270521_1}` |



## 🏁 Flags Captured

```
nw{cybersecurity_flag_captured_2608}
nw{networkwalks_persistence_jtr_270521}
nw{networkwalks_flag_260821_1}
nw{networkwalks_flag1_jtr_270521_1}
```

---

## 💡 Key Takeaways

- Weak, dictionary-guessable passwords (`good-luck`, `password1`) remain one of the easiest ways past "encryption."
- Extracting a hash with `pdf2john` (or an equivalent online parser) doesn't require decrypting the file — only its encryption metadata.
- Dictionary/wordlist attacks succeed fast against predictable passwords; success depends on wordlist quality and patience.
- GUI tools like Johnny make John the Ripper more approachable for beginners learning password auditing.

## ✅ Conclusion

All four target PDFs were successfully audited and unlocked, capturing all four flags — demonstrating practical understanding of PDF hash extraction and dictionary-based password cracking with both John the Ripper and web-based alternatives.

---

*Lab completed as part of Networkwalks Academy's Cybersecurity & Ethical Hacking training.*
