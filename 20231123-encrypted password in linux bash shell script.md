Note: String followed by echo command ‘Demo123’ is the password string that we want to encrypt it and ‘This_is_not_a_Password’ is the password that is used during the encryption. If the openssl version is 1.1.0 or less then skip these two options ‘-pbkdf2 -iter 100000’
echo "YourPassword" | openssl enc -aes-256-cbc -md sha512 -a -pbkdf2 -iter 100000 -salt -pass pass:This_is_not_a_Password > .secret_vault.txt

#!/bin/bash
fusermount -u /home/fepoper/fintwin
KEY=$(cat .secret_vault.txt | openssl enc -aes-256-cbc -md sha512 -a -pbkdf2 -iter 100000 -d -salt -pass pass:'This_is_not_a_Password')
echo $KEY | sshfs -p 22 devops@1.1.1.1:/ /home/devops/ -o password_stdin

#!/bin/bash
FILE=/home/devops/Input/DEMO.zip
FILELOG=/scripts/file.log
KEY=$(cat .secret_vault.txt | openssl enc -aes-256-cbc -md sha512 -a -d -pbkdf2 -iter 100000 -salt -pass pass:'This_is_not_a_Password')
if test -f "$FILE"; then
    echo "$(date +"%Y-%m-%d %T") Check $FILE is exists, system will send it to DEMO" >> $FILELOG
      lftp  -u 829,$KEY devops.id.vn:4999 << EOF
        cd Input
        lcd /home/devops/Input
        mput *
        bye
EOF
     echo "$(date +"%Y-%m-%d %T") Remove $FILE" >> $FILELOG
     rm -rf $FILE
fi
