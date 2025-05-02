### Customize terminal
Background: \#2d2a2e
Font: \#FFFFFF

```bash
# This is to change the directories color when we use ls
nano .bashrc

# Then add this line to the end of file
LS_COLORS=$LS_COLORS:'di=0;37:' ; export LS_COLORS # you can change di= variable for colors

# Reload file
source ~/.bashrc
```
