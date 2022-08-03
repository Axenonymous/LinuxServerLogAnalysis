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
    -cat access* | awk '{print $1}' | sort | uniq -c 

