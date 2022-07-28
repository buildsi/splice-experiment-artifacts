# Spliced Experiment Artifacts

![https://avatars.githubusercontent.com/u/85255731?s=200&v=4](https://avatars.githubusercontent.com/u/85255731?s=200&v=4)

This repository does an automated retrieval of results from [splice-experiment-runs](https://github.com/buildsi/splice-experiment-runs). It used to derive from [build-abi-containers](https://github.com/builsi/build-abi-containers), a (previously written) GitHub pipeline that I wrote, but we didn't use.
Now I'm back to GitHub because HPC never seems to work :) The process of retrieving artifacts works via the [GitHub workflow](.github/workflows/artifacts.yml) that runs the script [get_artifacts.py](get_artifacts.py) nightly to do the following:

1. Get a listing of artifacts from the repository that saves test results for spliced as artifacts.
2. For artifacts that are not expired (they expire typically in 90 days) and that are within 2 days of today (we don't need to parse artifacts more than once) do the following:
 - If the filename does not exist under [artifacts](artifacts), which is organized by `<tester>/<tester-version>/<package>/<package-version>/<result-type>`, copy it there. It's a new result.
 - If the filename already exists, compare the hashes of the two. If the hashes are different, only copy the artifact file if it's newer (it's a new version of a result).

This creates a nice structured tree of results, where the namespace is nicely organized
by tester, tester version, package version, and result types within the tester.
