Minimal repro of an issue in dbt-core: transitive local dependency is resolved from
the directory of the endmost dbt project, not from the directory of a dependency
whose `packages.yml` specifies the correct location.

- `a/`
  - An empty dbt project that depends on `common1`, which then depends on `common2`.
  - `dbt deps` command succeeds since `common2` can be resolved from `a` by `../common2`.
- `b/c/`
  - Another empty dbt project that depends on `common1`, which then depends on `common2`.
  - Running `dbt deps` in `b/c` fails with the following error, because `../common2` cannot be resolved.

    ```
    06:14:46  Running with dbt=1.1.1
    06:14:47  Encountered an error:
    Runtime Error
      no dbt_project.yml found at expected path /some/path/to/dbt_local_dep_path_issue/b/common2/dbt_project.yml
    ```
