---
layout: post
title: "Encrypt 해독 스크립트"
date: 2024-03-22
tags: [해독, Encrypt, 스크립트]
categories: blog
---

## 사용자 입력:
> #!/bin/bash

echo -n "Enter the input to encrypt: "
read INPUT

ENCRYPTION_KEY="lia"
JASYPT_JAR_PATH="../lib/jasypt-1.9.3.jar"
ALGORITHM="PBEWithMD5AndDES"

java -cp "$JASYPT_JAR_PATH" org.jasypt.intf.cli.JasyptPBEStringEncryptionCLI input="$INPUT" password="$ENCRYPTION_KEY" algorithm="$ALGORITHM"
위 스크립트는 encrypt인데, decode하는 거 만들어줘.

## ChatGPT 응답:
> 아래는 주어진 Bash 스크립트를 기반으로 암호화된 문자열을 복호화하는 스크립트입니다.

```bash
#!/bin/bash

echo -n "Enter the input to decrypt: "
read INPUT

ENCRYPTION_KEY="lia"
JASYPT_JAR_PATH="../lib/jasypt-1.9.3.jar"
ALGORITHM="PBEWithMD5AndDES"

DECRYPTED_TEXT=$(java -cp "$JASYPT_JAR_PATH" org.jasypt.intf.cli.JasyptPBEStringDecryptionCLI input="$INPUT" password="$ENCRYPTION_KEY" algorithm="$ALGORITHM" | tail -n 1)
echo "Decrypted Text: $DECRYPTED_TEXT"
```

이 스크립트는 사용자로부터 복호화할 입력을 받은 후, Jasypt 라이브러리를 사용하여 복호화합니다. 마지막으로 복호화된 문자열을 출력합니다.

