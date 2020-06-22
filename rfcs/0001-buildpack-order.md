# Proposed Buildpack Ordering in a Builder

## Summary

`pack inspect-builder gcr.io/paketo-buildpack/builder` should display a `Detection Order` that is deterministic and will never make it impossible for any buildpack to detect.

## Motivation

After a change to the builder detection order it became impossible for `php` apps to detect, because `nginx` and `httpd` were before `php` in the detect order. 
The `httpd.conf` or `nginx.conf` included in `php` apps will cause the `nginx` or `http` buildpacks to detect `true` before `php` ever gets to run `detect`,
making it impossible to access the `php` buildpack.


## Detailed Explanation

We propose the following rule to order buildpacks in the builder's `Detect Order`:

- All meta buildpacks should be come before implementation buildpacks, and sort each group by buildpaco-id.

- To prove this will always be valid: (1)
    - Consider 2 buildpacks A and B.
    - If any of buildpack B's order groups are a subset of any of buildpack A's order groups, then buildpack A must come before buildpack B in the builder order. 
    - An implementation buildpack can be defined as an order group of itself.

Thus, our exiting buildpacks would be ordered as follows:

- paketo-buildpacks/dotnet-core
- paketo-buildpacks/go
- paketo-buildpacks/java
- paketo-buildpacks/nodejs
- paketo-buildpacks/php
- paketo-community/python
- paketo-community/ruby
- paketo-buildpacks/httpd
- paketo-buildpacks/nginx
- paketo-buildpacks/procfile
- paketo-community/staticfile

A tool can be made to validate that a given order is correct. The tool reads `buildpack.toml` of each buildpack and validates the rule in (1).

## Rationale and Alternatives

An alternative solution is to test the buildpack orderings by running the acceptance test suite of each buildpack against the builder. If the order is correct they should all pass.
While this would catch errors in the order group we believe it is inferior to the proposed solution for the following reasons:

- Running all of the tests in parallel would take up a lot of computing resources in what would essentially be a duplication of effort; we already ran the acceptance tests of every buildpack on the builder.
- The reported error would be confusing, rather than failing with `the buildpack ordering is wrong`, one of the test suites would fail and you would have to figure out the reason for that failure.

Simply inspecting the order groups of each buildpack would take up way fewer computer resources, run faster, and provide a helpful error message.

## Implementation

The builder would need to sort each meta buildpack and each implementation buildpack by buildpack-id, and then place all of the meta buildpacks before the implementation buildpacks.

If we chose to make the order verifying tool described above, it would:

- Parse every `buildpack.toml` in order to read every order group.
- Compare each of those order groups to every order group of every other buildpack, while considering an implementation buildpack have one order group made up of itself.
- Return false if, for any two buildpacks, buildpack A has an order group that is a subset of any of buildpack B's order groups, and buildpack A is ordered above buildpack B.

