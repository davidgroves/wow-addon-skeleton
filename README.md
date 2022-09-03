# What is this.

This is a highly opiniated guide on how to setup a development environment for creating a WoW addon.

In August 2022, I wanted to create an addon to do a simple task, and I was perplexed with the WoW addon ecosystem. It obviously isn't as mainstream a development environment as working with the web and javascript, or data science and python would be. Therefore working out how package management works, what standards the WoW community uses, and what libraries are good and how to use them was a significant challenge.

This project is an attempt to consolidate this for other developers to reduce the time it takes to get going as a new wow addon developer.

# What you get.

- A good editor / environment to write your project in.
    - Syntax Highlighting.
    - Code Linting
    - A build tool to package and distribute your addon.

# Assumptions.

- You are developing on Windows 10.
- You have a copy of WoW on that Windows PC, that you intend to use for development.
- You have a working [WSL2 - Windows Subsystem for Linux 2](https://docs.microsoft.com/en-us/windows/wsl/install) install on your PC, or are willing to install it with Ubuntu 20.04.
- You will use [VScode](https://code.visualstudio.com/) as your editor.
- You will store your code in [Github](https://github.com/), and you have a github account.
- You want to release your addon on [Curseforge](https://curseforge.com/), [WowInterface](https://wowinterface.com/addons.php), [Wago](https://wago.io/) and [Github](https://github.com/).
- You want to use the ACE framework for your addon. ACE is the most popular framework for WoW Addon development, providing easy tools for GUI creation, variable persistance, and interplayer communication.


# Environment Setup (do this once).

## VSCode.

1. Install VScode.
2. Install the [Wow API plugin](https://marketplace.visualstudio.com/items?itemName=ketho.wow-api)
    - Follow the instructions carefully, in the section [Environment Setup](https://marketplace.visualstudio.com/items?itemName=ketho.wow-api) as to install this in the recommended manner you will install a Ubuntu Virtual Machine for WSL2.
    - If you had to reboot to setup WSL2, you run `bash` from a cmd prompt before starting the "WSL Ubuntu Bash Shell" section.
    - The `pip` in the `apt-get install` line needs to be `python-pip` instead.
    - Inside the Ubuntu VM, do `sudo apt install jq pandoc` to install the jq tool needed to upload, and the pandoc tool so markdown is supported.
3. Install the [WoW TOC plugin](https://marketplace.visualstudio.com/items?itemName=stanzilla.vscode-wow-toc), to colourise wow .toc files.
4. Install the [LuaFormatter plugin](https://marketplace.visualstudio.com/items?itemName=Koihik.vscode-lua-format) for vscode.
5. 


# Making a new WoW addon.

- Create a github project for your addon. Select "lua" as your gitignore format.
- Launch `bash` at the `cmd` prompt, to enter your ubuntu environment.
- `git clone` (using SSH), your project.
- Add .release/ to its gitignore.
- Copy the .toc and .lua files from `skeleton/` in this repo to your new project.
    _This is just by convention, you can keep whatever names you really want._
- Copy the skeleton .pkgmeta file into the root of your addon directory. Modify it to suite your addon.
- Update the TOC files with the correct current version as the "Interface" field. You can get this in game by running `/run print((select(4, GetBuildInfo())))` on the appropriate game version.
        - _The skeleton.toc file is the one for retail. WoW allows you to add \_Mainline to the filename, but curseforge needs you to just use the default fall back name currently_
- Rename all the .toc files to the same name as your addon directory.
- Add your curse project information to the TOC file. Find out more about how to setup pkgmeta and the build process at the [BigWigsPackager documentation](https://github.com/BigWigsMods/packager#customizing-the-build).
- Make a .env file with your tokens for the uploads, as explained in the [BigWigsPackager documentation](https://github.com/BigWigsMods/packager#uploading).
    - You get your CF token from the [CF authors page](https://authors.curseforge.com/account/api-tokens)
    - You get your WOWI token from [The WoWInterface Control Panel](https://www.wowinterface.com/downloads/filecpl.php?action=apitokens).
    - You get your WAGO token from the [Wago Account Page](https://addons.wago.io/account/apikeys)
    - You get your github OAUTH token from [Github Personal Access Tokens](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token)
- Manually make your first release, without uploading using `./release -d -r $PATH_TO_WOW_ADDON_DIRECTORY`. Note this is the path, as seen by the WSL environment, and so will most likely be `/mnt/c/PATH/TO/WOW/ADDONS/INTERFACE`


### Full example

In WSL bash shell :-

```bash
git clone git@github.com:davidgroves/wow-autojoke.git
cd wow-autojoke
wget https://raw.githubusercontent.com/davidgroves/wow-addon-skeleton/main/.pkgmeta
echo "# Ignore the release directory" >> .gitignore
echo ".release/" >> .gitignore
mkdir .release
cd .release
wget https://raw.githubusercontent.com/BigWigsMods/packager/master/release.sh
echo "CF_API_KEY=XXXXXXX" >> .env
echo "WOWI_API_KEY=XXXXXXX" >> .env
echo "WAGO_API_TOKEN=XXXXXXX" >> .env
echo "GITHUB_OAUTH=XXXXXXX" >> .env
./release -d
```

 In Powershell.