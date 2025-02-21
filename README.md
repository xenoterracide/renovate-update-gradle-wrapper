# minimal-reproduction-template

https://github.com/renovatebot/renovate/discussions/34273

renovate should create a PR for this repo. If you check out that branch and renovate has done the correct thing  `./gradlew build` should pass and `git diff` will show no changes.

## Current behavior

renovate does not update the wrapper files or locks when updating the wrapper. Currently only the `gradle-wrapper.properties` file is updated.

You will notice after updating that file if you run.

```shell
./gradlew wrapper
```
a file is downloaded and  gradlew` is changed

if you run it again

```shell
./gradlew wrapper
```
`gradlew` will change again

in this repository you will also hit a lock failure because you excluded write-locks.

## Expected behavior

an update of the wrapper needs to be the equivalent of

```shell
./gradew wrapper --gradle-version=8.12.1
./gradlew wrapper --write-locks
git add -u
```
otherwise depdencies created by use of the kotlin-dsl plugin are not propertly locked and the `gradlew`/`gradlew.bat` files are not fully updated (this is an ancient bug in gradle).

While this path will work it can also leave stale dependencies in the lockfile. When I update gradle I actually delete lockfiles first and then completely regenerate them. That's probably not a great path for renovate to take though; might be good to document.

## Link to the Renovate issue or Discussion

https://github.com/renovatebot/renovate/discussions/34273
