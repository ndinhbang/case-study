1. Install pyenv-win: https://github.com/pyenv-win/pyenv-win

    ```
    Invoke-WebRequest -UseBasicParsing -Uri "https://raw.githubusercontent.com/pyenv-win/pyenv-win/master/pyenv-win/install-pyenv-win.ps1" -OutFile "./install-pyenv-win.ps1"; &"./install-pyenv-win.ps1"
    ```
2. Run `pyenv install -l` to check a list of Python versions supported by pyenv-win
3. Run `pyenv install <version>` to install the supported version
4. Go to project directory and set python version locally

   ```
   mkdir my_project && cd my_project
   pyenv local <version>
   ```

5. Create `venv` virtual environment and activate it
   ```
   python -m venv env
   env\Scripts\activate
   ```
6. Install `pipenv` inside virtual environment
   
    ```
    pip install pipenv
    ```

7. Set up `pipenv` in your project
   
   ```
   pipenv install
   ```

   this command will generate 2 files: `Pipfile` & `Pipfile.lock`

8. Install packages as usual, for example:
    ```
    pipenv install requests
    ```
9. If you are done working in the virtual environment for the moment, you can deactivate it
    ```
    deactivate
    ```

10. In order to keep your environment consistent, it’s a good idea to “freeze” the current state of the environment packages. To do this, run:
    ```
    pip freeze > requirements.txt
    ```
    This will create a requirements.txt file, which contains a simple list of all the packages in the current environment, and their respective versions. You can see the list of installed packages without the requirements format using `pip list`

11. Later it will be easier for a different developer (or you, if you need to re-create the environment) to install the same packages using the same versions
    ```
    cd my_project
    pip install -r requirements.txt
    ```
