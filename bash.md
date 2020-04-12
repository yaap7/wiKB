# bash

## script stub

``` bash
#!/bin/bash

function show_usage() {
    echo "Usage: $0 -e|-s [-o outputdir] file.txt"
    echo '  Mandatory argument:'
    echo '    -e, --extract  : fill me in'
    echo '    -s, --stats    : blabla'
    echo '  Optionnal argument:'
    echo '    -o, --output output_directory  : extract data into this directory'
    exit 1
}

function debug() {
  [[ "x$DEBUG" == "x1" ]] && echo "$@"
  echo -n ''
}

function info() {
  echo -e "\e[34mInfo\e[0m: $@"
}

function success() {
  echo -e "\e[32mSuccess\e[0m: $@"
}

function error() {
  echo -e "\e[31mError\e[0m: $@" >&2
}

# configuration
DEBUG='0'

# argument parsing
# see: https://stackoverflow.com/questions/192249/how-do-i-parse-command-line-arguments-in-bash
POSITIONAL=()
while [[ $# -gt 0 ]] ; do
    key="$1"
    case $key in
        -h|--help)
        show_usage
        ;;
        -e|--extract)
        action='extract'
        shift # past argument
        ;;
        -s|--stats)
        action='stats'
        shift # past argument
        ;;
        -o|--output)
        outdir="$2"
        shift # past argument
        shift # past value
        ;;
        -d|--debug)
        DEBUG='1'
        shift # past argument
        ;;
        *)    # unknown option
        POSITIONAL+=("$1") # save it in an array for later
        shift # past argument
        ;;
    esac
done
set -- "${POSITIONAL[@]}" # restore positional parameters

debug "start of program"

exit 0
```
