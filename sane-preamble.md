# Sane Preamble

## Snippet
```bash
#!/bin/bash
set -euo pipefail
IFS=$'\n\t'

function fail() {
  local path=""
  # local length="${#FUNCNAME}"
  for i in "${!FUNCNAME[@]}"; do
    # omit index 0 is the function 'fail' itself
    # add '&& "$i" -lt "$length-1"' if you want to omit the 'main' function + line number
    if [[ "$i" != "0" ]]; then
      local to_add="${FUNCNAME[$i]}:${BASH_LINENO[$i-1]}"
      if [[ "$path" == "" ]]; then
        local path="$to_add"
      else
        local path="$to_add > $path"
      fi
    fi
  done
  echo >&2 "'$path' failed: $1"
  exit 1
}
```

## Example
```bash
#!/bin/bash
inner() {
  fail "boom"
}

outer() {
  inner "$1"
}

outer "test"

# 'main:10 > outer:7 > inner:3' failed: boom
```

## Notes
* http://redsymbol.net/articles/unofficial-bash-strict-mode/
