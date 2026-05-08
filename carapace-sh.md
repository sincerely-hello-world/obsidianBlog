https://carapace-sh.github.io/carapace-bin/setup.html

```
tee -a ~/.bashrc <<'EOF'

# Carapace completion
export CARAPACE_BRIDGES='zsh,fish,bash,inshellisense' # optional
source <(carapace _carapace)

alias sb='source ~/.bashrc'
EOF
```