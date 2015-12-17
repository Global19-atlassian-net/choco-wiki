# Community Feed Moderation

The community feed, which is found at https://chocolatey.org/packages, is a moderated feed. That means all new versions of packages are human reviewed prior to approval to check for safety, quality, and correctness. See [[What is moderation|ChocolateyFAQs#what-is-moderation]] for more details. There are also [[trusted packages|ChocolateyFAQs#what-is-a-trusted-package]].

## Definitions

* package - The Chocolatey/NuGet package
* software - The underlying software that the package assists in installing
* installer - The native installer, usually packaged as MSI, NSIS, InstallShield, Wise, Squirrel, or some other flavor.
* the validator - The [package validation service](https://github.com/chocolatey/package-validator) checks the quality of a package based on requirements, guidelines and suggestions for creating packages for Chocolatey’s community feed. We like to think of the validator as unit testing. It is validating that everything is as it should be and meets the minimum requirements for a package on the community feed.
* the verifier - The [package verifier service](https://github.com/chocolatey/package-verifier) checks the correctness (that the package actually works), that it installs and uninstalls correctly, has the right dependencies to ensure it is installed properly and can be installed silently. The verifier runs against both submitted packages and existing packages (checking every two weeks that a package can still install and sending notice when it fails). We like to think of the verifier as integration testing. It’s testing all the parts and ensuring everything is good.

### Roles
* Maintainer - A person that maintains packages. Maintainers are usually subject to the review process.
* Reviewer - Able to review packages but not approve/reject them
* Moderator - Able to set/remove package maintainers, review packages, approve/reject them, able to unlist packages.
* Administrator - Has access to administrative sections of the site. Can perform all functions that a moderator can perform.

#### Becoming a Maintainer
To become a package maintainer, you must have an account on https://chocolatey.org and have at least one package on the site.

#### Becoming a Reviewer
TBD

#### Becoming a Moderator

There is no set process for becoming a moderator yet. Usually it is having many approved packages and understanding the process of creating Chocolatey packages. Eventually it will be something you earn through your reputation on the site.

* Make awesome packages
* Work on the Disqus threads and mailing list.
* Have a desire to better the quality of Chocolatey
* Know a little PowerShell. More is better but yeah.
* Be friendly and customer service-oriented

#### Becoming an Admin
This is not an achievable status.

## Package Review Process
When reviewing new and existing packages, a reviewer/moderator will have a few things left for review after the verifier and validator have verified a package.

### Maintainer Process
As a maintainer you submit packages and they are reviewed to be sure they meet a minimum quality and are correct for being published on Chocolatey.org. It's an important distinction that while almost all valid packages are approved, a package can be rejected for a variety of reasons.

Packages go through two automated checks: validation and verification. There is about a 30 minute lag time from submission until any automatic verification kicks off - this allows the CDN to recheck and pull a newer version of the package up (in the case of resubmission), so that the package version being verified is the one you submitted and not a stale copy.

When you receive emails that require you to take action, you should review what is requested and make the changes. If a package is flagged and needs changes based on requirements, the process is for you to make the required changes and resubmit the ***exact*** same version. The faster you respond to the review process, the faster your package can get approved.

Please note that the process of moderation is an interactive process. If you don't respond to the review process within a reasonable timeframe (right now about 3 months), your package could be rejected on non-response. Moderators give you the benefit of the doubt and will work with you to help you get a package to an approved status. (This also includes the older review process based on email before the site allowed you to comment).

Ensure that you can receive emails from Chocolatey.org so that you will receive email notifications when a package review is updated.

### Reviewer / Moderator Process
Typically a package goes into the moderation queue when submitted.You can get to that by signing in and going to the packages page like you normally would.

 1. You should see a new drop down near the top that allows you to change your view. This is the moderation queue. ![Moderation Queue Dropdown](https://cloud.githubusercontent.com/assets/63502/7542991/b5032a38-f586-11e4-991a-7c7602d508aa.png)
 2. You will see items arranged in order based on reviewed and resubmitted at the top, items ready for review in order based on when they were submitted, and at the end of the queue, you will see items that are waiting for maintainer response. ![Moderation Queue](https://cloud.githubusercontent.com/assets/63502/7543076/58d5530c-f587-11e4-8d73-1325074d6e58.png)
 3. You grab a package and head in and review it based on the following items in the requirements and guidelines.
 4. Ensure the verifier has run. If you see a grey button, you will need to run the install/uninstall yourself if you can as the package is not verified for some reason. That reason should be listed on the package page.
 ![Passed Verifier](https://cloud.githubusercontent.com/assets/63502/11872220/bf58f590-a499-11e5-84bb-6fcf6d320227.png)
 5. Check over the verifier logs to be sure everything looks good (follow the link from the button). If necessary, you can rerun the verifier.
 ![Rerun](https://cloud.githubusercontent.com/assets/63502/11872329/4b8427ec-a49a-11e5-87ea-46e5e43e8140.png)
 6. Go over the review log - shows history and review information so far. Note that when the validator runs it leaves comments. Look for it to have done the automated part of the requirements/guidelines checks. If it has not, you are responsible to check all [[requirements/guidelines|Moderation#requirements-and-guidelines]] (see [[Requirements and Guidelines|Moderation#requirements-and-guidelines]] below).
 ![Review Notes](https://cloud.githubusercontent.com/assets/63502/11872453/fd757a28-a49a-11e5-98c0-cfff70caebbd.png)
 7. Look at the notes section from the latest run of the validator to see if there are additional flagging follow ups from the validator.
 8. Check over the package based on [[moderator review|Moderation#moderator-review]] (below).
 9. Review the previous comments if there are any. ![image](https://cloud.githubusercontent.com/assets/63502/7543258/c2c0abbc-f588-11e4-8cac-4c57b03671f8.png)
 10. Look through the package files ![image](https://cloud.githubusercontent.com/assets/63502/7543284/ddaa41e0-f588-11e4-817a-9d7bd1130a84.png)
 11. Leave comments in the review box ("Add to Review Comments" section) if you have any. Note that you can use markdown here.
 ![Review box](https://cloud.githubusercontent.com/assets/63502/11872386/8a58a1dc-a49a-11e5-9b38-04b42fcc6ebd.png)
 12. If you are approving the package, change Package Status to Approved. If you are Rejecting a package, change the status to Rejected. Otherwise leave the status as is (likely in Submitted).
 13. If you are making a comment or doing another action, but don't want to flag/hold the package for the maintainer to take action, uncheck the "require maintainer to make changes?" box. This is not required to be unchecked if you are approving the package.
 ![Require maintainers to make changes?](https://cloud.githubusercontent.com/assets/63502/11872860/3f47f0f0-a49d-11e5-8725-e58d12a377c8.png)
 14. If you are doing an action that doesn't need to notify the maintainer, uncheck "Send Maintainer email?".
 15. Click Save. You should get a message that the message was sent successfully.
 16. The maintainers receive an email noting the comments. They will follow up on the package page with their comments.
 17. Once the package is updated, it will show up in the top of the queue. At that time, please review it and make sure the maintainers made all changes requested.

#### Moderator Review
You can only ever require a maintainer to make changes if there are findings from the requirements section. Guidelines are strong suggestions that will improve the quality of the package, but consider that a quality over time. A maintainer is NOT required to make changes based on guidelines/suggestions. This deserves to be said twice: **"A moderator cannot hold up a package based on guidelines/suggestions *alone*"**.

The validator checks quite a few items (https://github.com/chocolatey/package-validator/wiki) and leaves a few for you to check. Ensure you have looked over the notes that it has left.

With the exception of included binaries, a review that doesn't flag should take under a minute. If you are holding a package, you can refer the maintainer to this link to save time: https://github.com/chocolatey/choco/wiki/Moderation

##### Requirements
Always be explicit that you are waiting on the maintainer to fix and resubmit the same version of the package so you can move the review process along.

* Is the title appropriate?
* The description should explicitly mention if this package installs trial software or software that needs a license present, or both.
* The authors field (software author) is not being used for the maintainers field (exception: when the maintainer is also the author)
* The tags field is not being abused - note this doesn't mean they are missing tags you believe they should have (that is a guideline).
* Binaries
  * If binaries are included in the package, does the maintainer have distribution rights? If they require explicit permission, a copy of that in PDF form should be in the package contents.
  * Find the checksum of the official binaries and verify they match the included binaries (listed on the package page).
* Automation scripts (the `ps1`/`psm1` files such as `chocolateyInstall.ps1`)
  * Do the scripts try to do anything malicious? This is almost always immediate grounds for banning the maintainer and deleting their packages.
  * Do the scripts set good defaults for silent args? A package should almost ALWAYS install completely silently by default. If a maintainer makes the argument that it is so folks can choose what to pass, remind them this already exists through install arguments (https://github.com/chocolatey/choco/wiki/CommandsInstall#options-and-switches) and if they want to add package parameters (https://github.com/chocolatey/choco/wiki/How-To-Parse-PackageParameters-Argument), they can also do that (and add them to the description).
  * Is there anything in the scripts that would not work with POSH v2? (We are working on making this automatically checked by the validator - see https://github.com/chocolatey/package-validator/issues/1)
  * If the package downloads anything, is it getting downloads from the proper location? Follow the projectUrl to the project site to see where it is downloading from - it should match the scripts. If not there needs to be a really, really good reason for not doing so.
  * Does the download version match the package version?
  * Does the download include both x86 and x64 urls if available?
* Not a package duplicating another existing package ***NOTE***: December 2015: Do not look for duplicate packages at this time. The validator will start handling that after the backlog is manageable.
* Brand New Packages ***ONLY*** (no approved or existing version in history, prereleases do not count)
  * Package Id naming - if the naming doesn't follow our conventions, it is grounds for rejecting immediately with the suggestion they resubmit with suggested name. Note that they may have had prereleases already, and it's still okay to move forward with the rejected status as long as the name of the name of package hasn't been previously approved. See https://github.com/chocolatey/choco/wiki/CreatePackages#naming-your-package
    * suggest the id split if over 25 chars with no "-" in the id
    * flag on "."" in name (unless .portable/.install)

##### Guidelines
If a package is only flagging on guidelines, be sure to move forward on approval (this means no requirements flagged by you or the validator checks).

* Trial software should include the #trial tag. (will become a requirement in Feb 2016)
* Software that requires a license should include a tag #license. (will become a requirement in Feb 2016)
* Tags could always use suggestions to add.
* Automation scripts (the `ps1`/`psm1` files such as `chocolateyInstall.ps1`)
  * Do any scripts try to do anything that an existing Chocolatey function already covers? The maintainers would need a really good reason for diverging from that.

### New Reviewers / Moderators
* Understand the package creation process and the current recommendations, written at https://github.com/chocolatey/choco/wiki/CreatePackages
* Become familiar with the package guidelines and all of the different Chocolatey functions available. https://github.com/chocolatey/choco/wiki/HelpersReference
* Join the [chocolatey-moderators at google groups dot com](https://groups.google.com/forum/#!forum/chocolatey-moderators) mailing list. This is necessary for communication with other moderators and receiving messages regarding changes in moderation.
* Join Gitter and hang out in the [Chocolatey.org Gitter](https://gitter.im/chocolatey/chocolatey.org) and [Choco Gitter](https://gitter.im/chocolatey/choco) rooms from time to time.

## Requirements and Guidelines
While probably the most comprehensive, this list may not be fully up-to-date. This should serve as a most general understanding, knowing that the [validator](https://github.com/chocolatey/package-validator/wiki) may be checking for newer things than are written here and that reviewers/moderators may find newer things to check from time to time.

### Existing Packages
This section provides the requirements for packages that have had at least one released version approved or exempted. This includes any packages that existed prior to moderation being turned on (possibly an Unknown status).

#### Requirements
Requirements represent the minimum quality of a package that is acceptable. When a package version has failed requirements, the package version requires fixing and/or response by the maintainer. Provided a Requirement has flagged correctly, it ***must*** be fixed before the package version can be approved. The exact same version should be uploaded during moderation review.

* ProjectUrl - It's required for the community feed
* Is the title appropriate?
* At least something written in the description. It should be sufficient to explain the software.
* The description should explicitly mention if this package installs trial software or software that needs a license present, or both.
* The authors field (software author) is not being used for the maintainers field (exception: when the maintainer is also the author)
* The tags field is not being abused - note this doesn't mean they are missing tags you believe they should have (that is a guideline).
* Tags do not include "chocolatey" (with the exception of the chocolatey packages)
* Look over the package files
  * If binaries are included in the package, does the maintainer have distribution rights? If they have explicit permission, a copy of that in PDF should be in the package contents.
  * **Install/Uninstall scripts:**
    * Do the scripts try to do anything malicious? This is almost always immediate grounds for banning the maintainer and deleting their packages.
    * Do the scripts set good defaults for silent args?
    * Is there anything there that would not work with POSH v2?
    * If it is a download, is it getting it from the proper location? Use the project site (projectUrl) to determine where the download for the file is coming from and it should match the one in the package files. If not there needs to be a really, really good reason for not doing so.
    * Does the download version match the package version?
    * Does the download include both x86 and x64 urls if available?
    * Flag the use of any of the following: $nugetChocolateyPath, $nugetPath,$nugetExePath, $nugetLibPath, $chocInstallVariableName, $nugetExe
    * Does the PowerShell script try to use any choco commands? e.g. choco install/upgrade/uninstall?
    * Does the package try to do anything that an existing Chocolatey function already covers? The maintainers would need a really good reason for diverging from that.
    * If the package is a portable package (downloads a zip file or non-install archive, many times carries the .portable name), does it try to put that in Program Files? This is a no no because Program Files requires admin permissions to write to and is typically the place for natively installed software.
* Does the package install correctly?
* Does the package uninstall correctly? (this means the package, not the underlying software. We'd like to have that as well but it's more a guideline at the moment than a requirement. Patience, we will get there).
* Brand New Packages ***ONLY*** (no approved or existing version in history, prereleases do not count)
  * Package Id naming - if the naming doesn't follow our conventions, it is grounds for rejecting immediately with the suggestion they resubmit with suggested name. Note that they may have had prereleases already, and it's still okay to move forward with the rejected status as long as the name of the name of package hasn't been previously approved. See https://github.com/chocolatey/choco/wiki/CreatePackages#naming-your-package
    * suggest the id split if over 25 chars with no "-" in the id
    * flag on "."" in name (unless .portable/.install)

#### Guidelines
Guidelines are strong suggestions that improve the quality of a package version. These are considered something to fix for next time to increase the quality of the package. Over time Guidelines can become Requirements. A package version can be approved without addressing Guideline comments but will reduce the quality of the package.

* Trial software should include the #trial tag. (will become a requirement in Feb 2016)
* Software that requires a license should include a tag #license. (will become a requirement in Feb 2016)
* LicenseUrl is nearly a requirement. The only reason it sits in guidelines is that not all software has a url out there containing its license information. We request that in those cases they point to the url for the FOSS license of the software, if they have an open license.
* We really want to see the IconUrl being used, and some moderators want to see it being used properly, using the rawgit CDN. However it is a guideline, and something to note for a maintainer to fix up next time, not currently. Some software doesn't have a proper icon.
* Suggest description get really filled out and they take full advantage of the use of markdown.
* Summary is important, but it doesn't show up on the package page.
* Tags could always use suggestions to add.
* Look over the package files
  * **Install / Uninstall Scripts:**
    * Be familiar with things that have been deprecated and add a gentle reminder about those things for them to clean up.
    * Are the commented lines from the template in there? Those should be cleaned up. It is not required to remove all comments, some comments are helpful. It’s a bit subjective on what is helpful and what is noise.
* Something in the releaseNotes section would be great.
