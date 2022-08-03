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
#### 3. To see the others tools (in user-agent)
    - awk -F'"' '/GET/ {print $6}' /var/log/apache2/access* | cut -d' ' -f1 | sort | uniq -c | sort -rn
##### NB : changes the "-f1" to others field to see different results tools ex:-f2 or -f3
#### 4. To see more details about the tools on user-agent "http status code"
    - cat access* | grep "gobuster" | cut -d " " -f9 | sort | uniq -c
##### NB : Pay attention for success or redirect http status code (200 or 300 variant)
#### 5. Observe http status code for 301 from the used scanning tools
    - cat access* | grep "gobuster" | awk '{ if($9==301) {print}}'
##### NB : Now we know what url that the adversary succed scanning, one or some of them maybe vuln
#### 6. Analyze the activities in all the adversary succeded scanning     
    - cat access* | grep "akismet" | cut -d " " -f9 | sort | uniq -c
##### NB : Observer all http status code, foccuss on 200 and 301, is there any malicious?
    - cat access* | grep "akismet" | awk '{ if($9==301) {print}}' && cat access* | grep "akismet" | awk '{ if($9==200) {print}}'
