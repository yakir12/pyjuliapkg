# JuliaPkg

Do you want to use [Julia](https://julialang.org/) in your Python script/project/package?
No problem! JuliaPkg will help you out!
- Declare the version of Julia you require in a `juliapkg.json` file.
- Add any packages you need too.
- Call `juliapkg.resolve()` et voila, your dependencies are there.
- Use `juliapkg.executable()` to find the Julia executable and `juliapkg.project()` to
  find the project where the packages were installed.
- Virtual environments? PipEnv? Poetry? Conda? No problem! JuliaPkg will set up a
  different project for each environment you work in, keeping your dependencies isolated.

## Install

```sh
pip install juliapkg
```

## Declare dependencies

### Functional interface

- `status(target=None)` shows the status of dependencies.
- `require_julia(version, target=None)` declares that you require the given version of
  Julia. The `version` is a Julia compat specifier, so `1.5` matches any `1.*.*` version at
  least `1.5`.
- `add(pkg, uuid, dev=False, version=None, path=None, url=None, rev=None, target=None)`
  adds a required package. Its name and UUID are required.
- `rm(pkg, target=None)` remove a package.

Note that these functions edit `juliapkg.json` but do not actually install anything until
`resolve()` is called, which happens automatically in `executable()` and `project()`.

The `target` specifies the `juliapkg.json` file to edit, or the directory containing it.
If not given, it will be your virtual environment or Conda environment if you are using one,
otherwise `~/.pyjuliapkg.json`.

### juliapkg.json

You can also edit `juliapkg.json` directly if you like. Here is an example which requires
Julia v1.*.* and the Example package v0.5.*:
```json
{
    "julia": "1",
    "packages": {
        "Example": {
            "uuid": "7876af07-990d-54b4-ab0e-23690620f79a",
            "version": "0.5",
        }
    }
}
```

## Using Julia

- `juliapkg.executable()` returns a compatible Julia executable.
- `juliapkg.project()` returns the project into which the packages have been installed.
- `juliapkg.resolve(force=False)` ensures all the depdencies are installed. You don't
  normally need to do this because the other functions resolve automatically.

## Details

### Adding Julia dependencies to Python packages

JuliaPkg looks for `juliapkg.json` files in many locations, namely:
- Your virtual environment or Conda environment, if you have one.
- `~/.pyjuliapkg.json` otherwise.
- Every directory and direct sub-directory in `sys.path`.

The last point means that if you put a `juliapkg.json` file in a package, then install
that package, then JuliaPkg will find those dependencies and install them.

You can use `add`, `rm` etc. above with `target='/path/to/your/package'` to modify the
dependencies of your package.
