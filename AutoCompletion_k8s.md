1- Ensure bash-completion is installed
``` bash
sudo yum install bash-completion
```

2- nable kubectl autocompletion : Add the autocompletion script to your ~/.bashrc file
 A- Add the autocompletion script to your ~/.bashrc file
``` bash
echo 'source <(kubectl completion bash)' >>~/.bashrc
```

 B- If you're using an alias for kubectl, add the following:
``` bash
echo 'alias kubectl="k3s kubectl"' >>~/.bashrc
echo 'complete -F __start_kubectl kubectl' >>~/.bashrc
```
C- Reload your ~/.bashrc file to apply the changes:

``` bash
source ~/.bashrc
```
3- Apply autocompletion immediately (optional):
  A- To apply autocompletion immediately without restarting your shell, run:
``` bash
source <(kubectl completion bash)
alias kubectl='k3s kubectl'
complete -F __start_kubectl kubectl
```
