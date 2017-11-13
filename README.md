# PyInstaller Docker Image

This is a container to build python scripts into windows exes on linux.

## Tag

`brimstone/pyinstallerx` has only `:latest`

This runs Python 3.4 and pyinstaller 3.2.1

## Usage

This container runs Wine inside Ubuntu to emulate Windows in Docker.

To build your application, you need to mount your source code into the `/src/`
volume.

The source code directory should have your `.spec` file that PyInstaller
generates. If you don't have one, you'll need to run PyInstaller once locally to
generate it.

If the `src` folder has a `requirements.txt` file, the packages will be
installed into the environment before PyInstaller runs.

For example, in the folder that has your source code, `.spec` file and
`requirements.txt`:

```
docker run -v "$(pwd):/src/" brimstone/pyinstaller-windows
```

will build your PyInstaller project into `dist/windows/`. The `.exe` file will have the same name as your `.spec` file.

##### How do I install system libraries or dependencies that my Python packages need?

You'll need to supply a custom command to Docker to install system pacakges. Something like:

```
docker run -v "$(pwd):/src/" brimstone/pyinstaller "apt-get update -y && apt-get install -y wget && pip install -r requirements.txt && pyinstaller --clean -y --dist ./dist/windows --workpath /tmp *.spec"
```

Replace `wget` with the dependencies / package(s) you need to install.

##### How do I generate a .spec file?

`docker run -v "$(pwd):/src/" brimstone/pyinstaller pyinstaller your-script.py`

will generate a `spec` file for `your-script.py` in your current working directory. See the PyInstaller docs for more information.

##### How do I change the PyInstaller version used?

Add `pyinstaller=3.3` to your `requirements.txt`.

##### Is it possible to use a package mirror?

Yes, by supplying the `PYPI_URL` and `PYPI_INDEX_URL` environment variables that point to your PyPi mirror.

## Credits

Thanks to https://github.com/cdrx/docker-pyinstaller for the original work.
