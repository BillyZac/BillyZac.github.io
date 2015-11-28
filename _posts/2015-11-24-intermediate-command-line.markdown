## Monitoring Your System
### ps
ps -ax | grep Chrome

pretty ugly

### top
top six processes using the CPU

### htop
$ brew install htop

Dandy's go to
much prettier
use F6 to sort by categories

## fortune
$ brew install fortune
random quote

### cowsay
$ fortune | cowsay

### alias
alias sheepsay='cowsay -f sheep'
alias git commands:
- alias a="atom ."

## BASH Scripting
touch helloworld.sh


\#!/bin/bash
// defines the executables environment
echo "What is your name?"
read NAME
echo $NAME

### change permissoins
$ chmod +x helloworld.sh

### export to json file:
echo "{ name: $NAME, age: $AGE, favoritecolor: $COLOR }" > $NAME.json

### More Bash
http://tldp.org/HOWTO/Bash-Prog-Intro-HOWTO.html

