---

title: "PC Automation: Zero Percent User Interaction Automation"
date: 2017-01-01
tags: [PC Automation, Automation, Test Automation, QA, Security Automation, Restart Automation]
header:
excerpt: "PC Automation, No User Interaction, Test Automation"
mathjax: "true" 
---

## Continuing the Console automation execution without user interaction after restart of machine using bat programming.

Basically the major problem with automation scripts or frameworks is that execution of automation scripts halts when a machine restart is needed(most important step for security application after installation) and a user interaction is needed in ordered to continue the execution of automation after restart of test machine from the step where it stopped its execution. Below batch code just stores the current automation script in the memory and executes automatically from the next step once machine restarts.

**Steps:**
* Create and Empty text file(jink1.txt)
* Add the all the automation script file location(exe or dll's) to run in the sequential order as you need.
  Ex:
    /home/desktop/xyz.dll
    /home/desktop/abc.dll ....
* As mentioned above xyz.dll will execute first and followed by abc.dll
* Create a batch file and copy the below code and add the above text file location and the current batch file location    
  into the batch code(because machine need to know from where to start execution after restart).
* Run the batch code and automation execution starts and after restart of machine it will automatically resumes from next
  step were it had paused.

**Note:** This has been tested on the Nunit Console.  

Batch Code Block:
  ```batch

  @echo off

  if [%1] == [] goto usage
  if [%2] == [] goto usage

  call :print_head %1 %2
  goto :eof

  :print_head
  setlocal EnableDelayedExpansion

  set "cmd=findstr /R /N "^^" %2 | find /C ":""

  for /f %%a in ('!cmd!') do set number=%%a
  echo %number%

  pause()

  timeout /t 10

  set /a counter=0

  REM set first = %1

  for /f ^"usebackq^ eol^=^

  ^ delims^=^" %%a in (%2) do (
    if "!counter!" LSS "!number!"  (
      if "!counter!" LSS "%1" (set /a counter+=1)
        if "!counter!" GEQ "%1" (
            set /a counter+=1
            REG ADD "HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Run" /V "My App" /t REG_SZ /F /D "C:\Users\Downloads\new.bat !counter! C:\Users\Downloads\junk1.txt"

            %%a

            timeout /t 10  

            )

          )


        )


    goto :eof

    :usage
    echo Usage: nothing much

  ```  
