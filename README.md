# LinuxServerLogAnalysis
Note for analyzing logs on linux server from siber attack

## Linux Command Line in Linux Logging Basics
### Command-lines tools that usefull in log analysis :
    -Head (head [options] [file_name])
    -Tail (tail [options] [file_name])
    -Cat (cat [options] [file_name])
    -Grep (grep [options] [file_name])
    -Awk (awk '/search_pattern/ { action_to_take_on_matches; another_action; }' file_to_parse)
### /var/log/apache2/ logging analysis practice :
#### 1. To see how many IP address accessed the webserver and how many hits they do
    -cat access* | awk '{print $1}' | sort | uniq -c
#### 2. To see the the selected IP address logs in the first 100 lines
    -cat access* | grep "192.168.2.145" | head -100
##### NB : select for the biggest hits from one IP and observer the user-agent field
#### 2. To see the the selected IP address logs in the first 100 lines
    -cat access* | grep "192.168.2.145" | head -100
