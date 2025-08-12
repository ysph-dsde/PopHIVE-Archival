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


