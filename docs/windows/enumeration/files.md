# Files
---

```powershell
# Search for useful files that might contains sensitive information.
Get-ChildItem -Path C:\Users\$username\ -Include *.txt,*.pdf,*.xls,*.xlsx,*.doc,*.docx -File -Recurse -ErrorAction SilentlyContinue

# Search for KDBX database.
Get-ChildItem -Path C:\ -Include *.kdbx -File -Recurse -ErrorAction SilentlyContinue
```