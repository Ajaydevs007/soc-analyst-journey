# INV-031 — Ransomware Obfuscation Technique

## Question

Now that you know the name of the ransomware's encryptor file, what obfuscation technique does it likely use?

## Answer

`Steganography`

## Related Artifact

`mhtr.jpg`

## Investigation Path

1. Q30 identified `mhtr.jpg` as the file containing the Cerber cryptor code.
2. Examine the file extension and apparent file type.
3. The payload uses a `.jpg` extension, making it appear to be an image.
4. The file is nevertheless associated with malicious Cerber cryptor code.
5. Hiding malicious data/code within an apparently legitimate image is characteristic of steganography.

## Conclusion

The likely obfuscation technique is:

`Steganography`

## Analyst Note

The use of a `.jpg` extension alone does not prove steganography. In the BOTS v1 scenario, the historical analysis associates `mhtr.jpg` with this technique.