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
#### 6. Analyze the activities in all the adversary succeded scanning start from http status code    
    - cat access* | grep "akismet" | cut -d " " -f9 | sort | uniq -c
##### NB : Observer all http status code (change the grep "plugins_name" to all that exist), foccuss on 200 and 300 variant, is there any malicious?
    - cat access* | grep "akismet" | awk '{ if($9==301) {print}}' && cat access* | grep "akismet" | awk '{ if($9==200) {print}}'
#### 7. Analyze is there any tools used (in user-agent field)    
    - cat access* | grep "themeisle" | cut -d " " -f12 | sort | uniq -c
#### 8. If there any new tools in user-agent field (-f12), crosscheck betwen plugins and tools    
    - cat access* | grep "themeisle" | grep -i "gobuster" && cat access* | grep "themeisle" | grep -i "curl"
##### NB : Sometimes we can found malicious upload file in POST and GET method

### /var/log/auth* logging analysis practice :
#### 1. Identify malicious activities by web user in the system (user www-data)
    - cat auth* | grep -a "www-data"
##### NB : -a options is for searching the keywords in binaries file (existing system command executions,ex:sudo,chmod,adduser etc.)
#### 2. Identify Privelege Escalation and Persistance
    - cat auth* | grep -a "add"
##### NB : -a options is for searching the keywords in binaries file (existing system command executions,ex:sudo,chmod,adduser etc.)
#### 3. Identify automated malicious running program
    - /var/log/$ sudo crontab -l
##### NB : locate the bash script
    - htop (to see running background program by malicious bash script)
    
### /var/www/html directory analysis practice :
#### 1. Timestamp sorting for analyzing directory or file modify and changes
    - ls -alrt
##### NB : Look for the latest modified directory and file and check what is it
#### 2. Check for all plugins CVE in (https://www.exploit-db.com/)
#### 3. Check for backdoors webshell file
#### 4. Check for another user 
    - #lastlog
    - #cat /etc/passwd
    - #ls /home (user home dir)










