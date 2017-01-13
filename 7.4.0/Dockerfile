FROM microsoft/nanoserver

SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]

ENV NPM_CONFIG_LOGLEVEL info
ENV NODE_VERSION 7.4.0
ENV NODE_SHA256 01739fabdec4fb63eff761022f7b9a2d241430d2c9a5755a07a39e48b54bb471

RUN Invoke-WebRequest $('https://nodejs.org/dist/v{0}/node-v{0}-win-x64.zip' -f $env:NODE_VERSION) -OutFile 'node.zip' -UseBasicParsing ; \
		if ((Get-FileHash node.zip -Algorithm sha256).Hash -ne $env:NODE_SHA256) {exit 1} ; \
    Expand-Archive node.zip -DestinationPath C:\ ; \
    Rename-Item -Path $('C:\node-v{0}-win-x64' -f $env:NODE_VERSION) -NewName 'C:\nodejs' ; \
    New-Item $($env:APPDATA + '\npm') ; \
    $env:PATH = 'C:\nodejs;{0}\npm;{1}' -f $env:APPDATA, $env:PATH ; \
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\Environment\' -Name Path -Value $env:PATH ; \
    Remove-Item -Path node.zip

RUN write-output 'copy -Path c:\code -Destination c:\run -Recurse -Force' | out-file c:\run.ps1
RUN write-output 'set-location c:\run' | out-file c:\run.ps1 -append
RUN write-output 'npm install' | out-file c:\run.ps1 -append
RUN write-output 'npm start' | out-file c:\run.ps1 -append

CMD \run.ps1