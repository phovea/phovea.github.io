---
layout: documentation
title: Phovea Development Process
order: 2
---


First part we explain how all Phovea repositories are structured. Then how to contribute new features and bugfixes to Phovea repositories and finally how to proceed when creating a new release.

## Phovea repository structure

The structure of all Phovea repositories is based on [Driessen's Git branching model](http://nvie.com/posts/a-successful-git-branching-model/).

In summary:

* All Phovea repositories must have a master and a develop branch ([main branches](http://nvie.com/posts/a-successful-git-branching-model/#the-main-branches))
  * **master** branch contains stable code that is production-ready
    * All releases are tagged with a version number
    * Dependencies to other Phovea repositories must point to a *specific version number*
  * **develop** branch contains code that is under on-going development
    * Dependencies to other Phovea repositories must point to the respective *develop branch*
* Phovea repositiories may have [supporting branches](http://nvie.com/posts/a-successful-git-branching-model/#supporting-branches)
  * **[Feature branches](http://nvie.com/posts/a-successful-git-branching-model/#feature-branches)** contain code for new features for the upcoming or a distant future release
    * May branch off from: **develop**
    * Must merge back into: **develop** by filing a reviewed pull request (PR)
    * Branch naming convention: anything except master, develop, release-\*, or hotfix-\* 
  * **[Release branches](http://nvie.com/posts/a-successful-git-branching-model/#release-branches)** support preparation of a new production release and allow for minor bug fixes and preparing meta-data for a release (version number, build dates, etc.).
    * May branch off from: **develop**
      * NOTE: By branching off the **develop** branch is cleared to receive features for the next big release
    * Must merge back into: **develop** *and* **master** by filing a reviewed PR
    * Branch naming convention: release-\* 
  * **[Hotfix branches](http://nvie.com/posts/a-successful-git-branching-model/#hotfix-branches)** contain code that fix an undesired state of a live production version.
    * May branch off from: **master**
    * Must merge back into: **develop** *and* **master** by filing a reviewed PR
    * Branch naming convention: hotfix-\* 


## Contribute to Phovea repositories

1. Develop features in a separate **feature branch** that is branched off the **develop** branch
  * IMPORTANT: If your feature affects code in multiple repositories, use the same branch name accross all repositories
1. Remember to commit and push your changes regularly
1. Once the feature implementation is done, [file a pull request](https://blog.alphasmanifesto.com/2016/07/11/how-to-create-a-good-pull-request/) (PR) and reference the PR in other repositories for the same feature
1. Check if Travis is green, otherwise fix the issue with a new commit to the **feature** branch
1. Assign a reviewer for your PR
1. The reviewer will [review the code](https://blog.alphasmanifesto.com/2016/11/17/how-to-perform-a-good-code-review/) and reject or approve the PR
1. Approved PRs merge the new feature back to the **develop** branch
1. The reviewer removes the **feature** branch
1. The *develop branch* collects all features for the (undefined) next release


## Phovea release process

1. When all (or enough) features are implemented and tested create a new **release branch** from the **develop** branch
  * Use the next version number in the branch name (e.g., *release-1.0.0*)
1. Clear the **develop** branch to receive features for the next big release
1. Changes that can/should be done in the release branch:
  * Update version number and build dates
  * Apply minor fixes only
  * Change Phovea dependencies from **develop** to the new version number
    * NOTE: Follow the dependency hierarchy (see below)
1. File two [pull requests](https://blog.alphasmanifesto.com/2016/07/11/how-to-create-a-good-pull-request/): 1) **release** -> **master**; 2) **release** -> **develop**
1. Check if Travis is green for both PRs, otherwise fix the issue with a new commit to the **release** branch
1. Assign a reviewer for both PRs
1. The reviewer will test once more and check if the version number is correct
1. Approved PRs merge all features back into **master** and **develop**
1. The reviewer removes the **release** branch
1. Go to the **master** branch
1. Create a new tag using the new version number ([Github release](https://github.com/blog/1547-release-your-software))
  * NOTE: The release notes must contain a list of all PRs/issues that are contained in this release
1. *TODO: Publish on npm and pip*

### Dependency hierarchy

When releasing a new version follow the hierarchy from outside to inside. Please note that some dependency might have changed meanwhile. The most recent dependencies are listed in the *packages.json* within each repository.

* [server](https://github.com/phovea/phovea_server)
 * [data_redis](https://github.com/phovea/phovea_data_redis)
 * [data_mongo](https://github.com/phovea/phovea_data_mongo)
 * [data_hdf](https://github.com/phovea/phovea_data_hdf)
 * [data_sql](https://github.com/phovea/phovea_data_sql)
* [core](https://github.com/phovea/phovea_core)
 * [d3](https://github.com/phovea/phovea_d3)
 * [vis](https://github.com/phovea/phovea_vis)
 * [vis_lineup](https://github.com/phovea/phovea_vis_lineup)
 * [ui](https://github.com/phovea/phovea_ui)
* [server](https://github.com/phovea/phovea_server) + [core](https://github.com/phovea/phovea_core)
 * [security_flask](https://github.com/phovea/phovea_security_flask)
* [d3](https://github.com/phovea/phovea_d3) + [ui](https://github.com/phovea/phovea_ui)
 * [importer](https://github.com/phovea/phovea_importer)
* [security_flask](https://github.com/phovea/phovea_security_flask) + [ui](https://github.com/phovea/phovea_ui) + [vis](https://github.com/phovea/phovea_vis)
 * [clue](https://github.com/phovea/phovea_clue)
* [core](https://github.com/phovea/phovea_core) + [redis](https://github.com/phovea/phovea_data_redis)
 * [processing_queue](https://github.com/phovea/phovea_processing_queue)
