# system_things
new computer? drag and drop to .bashrc

~~~~
###############
# Alias Block #
###############
alias diskspace="du -S | sort -n -r |more"
alias du="du -ach | sort -h"
alias ps="ps auxf"
alias psg="ps aux | grep -v grep | grep -i -e VSZ -e"
alias mkdir="mkdir -p"
alias histg="history | grep"
alias myip="curl http://ipecho.net/plain; echo"
alias gitgraph="git log --graph --abbrev-commit --decorate --format=format:'%C(bold blue)%h%C(reset) - %C(bold green)(%ar)%C(reset) %C(white)%s%C(reset) %C(dim white)- %an%C(reset)%C(bold yellow)%d%C(reset)' --all"
alias gcleanup='git branch --merged | grep -v "\*" | grep -v "master" | xargs -n 1 git branch -d'
alias greset='git checkout master && git pull && git remote prune origin && gcleanup'

~~~~

~~~~
#############
# Functions #
#############

function extract {
 if [ -z "$1" ]; then
    # display usage if no parameters given
    echo "Usage: extract <path/file_name>.<zip|rar|bz2|gz|tar|tbz2|tgz|Z|7z|xz|ex|tar.bz2|tar.gz|tar.xz>"
    echo "       extract <path/file_name_1.ext> [path/file_name_2.ext] [path/file_name_3.ext]"
    return 1
 else
    for n in $@
    do
      if [ -f "$n" ] ; then
          case "${n%,}" in
            *.tar.bz2|*.tar.gz|*.tar.xz|*.tbz2|*.tgz|*.txz|*.tar) 
                         tar xvf "$n"       ;;
            *.lzma)      unlzma ./"$n"      ;;
            *.bz2)       bunzip2 ./"$n"     ;;
            *.rar)       unrar x -ad ./"$n" ;;
            *.gz)        gunzip ./"$n"      ;;
            *.zip)       unzip ./"$n"       ;;
            *.z)         uncompress ./"$n"  ;;
            *.7z|*.arj|*.cab|*.chm|*.deb|*.dmg|*.iso|*.lzh|*.msi|*.rpm|*.udf|*.wim|*.xar)
                         7z x ./"$n"        ;;
            *.xz)        unxz ./"$n"        ;;
            *.exe)       cabextract ./"$n"  ;;
            *)
                         echo "extract: '$n' - unknown archive method"
                         return 1
                         ;;
          esac
      else
          echo "'$n' - file does not exist"
          return 1
      fi
    done
fi
}


~~~~



~~~~
#####################
#PS1 w/ git and k8s #
#####################
source /home/paul/work/go_projects/src/github.com/jonmosco/kube-ps1/kube-ps1.sh
PS1='\n \[\e[1;37m\]|-- \[\e[1;32m\]\u\[\e[0;39m\]@\[\e[1;36m\]\h\[\e[0;39m\]:$(kube_ps1) \[\e[1;33m\]\w\[\e[0;39m\]\[\e[1;32m\]$(__git_ps1 " (%s)")\[\e[0;39m\] \[\e[1;37m\]--|\[\e[0;39m\]\n$'

############
#direnv    #
############
eval "$(direnv hook bash)"
~~~~
