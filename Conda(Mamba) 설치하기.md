Conda(Mamba) 설치하기
======
서버에 id와 password를 만들었다면 conda(mamba)를 설치할 차례입니다.

# Conda(Mamba) 란?
* conda는 파이썬 및 기타 프로그래밍 언어를 위한 오픈소스 패키지 관리 시스템 및 환경 관리 시스템입니다.
* mamba는 conda 보다 훨씬 빠른 시스템입니다. 사용방법은 conda 를 mamba로 바꾸기만 하면 됩니다.

# Conda(Mamba) 설치 방법
## 다운로드
https://github.com/conda-forge/miniforge 사이트에 들어가
OS : Linux, Architecture: x86_64 (amd64) 인 Miniforge3의 링크를 복사합니다.
(Download 부분의 파란색 하이퍼링크에서 우클릭 -> 링크 복사)

## 설치
이후 본인 home 서버의 터미널에 wget 을 이용하여 다운로드 합니다.

    wget https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-Linux-x86_64.sh

다운로드한 shell 파일을 bash 를 이용하여 실행합니다.

	bash Miniforge3-Linux-x86_64.sh

이후 과정에서는 ENTER 와 yes 를 눌러서 license관련 내용에 동의하시면 됩니다.

## 설정
설치가 끝난 후 기본으로 conda(mamba)가 실행될 지 지정할 수 있습니다.
yes 로 지정하시면 편합니다.

이후 터미널을 다시 시작해 주세요.
터미널을 다시 시작하였다면 

(base) 본인id@methionine:~$ 

와 같이 base 환경이 기본으로 실행되는 것을 볼 수 있습니다.

# home1 과 home 관련 설정
## home1과 home의 차이가 뭔가요?
서버의 홈이 home이 아닌 home1(또는 그 외)인 경우, 추가적인 확인이 필요합니다.
예시로, Methionine에는 home과 home1이 존재하고, 그 외 다른 서버에는 home 만 존재합니다.
home1은 Methionine에 만 있는 경로 이고, 다른 서버에도 존재하는 home은 전체 서버에서 사용이 가능하도록 심볼릭 링크로 만들어 놓은 경로입니다.
따라서 home1으로 경로를 지정하게 된다면 Methionine에서만 실행 가능합니다.

## 모든 서버에서 Conda(Mamba) 사용하기 - 1
설치를 완료한 터미널에

	conda

또는

	mamba

를 치면

        usage: conda [-h] [-v] [--no-plugins] [-V] COMMAND ...

    conda is a tool for managing and deploying applications, environments and packages.

    options:
    -h, --help          Show this help message and exit.
    -v, --verbose       Can be used multiple times. Once for detailed output, twice for INFO logging, thrice for DEBUG logging, four times for TRACE logging.
    --no-plugins        Disable all plugins that are not built into conda.
    -V, --version       Show the conda version number and exit.

    commands:
    The following built-in and plugins subcommands are available.

    COMMAND
        activate          Activate a conda environment.
        clean             Remove unused packages and caches.
        compare           Compare packages between conda environments.
        config            Modify configuration values in .condarc.
        create            Create a new conda environment from a list of specified packages.
        deactivate        Deactivate the current active conda environment.
        doctor            Display a health report for your environment.
        export            Export a given environment
        info              Display information about current conda install.
        init              Initialize conda for shell interaction.
        install           Install a list of packages into a specified conda environment.
        list              List installed packages in a conda environment.
        notices           Retrieve latest channel notifications.
        package           Create low-level conda packages. (EXPERIMENTAL)
        remove (uninstall)
                        Remove a list of packages from a specified conda environment.
        rename            Rename an existing environment.
        repoquery         Advanced search for repodata.
        run               Run an executable in a conda environment.
        search            Search for packages and display associated information using the MatchSpec format.
        update (upgrade)  Update conda packages to the latest compatible version.

와 같은 글자가 뜨는 것을 확인할 수 있습니다. 

하지만, 다른 서버에 들어가서 mamba나 conda를 쳤을 때 

	-bash: /home/본인id/miniforge3/bin/mamba: /home1/본인id/miniforge3/bin/python: bad interpreter: No such file or directory

와 같은 오류가 뜨는 것을 볼 수 있습니다.
이 경우, mamba 실행파일에 있는 python 인터프리터 경로가 잘못되었다는 뜻입니다.
즉, 앞서 이야기했던 home1과 home 간의 차이로 인한 오류입니다.

## 모든 서버에서 Conda(Mamba) 사용하기 – 2
그렇다면 이 오류를 고쳐봅시다.

	vi miniforge3/bin/mamba

를 치면

    (base) jyr@methionine:~$ vi miniforge3/bin/mamba
    
    #!/home/jyr/miniforge3/bin/python
      
    # -*- coding: utf-8 -*-
    import re
    import sys
    
    from mamba.mamba import main
    
    if __name__ == '__main__':
        sys.argv[0] = re.sub(r'(-script\.pyw?|\.exe)?$', '', sys.argv[0])
        sys.exit(main())

와 같은 글자가 뜹니다.
이때 수정해야 하는 부분은 

	#!/home1/jyr/miniforge3/bin/python

이 부분입니다. 이 home1 부분을

	#!/home/jyr/miniforge3/bin/python

와 같이 home으로 수정하시면 됩니다.
