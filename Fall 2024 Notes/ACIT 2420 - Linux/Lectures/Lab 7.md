Brandon Chow A00931386

```bash
#!/bin/bash
if [[ "$#" -eq 0 ]]; then 
	less "$HOME/.local/share/todos"
fi

while getopts ":n:s:" opt; do
	case "${opt}" in 
		n)
			echo "${OPTARG}" >> "$HOME/.local/share/todos"
			;;
		s)
			grep "${OPTARG}" "$HOME/.local/share/todos"
			;;
		*)
			less "$HOME/.local/share/todos"
			;;
	esac
done
```