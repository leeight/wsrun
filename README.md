# Workspace script runner

Run npm scripts in a yarn workspace, like a boss.

### Usage:

```
wsrun cmd [<package>] [options]

Options:
  Mode (choose one):
    --parallel                      fully parallel mode (default)
    --stages                        run in stages; start with packages that have no deps.
    --serial                        same as "stages", but with no parallelism at the stage level

  Individual package opts:
    -r, --recursive                 execute the same script on all of its dependencies, too

  Misc:
    --fast-exit                     if at least one script exits with code > 0, abort
    --collect-output                collect per-package stdout, print everything at the end, grouped
    --no-prefix                     don't prefix output
    --bin=yarn                      which program should we pass the script to (default yarn)
    --done-criteria=regex           consider the process "done" when output line matches regex
    --exclude pkgname               skip actually running the script for that package
    --exclude-missing               skip packages which lack the specified script
    --report                        show an execution report once all scripts are finished
```

### Examples:

`yarn wsrun watch` will run `yarn watch` on every individual package, in parallel.

`yarn wsrun build --stages` will build all packages, in stages, starting from those that don't
depend on other packages.

`yarn wsrun watch planc -r` will watch planc and all of its dependencies.

`yarn wsrun watch planc -r --exclude planc` will watch all of planc's dependencies but not planc

`yarn wsrun build h4zip -r --stages` will build all the deps. in order, then build h4zip

`yarn wsrun watch planc -r --stages --done-criteria='Compilation complete'` will watch planc deps,
in order, continuing when command outputs "Compilation complete"

`yarn wsrun clean` will remove "build" folders in every package.

`yarn wsrun test` will test every package.

`yarn wsrun test --exclude-missing` will run the test script only on packages that have it
