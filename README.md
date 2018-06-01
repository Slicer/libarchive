What is this project ?
----------------------

This project is **NOT** the official libarchive repository.

It is a fork of libarchive sources hosted at https://github.com/libarchive/libarchive.

It is used as staging area to maintain and test patches that will be contributed back to the
official repository.


What is the branch naming convention ?
--------------------------------------

Each branch is named following the pattern `slicer-vY.Y.Z-YYYY-MM-DD-SHA{N}`

where:

* `vX.Y.Z` is the version of the forked project
* `YYYY-MM-DD` is the date of the last official commit associated with the branch.
* `SHA{N}` are the first N characters of the last official commit associated with the branch.

For more details, see https://www.slicer.org/wiki/Documentation/Nightly/Developers/ProjectForks


How to update the version of libarchive ?
-----------------------------------------

1. Clone this repository and add a remote to the official project

```
git clone git://github.com/Slicer/libarchive
cd libarchive
git remote add upstream git://github.com/libarchive/libarchive
git fetch upstream
```

2. Checkout base of the new branch

```
git checkout -b master upstream/master # You could also checkout a specific tag
```

3. Create a new branch following the convention

```
# Extract version
XYZ=$(cat libarchive/archive.h | grep "define\sARCHIVE_VERSION_ONLY_STRING" | cut -d" " -f2 | sed -re "s/\"(.+)dev\"/\\1/" | sed -re "s/\"(.+)\"/\\1/")
echo "XYZ [${XYZ}]"

DATE=$(git show -s --format=%ci HEAD | cut -d" " -f1)
echo "DATE [${DATE}]"

SHA=$(git show -s --format=%h HEAD)
echo "SHA [${SHA}]"

BRANCH_NAME=slicer-v${XYZ}-${DATE}-${SHA}
echo "BRANCH_NAME [${BRANCH_NAME}]"

git checkout -b ${BRANCH_NAME} ${SHA}
```

4. Cherry-pick the Slicer specific commits from last branch. Resolve conflict as needed.

5. To **test the changes**, locally rebuild Slicer.

6. Publish the branch. (directly in this repo if you have push rights, or on a fork)

7. Update Slicer libarchive external project and submit a pull request.


How to be granted push rights ?
-------------------------------

Ask on https://discourse.slicer.org/


Questions
---------

If you have questions, see https://discourse.slicer.org/

