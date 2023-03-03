***Install `chocolately`***

https://docs.chocolatey.org/en-us/choco/setup

`chocolately` is a package managers for windows, just like other package managers in linux (`apt`, `yum`, `dnf`)

Open the Command Prompt (`cmd.exe`) as administrator, then run below command

```
@"%SystemRoot%\System32\WindowsPowerShell\v1.0\powershell.exe" -NoProfile -InputFormat None -ExecutionPolicy Bypass -Command "[System.Net.ServicePointManager]::SecurityProtocol = 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))" && SET "PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin"
```

***Install `jbang`***

https://www.jbang.dev/

```
choco install jbang
```

***Install `quarkus`***

```
jbang trust add https://repo1.maven.org/maven2/io/quarkus/quarkus-cli/"
jbang app install --fresh --force quarkus@quarkusio"
```


***Install `graalvm`***

```
choco install graalvm
```
