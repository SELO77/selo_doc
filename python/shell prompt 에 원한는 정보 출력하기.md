# shell prompt 에 원한는 정보 출력하기

해당 코드는 bash_it 이 설치된 환경에서의 예시코드 입니다.

```bash
parse_pyenv_venv () {
	if ! [ x$PYENV_VIRTUAL_ENV == x ]; then
		parsepath=$(echo $PYENV_VIRTUAL_ENV | tr "//" "\n")
		for x in $parsepath
		do 
			last=$x
		done
		echo "($last)"
	fi
}

function prompt_command() {
  PS1="\[${BOLD}${MAGENTA}\]\u $(parse_pyenv_venv)\n\[$WHITE\]at \[$GREEN\]\w\[$WHITE\]\$([[ -n \$(git branch 2> /dev/null) ]] && echo \" on \")\[$PURPLE\]\$(parse_git_branch)\[$WHITE\]\n\$ \[$RESET\]"
}

safe_append_prompt_command prompt_command
```