# Build ABI Containers Results

![https://avatars.githubusercontent.com/u/85255731?s=200&v=4](https://avatars.githubusercontent.com/u/85255731?s=200&v=4)

This repository does an automated retrieval of results from [build-abi-containers](https://github.com/buildsi/build-abi-containers).
This works via the [GitHub workflow](.github/workflows/artifacts.yml) that runs
the script [get_artifacts.py](get_artifacts.py) nightly to do the following:

1. Get a listing of artifacts from [build-abi-containers](https://github.com/builsi/build-abi-containers), which saves test results for containers (a tester base like libabigail combined with multiple versions of a particular spack package) as artifacts.
2. For artifacts that are not expired (they expire typically in 90 days) and that are within 2 days of today (we don't need to parse artifacts more than once) do the following:
 - If the filename does not exist under [artifacts](artifacts), which is organized by `<tester>/<tester-version>/<package>/<package-version>/<result-type>`, copy it there. It's a new result.
 - If the filename already exists, compare the hashes of the two. If the hashes are different, only copy the artifact file if it's newer (it's a new version of a result).

This creates a nice structured tree of results, where the namespace is nicely organized
by tester, tester version, package version, and result types within the tester. For example, for libabigail
we likely want to run `abidw` on some set of libraries and this is saved under `lib`. For
`abidiff` we save results under a `diff` folder. Given that the library can compile an example
and run `abicompat`, that gets saved under `compat`. If you want to edit the specific versions
that are tested, and the libraries that data is extracted for, you should edit the files
in the [tests](https://github.com/buildsi/build-abi-containers/tree/main/tests) folder of the
main repository. Updating any of those files will automatically deploy a new container and generate
a new artifact that will be discovered here. If you want to update a tester base, the files
are also located in that repository.

Once we have more than one tester, we probably want to think about how to compare results between them.
But likely first we need to finish our tool [Smeagle](https://github.com/buildsi/Smeagle) to be able
to run tests here, and also to figure out what "experiments" we want to run. Likely we want
to extend the test capability beyond comparing multiple versions of the same package within
a container, so that is something to talk about too. Please [open an issue](https://github.com/buildsi/build-abi-containers-results/issues)
if you have a question or point of discussion. 
