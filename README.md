![Static Badge](https://img.shields.io/badge/Activity_Status-Archived-red)

## Overview

The PopHIVE project established its own GitHub organization in the summer of 2025. Most codebases were transferred to this new organization in August, unless otherwise noted. For long-term storage, a bare clone of each repository was created and saved in this repository. After completing the transfers to the [PopHIVE](https://github.com/PopHIVE) GitHub Organization, the original codebases in the [YSPH Data Science and Data Equity](https://github.com/ysph-dsde) (YSPH-DSDE) GitHub Organization were deleted.

Refer to the notes below for details on how the archives were created and instructions on how to use them. The new locations of the codebases can also be found in the table below.

&nbsp;

| Repository Name  | New Location                                          | Transferred                   |
|:-----------------|:------------------------------------------------------|:------------------------------|
| data-gov         | [PopHIVE/data-gov](https://github.com/PopHIVE/data-gov)              | Aug 11th, 2025 |
| DSDE-PopHIVE     | [PopHIVE/DSDE-PopHIVE](https://github.com/PopHIVE/DSDE-PopHIVE)      | Aug 11th, 2025 |
| PopHIVE          | [PopHIVE/PopHIVE](https://github.com/PopHIVE/PopHIVE)                | Aug 11th, 2025 |
| PopHIVE-Prototype| [PopHIVE/PopHIVE-Prototype ](https://github.com/PopHIVE/PopHIVE-Prototype)| Aug 12th, 2025 |
| PopHIVE-Shiny    | [PopHIVE/PopHIVE-Shiny](https://github.com/PopHIVE/PopHIVE-Shiny)    | Aug 12th, 2025 |
| PopHIVE_DataHub  | N/A | Not Yet Transferred |

&nbsp;

![GitHub Repo stars](https://img.shields.io/github/stars/ysph-dsde/PopHIVE-Archival) ![GitHub watchers](https://img.shields.io/github/watchers/ysph-dsde/PopHIVE-Archival) ![GitHub forks](https://img.shields.io/github/forks/ysph-dsde/PopHIVE-Archival) [![License: CC BY-NC 4.0](https://img.shields.io/badge/License-CC%20BY--NC%204.0-lightgrey.svg)](http://creativecommons.org/licenses/by-nc/4.0/)

![GitHub Issues or Pull Requests](https://img.shields.io/github/issues/ysph-dsde/PopHIVE-Archival) ![GitHub Issues or Pull Requests](https://img.shields.io/github/issues-pr/ysph-dsde/PopHIVE-Archival) ![GitHub Release Date](https://img.shields.io/github/release-date/ysph-dsde/PopHIVE-Archival) ![GitHub repo size](https://img.shields.io/github/repo-size/ysph-dsde/PopHIVE-Archival)

![GitHub contributors](https://img.shields.io/github/contributors/ysph-dsde/PopHIVE-Archival) ![GitHub last commit](https://img.shields.io/github/last-commit/ysph-dsde/PopHIVE-Archival) ![GitHub commit activity](https://img.shields.io/github/commit-activity/w/ysph-dsde/PopHIVE-Archival) ![GitHub language count](https://img.shields.io/github/languages/count/ysph-dsde/PopHIVE-Archival) ![GitHub top language](https://img.shields.io/github/languages/top/ysph-dsde/PopHIVE-Archival)

## Archival Process

To archive the codebases a bare clone was saved prior to removing the GitHub repository.

``` {.bash filename="Command-Line Application"}
cd "/file_location/"
git clone --bare git@github.com:ysph-dsde/data-gov.git
```

Some of the repositories’ bare clone files contained large `*.pack` files that prevented uploading to this GitHub repository. To resolve this issue, the files were unpacked and repacked with a size limit imposed.

``` {.bash filename="Command-Line Application"}
cd "ORIGINAL-REPOSITORY.git/objects/pack"       # Open directory with the pack object
cat pack-xxxx.pack | git unpack-objects -q      # Unpack "pack-xxxx.pack"
git repack -a -d --max-pack-size=50m            # Repack the object with the file size limit imposed
```

This will generate one `*.pack`, `*.idx` and `*.rev` file of the same name for each split version of the original pack file. To undo this, and recompile the files into one, do the following:

``` {.bash filename="Command-Line Application"}
git repack -a -d -f --max-pack-size=999999999   # Repack into one file without size limit
```

Note that, you do not have to specify the size depending on your global settings.

``` {.bash filename="Command-Line Application"}
git config --global --get pack.packSizeLimit    # Verify the current global setting
git config --global pack.packSizeLimit 50m      # OPTIONAL: change to 50 MB if the limit is too large or not set
git repack -a -d -f                             # Repack using the global setting
```

Sometimes, these operations will generate a `pack-xxxx.bitmap` file. Bitmap indexes accelerate Git version control operations by providing an efficient way to determine reachable objects from a set of commits. This is useful for large repositories and can significantly speed up operations like cloning and fetching.

`pack-xxxx.bitmap` is not a required file and can be deleted. If you decide you want to include one, you can manually add it using:

``` {.bash filename="Command-Line Application"}
git repack -a -d -f --write-bitmap-index        # Repack using the global setting and add bitmap
```

## Re-Initiate an Using Archived File

To create a new GitHub repository using archived bare clone copy, use the following steps.

1. Download the `*.git` bare clone file for the repository you want to reopen.

2. Move the file into the directory you want to save the project on your local device.

3. In your command-line application, navigate into the bare clone file.

    ``` {.bash filename="Command-Line Application"}
    cd "filepath/ORIGINAL-REPOSITORY.git"
    ```

4. Search GitHub, log-in, and select the drop-down menu in the top-right of the page navigation bar to create a “New repository”.

5. Fill out the following sections:

    a. Adjust the GitHub account owner as needed and create the name for the new repository.

    b. It is good practice to initially set the repository to “Private”.

    c. Do _NOT_ use a template or include a description, README.md, .gitignore, or license.

6. In the newly created GitHub repository under “Quick setup” you will find the repository’s SSH key or HTTPS url. Copy this.

7. Back in the command-line application, push a mirror of the cloned git file to your newly created GitHub repository using its SSH key or HTPPS url:

    ``` {.bash filename="Command-Line Application"}
    # SSH
    git push --mirror git@github.com:EXAMPLE-USER/NEW-REPOSITORY.git
    
    # HTTPS
    git push --mirror https://github.com/EXAMPLE-USER/NEW-REPOSITORY.git
    ```

    Refresh the new GitHub repository webpage to confirm the push was successful.

9. Delete the bare cloned file used to create a new remote repository.

    ``` {.bash filename="Command-Line Application"}
    cd ..                                   # Go back one file location
    rm -rf ORIGINAL-REPOSITORY.git          # Delete the bare clone
    ```

**NOTE:** If any of these repositories do not include the [Creative Commons Attribution-Non Commercial 4.0 International](https://creativecommons.org/licenses/by-nc/4.0/) license or “Legal Disclaimer” statement, please be aware that they still apply to all codebase. We ask that you add them as needed.

## Legal Disclaimer

These data and PopHIVE statistical outputs are provided "as is", without warranty of any kind, express or implied, including but not limited to the warranties of merchantability, fitness for a particular purpose, and noninfringement. In no event shall the authors, contributors, or copyright holders be liable for any claim, damages, or other liability, whether in an action of contract, tort, or otherwise, arising from, out of, or in connection with the data or the use or other dealings in the data.

The PopHIVE statistical outputs are research tools intended for use in the fields of public health and medicine. They are not intended for clinical decision making, are not intended to be used in the diagnosis or treatment of patients and may not be useful or appropriate for any clinical purpose. Users of the PopHIVE statistical outputs should be aware of their responsibilities to ensure the ethical and appropriate use of this technology, including adherence to any applicable legal and regulatory requirements.

The content and data provided with the statistical outputs do not replace the expertise of healthcare professionals. Healthcare professionals should use their professional judgment in evaluating the outputs of the PopHIVE statistical outputs.


