# Software releases

* [Intro](#intro)
* [Prepare environment](#prepare-environment)
* [Runbook](#runbook)
* [Update Documentation](#update-documentation)


## Intro

The BitDust project development flow is based on two independent Git repositories:

* [Development repository](https://github.com/bitdust-io/devel)
* [Stable repository](https://github.com/bitdust-io/public)


When changes in the development repo are considered "good enough", the files are manually copied by one of the developers to the stable repo fork on their local machine, and a new commit must be created to start a new Pull Request.

This process can be considered a new "Release" - BitDust does not have any versioning of releases because it is not required. After the Pull Request is merged, the release is done.

Every BitDust node whose sources are located in the `~/.bitdust/src/` folder "checks & fetches" fresh commits from the Git repository it was cloned from some time after it starts (`git fetch` and `git reset --hard origin/master`).
This way the BitDust software on your local machine automatically "updates itself" and stays in sync with the "Stable" repository. Sources cloned to any other location are not updated automatically. Every BitDust contributor is responsible for delivering high-standard, super reliable and well-tested source code changes every time a Pull Request is merged.

You can also fork the BitDust "stable" repo and clone your fork locally to quickly check and run the main Python code. Just like any other GitHub project you have forked, your fork will be fully independent of the main repo - you will have to sync with the main repo manually if you wish to stay on the same version. This works best for all developers and also for those who wish to learn BitDust or participate in testing.

If you stay out of sync with other users, your data is at risk! Remember to always check & pull your BitDust software sources and stay in sync with the main network by updating your code from the [Stable repository](https://github.com/bitdust-io/public).

For non-developers and all other people willing to join the BitDust network in the usual way, we maintain the [BitDust Desktop](https://github.com/bitdust-io/desktop/releases) application installer.

You can go directly to the [bitdust.io web site](https://bitdust.io) and download the installer file - we provide a link directly from the [GitHub repo](https://github.com/bitdust-io/desktop/). The application uses the Electron framework for the GUI; it will automatically clone the BitDust Stable repo into the `~/.bitdust/src/` folder the first time you run the application and keep your local sources in sync with the master branch.

As a user of the BitDust software, you can disable automatic updates at any moment by setting the environment variable `BITDUST_GIT_SYNC_SKIP=1` before starting BitDust (or by keeping the sources outside of the `~/.bitdust/src/` folder), and always run only the code you cloned the first time you joined the BitDust network.

Below is a step-by-step guide for developers to deliver changes from the "Development" repository into the "Stable" repository.



## Prepare environment

To be able to run a new BitDust release, you must already have forked and cloned both repositories.
Basically you need to do this only once and then just keep your repositories up to date with the main branches.

Here we assume your fork will be "origin" and the main repository will be added as "upstream":

    # fork and clone development repo
    git clone git@github.com:<your GitHub username here>/devel.git bitdust.devel
    cd bitdust.devel
    git remote add upstream https://github.com/bitdust-io/devel.git
    cd ..

    # fork and clone stable repo
    git clone git@github.com:<your GitHub username here>/public.git bitdust
    cd bitdust
    git remote add upstream https://github.com/bitdust-io/public.git
    cd ..



## Runbook

Change to the `bitdust.devops/` repository and run the first script:

    git clone https://github.com/bitdust-io/devops.git bitdust.devops
    cd bitdust.devops/
    ./cicd/release_prepare


This will prepare all files to be committed to the "Stable" repository.
All modified/added/removed files will be displayed in your terminal output. The script does a bunch of things:

* copy files from "devel" to "stable" repo
* disable DEBUG mode in all Python files
* compare all files in both repositories
* update the `HISTORY.txt` file in the "devel" repo


Now edit the `bitdust.devel/CHANGELOG.txt` file manually - you need to provide some info about your changes.
After running `release_prepare` you should see a list of the most recent commits in your terminal output.
Just copy those lines and paste them at the top of the `CHANGELOG.txt` file:

    # copy text block from `HISTORY.TXT` file into `CHANGELOG.txt` and make it look nice
    nano ../bitdust.devel/CHANGELOG.txt


Now you need to change to the "stable" repository and run `git add ...` / `git rm ...` commands to confirm changes.

First, mark all modified files in git to be committed in the new release:

    cd ../bitdust
    git add -u .


Add all other new files to git manually - it is important not to miss any files recently created in the "devel" repo:

    git add <some new file>


If some files or folders were removed from the "devel" repo, do not forget to also remove them from the "stable" repo and mark those changes to be committed:

    git rm <some old file>


We are almost done!

Change back to the `bitdust.devops` repository and run the second script:

    cd ../bitdust.devops/
    ./cicd/release_start


That script will push all prepared changes to your forked repositories.

All you need to do now to start the release is to create a new [Pull Request](https://github.com/bitdust-io/public/pulls) to the "stable" repository via the GitHub web site.

Make sure the Travis build is green, people review your changes and everyone agrees with your code.

One of the core developers must click the Merge button, and ... Congratulations, your changes are LIVE!

Do not forget to update your fork right away to stay in sync and avoid merge conflicts later:

    git pull upstream master
    git push origin master



## Update Documentation

The BitDust project documentation is fully open source, and everyone is welcome to contribute to it and improve it in the [Documentation repository](https://github.com/bitdust-io/docs).
These steps are required to keep the documentation in sync with the code.

Update the "docs" repo first; you must already have it forked and cloned into the `./bitdust.docs/` folder in the same parent folder as the "devel" and "stable" repos:

    cd ../bitdust.docs/
    ./build_api
    ./build_settings
    # TODO: ./build_changelog


Push the changes to your fork of the documentation repository and open a new [Pull Request](https://github.com/bitdust-io/docs/pulls) to the upstream:

    git push origin master
    # Open & Review & Merge Pull Request


Update your fork to stay in sync after a merge:

    git pull upstream master
    git push origin master


ALL DONE!

