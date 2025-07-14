#### Перенос данных с СХД по iSCSI на другой сервер.
Имеем TATLIN.UNIFIED компании YADRO, от куда будем переносить данные и сервер Windows 2016 куда переносим данные.

- UNIFIED в названии говорит о унифицированном доступе, что и обеспечивается поддержкой следующих протоколов:
```bash
• Поддержка протоколов SMB 2.1, 3.0, 3.1
• Поддержка протоколов NFS 3, 4, 4.1, 4.2
• Поддержка разграничения прав доступа на уровне пользователей
и групп
• Анонимный доступ для SMB и NFS v3
• Дополнительное разграничение прав доступа на уровне подсетей
• Возможность использования единого пула хранения для файловых и
блочных ресурсов
• Удобная визуализация при одновременной работе с блочными и
файловыми ресурсами
• Использование «плавающего» IP адреса упрощает переключение между
контроллерами в случае сбоев
```

<img width="1280" height="641" alt="image" src="https://github.com/user-attachments/assets/02671494-eab8-429d-82b2-8861125a4810" />

Подключаемся на сервере WINDOWS 2016 по таргеру <iscsi-target-1-ip> к TATLIN.UNIFIED через iSCSI.

Далее

Делаем скрипт в powershell и запускаем его на сервере WINDOWS 2016.
>[!WARINING]
>Не забудьте создать на новом месте папки с названием, которые будете копировать.
 
```bash
$robocopyRunning = Get-CimInstance Win32_Process | Where-Object { $_.Name -eq "robocopy.exe" }
if ($robocopyRunning) {
    Write-Host "Уже запущен robocopy. Выход из скрипта."
    exit
}

$dir = "Название папки 1","Название папки 2","Название папки 3","Название папки 4","Название папки 5" и т.д.
$rSrv = "\\10.148.2.1\"

foreach ($d in $dir) {
$rPath = $rSrv + $d
$lPath = "D:\" + $d
cmd.exe /c "robocopy "$rPath" "$lPath" /MIR /E /Z /COPY:DAT /DCOPY:T /R:2 /W:5 /MT:128 /XD DfsrPrivate ~snapshot /TEE /LOG+:C:\script\log\robocopy$d.txt"
 else {
$encoding = [System.Text.Encoding]::UTF8
Send-MailMessage -From ххххх@ххххх.ru -To ххххх@ххххх.ru
-Encoding $encoding -Subject "Error Backup" -Body "Ошибка синхронизации директории $d." -SmtpServer 10.148.2.1}
}

или так, без указания почтового клиента

$robocopyRunning = Get-CimInstance Win32_Process | Where-Object { $_.Name -eq "robocopy.exe" }
if ($robocopyRunning) {
    Write-Host "Уже запущен robocopy. Выход из скрипта."
    exit
}

$dir = "Название папки 1","Название папки 2","Название папки 3","Название папки 4","Название папки 5" и т.д.
$rSrv = "\\10.148.2.1\"

foreach ($d in $dir) {
    $rPath = $rSrv + $d
    $lPath = "D:\" + $d

    if ((Test-Path $rPath) -and (Test-Path $lPath)) {
        cmd.exe /c "robocopy "$rPath" "$lPath" /MIR /E /Z /COPY:DAT /DCOPY:T /R:2 /W:5 /MT:128 /XD DfsrPrivate ~snapshot /TEE /LOG+:C:\script\log\robocopy$d.txt"
    }
    else {
        Write-Warning "Пропущено: $d — один из путей недоступен. Удалённый: $rPath / Локальный: $lPath"
    }
}
```



