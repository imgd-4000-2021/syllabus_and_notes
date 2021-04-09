# Unreal + Git == pain

- [Understanding Git](https://git-scm.com/book/en/v2/Getting-Started-About-Version-Control)
- [Tutorial on UE4 + Git](https://stefanperales.com/blog/unreal-engine-4-and-git-lfs/)
- [A previous IMGD 4000 Tutorial on source control](https://github.com/imgd-4000-2020/syllabus-and-notes/blob/master/version_control_day6.md). This tutorial might make sense to follow if everyone in your group is comfortable with the command line.

## Installation of git, git-lfs, and git plugin for UE4
- [Install git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
- [Install git-lfs](https://git-lfs.github.com)
- [Git plugin](https://srombauts.github.io/UE4GitPlugin/)

## Troubleshooting macOS pointer to Git
If you’re on a Mac, you’ll need to follow these steps to get git-lfs to work:
https://answers.unrealengine.com/questions/704810/git-lfs-problems-on-mac.html

Ask me for help with this, as you’ll need to properly identify the paths to your installation of git and git-lfs. For me my script looks like this (with git-lfs installed via homebrew): 

```bash
  #!/bin/sh
  GIT_PATH="/usr/bin"
  GIT_LFS_PATH="/usr/local/Cellar/git-lfs/2.10.0/bin"
  /usr/bin/env PATH="${GIT_PATH}:${GIT_LFS_PATH}:${PATH}" "${GIT_PATH}/git" "$@"
```

## .gitattributes file
I also needed to manually add my `.gitattributes` file to the top level of my project directory (Windows users might not need to do this).

```
# UE file types
*.uasset filter=lfs diff=lfs merge=lfs -text
*.umap filter=lfs diff=lfs merge=lfs -text
# Raw Content types
*.fbx filter=lfs diff=lfs merge=lfs -text
*.3ds filter=lfs diff=lfs merge=lfs -text
*.psd filter=lfs diff=lfs merge=lfs -text
*.png filter=lfs diff=lfs merge=lfs -text
*.mp3 filter=lfs diff=lfs merge=lfs -text
*.wav filter=lfs diff=lfs merge=lfs -text
*.xcf filter=lfs diff=lfs merge=lfs -text
*.jpg filter=lfs diff=lfs merge=lfs -text
```
