---
title : "Installing the AWS Copilot CLI"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 3.1 </b> "
---

#### Download binary

1. For Linux x86 (64-bit) systems:

```
sudo curl -Lo /usr/local/bin/copilot https://github.com/aws/copilot-cli/releases/latest/download/copilot-linux \
   && sudo chmod +x /usr/local/bin/copilot \
   && copilot --help
```

![Create Docker](/images/7/1.png?featherlight=false&width=90pc)

2. (Optional) Verify the manually installed AWS Copilot CLI using PGP signatures

```
sudo yum install gnupg2 --allowerasing
```

![Create Docker](/images/7/3.png?featherlight=false&width=90pc)

3. Verify the GnuPG path is added to your environment path.

```
echo $PATH
```

![Create Docker](/images/7/4.png?featherlight=false&width=90pc)

4. Create a local plain text file.

```
touch public_key_filename.txt
```

5. Add the following contents of the Amazon ECS PGP public key and save the file. 

```
gpg --gen-key
```

```
gpg --export ecs | base64
```

6. Download the AWS Copilot CLI signatures. 

```
sudo curl -Lo copilot.asc https://github.com/aws/copilot-cli/releases/latest/download/copilot-linux.asc
```

![Create Docker](/images/7/5.png?featherlight=false&width=90pc)


