# Sample Dockerfile

# Indicates that the mssql-server-windows-developer:2017-CU3 image will be used as the base image.
FROM microsoft/mssql-server-windows-developer:2017-CU3

# Metadata
LABEL maintainer="Matthew Winfield <mwinfie@gmail.com>" \
    version="1.0" \
    description="This Dockerfile creates my first windows image."

# Install git
ENV GIT_VERSION 2.18.0
ENV ANSIBLE_VERSION v2.6.1

# Stops docker builds on error as opposed to the default, $ErrorActionPreference = Continue
RUN $ErrorActionPreference = 'Stop'

RUN [Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12 ; \
    Invoke-WebRequest $('https://github.com/git-for-windows/git/releases/download/v{0}.windows.1/MinGit-{0}-64-bit.zip' -f $env:GIT_VERSION) -OutFile 'mingit.zip' -UseBasicParsing ; \
    Expand-Archive mingit.zip -DestinationPath c:\mingit ; \
    Remove-Item mingit.zip -Force ; \
    setx /M PATH $('c:\mingit\cmd;{0}' -f $env:PATH)

# Pull down remote repository for testing
RUN git.exe clone https://github.com/mwinfie/ansible-modules.git

# Obtain versioned Ansible.ModuleUtils.Legacy.psm1 for testing
RUN git.exe clone https://github.com/ansible/ansible.git; \
    cd C:\ansible ; \
    git.exe checkout $env:ANSIBLE_VERSION ; \
    Copy-Item -Path C:\ansible\lib\ansible\module_utils\powershell\Ansible.ModuleUtils.Legacy.psm1 \
        -Destination C:\ansible-modules\Ansible.ModuleUtils.Legacy.psm1; \
    cd C:\ ; \
    Remove-Item -Path C:\ansible -Recurse -Force

# Run tests