# DoD Cyber Sentinel One 2025 CTF: Remote-Help Challenge Write-Up
[Full Article](https://medium.com/@rameses.p.jr/dod-cyber-sentinel-one-2025-ctf-write-up-remote-help-62db9e2dd7af)

Challenge Overview:
Engaged in a Capture The Flag (CTF) challenge from the DoD Cyber Sentinel program, which involved analyzing a suspicious shell script to uncover hidden flags. The task required skills in reverse engineering, malware analysis, and system behavior interpretation.

Analysis Steps:

Initial Script Examination:
The provided GitHub repository contained a .sh file and a test image. Upon reviewing the script, it was observed that certain lines were incomplete, ending abruptly. These fragments were followed by a | base64 -d command, indicating base64-encoded content. Decoding this segment revealed readable code.

System Reconnaissance and Data Exfiltration:
The decoded script performed system reconnaissance and attempted data exfiltration. It contained intentional errors to prevent accidental execution. The script sent data to msoidentity.com/log, where the first part of the flag was retrieved.

Base32 Decoding and Further Analysis:
Further examination revealed sections encoded in base32. Accessing msoidentity.com/info provided a file containing a second payload. Decoding this payload established persistence mechanisms on the system. The OpenSSL password used appeared to be in hexadecimal format; decoding it revealed the third part of the flag.

Decrypting Encrypted Backup:
The script created an encrypted file, backup.enc, using the decoded password. Decrypting this file with the following OpenSSL command:

openssl enc -d -aes-256-cbc -nosalt -in ./backup.enc -out ./backup.sh -pass pass:45337a3067335f56475f


yielded another payload. This payload disguised itself as a system process while continuing data exfiltration. It communicated back to msoidentity.com on port 4443, where the final part of the flag was obtained.

Final Flag Retrieval:
The second part of the flag was not found within the code or associated files. Upon revisiting the GitHub repository and using the git show command, the final part of the flag was discovered.
