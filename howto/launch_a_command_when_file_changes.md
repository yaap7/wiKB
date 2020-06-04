# How-To launch a command everytime a file changes

## Using inotify

Source: <https://superuser.com/questions/181517/how-to-execute-a-command-whenever-a-file-changes>

Prerequisite:

``` bash
sudo apt install inotify-tools
```

Example: copy a file to another server whenever it changes (e.g. for easy web development):

``` bash
while inotifywait -e close_write myfile.py; do scp ./myfile.py remote-server:/var/www/html/ ; done
```
