# git-hook-deploy

Simple shell script to deploy static websites. Based on [Jekyll](jekyllrb.com), but can be adjusted to just about any static site builder.

## Background

Deploying lots of static sites that change frequently can become cumbersome, especially when you deploy to shared webspace purchased through a random provider and only have access to a slow web-based FTP interface. 

Git hooks are scripts that can be executed automatically at specific points during a Git workflow. The shell script below leverages this capability to automatically deploy specific branches of your static project whenever a certain branch has another branch merged into it.

Read more about Git hooks in the [official Git documentation](https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks) or [githooks.com](https://githooks.com/). 

## Requirements

* Git (and a solid knowledge of branch strategy)
* Rsync 
* Unix-based OS. You might be able to get this to work on Windows but will need to find your own resources on how Git hooks behave on Windows.
* SSH access to your destination server. I find that many providers give you SSH access even for shared webspace. Check with your provider if they do. 
* SSH knowledge. If you have never worked with SSH, you will find this script rather difficult to understand and maintain. This in-depth tutorial on [SSH essentials by Digital Ocean](https://www.digitalocean.com/community/tutorials/ssh-essentials-working-with-ssh-servers-clients-and-keys) is a good first step.

## Usage

* From your project root, go to `.git/hooks/`
* Run `wget https://raw.githubusercontent.com/smnwltr/git-hook-deploy/main/post-merge` to download the file directly from GitHub or create it manually via `touch post-merge` and copy paste the code. Make sure you have `wget` installed (ships with most Linux distributions, get it with Homebrew if you are on macOS)
* Make it executable with `chmod +x post-merge`
* Replace the placeholders with your project info (lines 4-16)
* Test if it works by executing the script via `./post-merge`. Make sure you specified the current branch as the production branch in the previous step.
* If it worked, it will now automatically deploy your site every time you merge a branch into your designated production branch

The script uses Rsync to transfer files securely via SSH from your local source to the remote destination. A set of standard options is used. If you know your way around Rsync, adjust the options based on your needs.


## Customization

This script should work with other static site builders out there. All you have to adjust is the build command and rsync source on line 21. 


## Example

I always designate the `main` branch of my repo as the live production branch. All work is done on `develop`. When a website update is ready to go live, I checkout `main` and then merge `develop` into `main`. This triggers my `post-merge` script and deploys the site.

You can extend the script by adding an additional environment, e.g. `staging`, which can be hosted on the same server but different directory, or on a different server altogether. The logic remains the same, simply add another branch and conditional code fragement to be executed when your designated branch is being merged into.


## Pages deployed with this script
* [wltr.co](https://wltr.co)