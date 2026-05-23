# Main Learning

This folder stores long-form human-readable learning documents for the Proteus generator project.

## Files

- `proteus_ai_generator_full_conversation_handoff.pdf.base64`

The GitHub connector used by ChatGPT can write UTF-8 text files directly. To preserve the PDF safely through that connector, the PDF is stored as Base64 text.

## How to restore the PDF

On Linux/macOS/Git Bash:

```bash
base64 -d "main learning/proteus_ai_generator_full_conversation_handoff.pdf.base64" > "main learning/proteus_ai_generator_full_conversation_handoff.pdf"
```

On PowerShell:

```powershell
[IO.File]::WriteAllBytes(
  "main learning/proteus_ai_generator_full_conversation_handoff.pdf",
  [Convert]::FromBase64String((Get-Content "main learning/proteus_ai_generator_full_conversation_handoff.pdf.base64" -Raw))
)
```
