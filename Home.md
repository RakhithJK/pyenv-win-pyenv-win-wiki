[pyenv](https://github.com/pyenv/pyenv) is a tool for simple Python version management.

pyenv is a great tool. We have ported it to Windows.

To install pyenv-win, please refer to the [Readme](https://github.com/pyenv-win/pyenv-win#readme).

## Troubleshooting / FAQ

### How to verify that I have set up pyenv-win correctly?

1. Check that `pyenv` is in your PATH:

    ```pwsh
    where.exe pyenv
    ```

    Two rows are displayed *(for your user)*:

    ```bash
    C:\Users\username\.pyenv\pyenv-win\bin\pyenv
    C:\Users\username\.pyenv\pyenv-win\bin\pyenv.bat
    ```

    If not, see [Finish the installation](https://github.com/pyenv-win/pyenv-win#finish-the-installation) to set up all Environment Variables.

2. Check that `python` is in your PATH:

    ```pwsh
    where.exe python
    ```

    Both **pyenv-win**'s rows at the top of command's output.
    If not, see [Configure the order of PATH variable](#configure-the-order-in-path-variable) below.

### Configure the order in PATH variable

  1. **Simple Way**: Move Up two pyenv-win's paths

      - Open ***Advanced System Properties***:
      - Win + R and run `systempropertiesadvanced`
      - Click on ***Environment Variables..***. button
      - Select **Path** in **User variables** table and click on **Edit...** button
      - Find and select two pyenv-win's rows and move them up clicking **Move Up** button (each row you have to move separately)
      - Click **OK** button twice and RESTART (open new) shell terminal window (terminal).

      See lovely [GIF](./img/user-path-changing-order.gif) *(for understanding text instructions or lazy person :sleeping:)*

  2. **Advanced Way**: Leave only pyenv-win's paths in System PATH

      Instead of changing every time User PATH, you can add two pyenv-win's rows to **System** PATH. They can be at the bottom of the system list. BUT watch out for another paths which higher pyenv-win's paths. They could contain `python.exe`.

      ![Add to System PATH](./img/add-system-path.png)

      In any case, to be sure - restart your shell and check the order by `where.exe python` command.

### Windows PATH priority

Windows detects current (active) Python interpreter from `PATH` Environment Variable. When you type `python` in your favorite shell (Terminal, CMD, PowerShell, etc.) OS searches `python.exe` in PATH records starting from the top (beginning). As soon as the interpreter is found - OS stops search and runs the executable file. That's why the order in PATH matters.

Windows has three scopes of environment variables: System (or Machine), User and Process scope. Interpreter search follows the same sequence: **System PATH -> User PATH -> Process (session) PATH**

[One picture](./img/solution-with-system-path.png) instead of thousand words.

When you set up **pyenv-win** you add two records (paths) at the top of User PATH variable by executing following PowerShell command from [Readme steps](https://github.com/pyenv-win/pyenv-win#finish-the-installation):

<a id="ps-user-path"></a>

```pwsh
[System.Environment]::SetEnvironmentVariable('path', $env:USERPROFILE + "\.pyenv\pyenv-win\bin;" + $env:USERPROFILE + "\.pyenv\pyenv-win\shims;" + [System.Environment]::GetEnvironmentVariable('path', "User"),"User")
```

and everything is fine (works correctly) until you install another Python version from python.org or add path with installed interpreter to System PATH variable. Always check you Pythons paths if something goes wrong.

### pyenv-win is installed but does not run in PowerShell

If you see this error "*... pyenv.ps1 cannot be loaded
because running scripts is disabled on this system ...*":

![PowerShell Execution Policy Error](./img/powershell-execution-policy-error.png)

To fix it, run following command in PowerShell and type 'Yes' for its confirmation.

```pwsh
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

By default PowerShell forbids to run any script file (`.ps1`) due to ***Default*** Execution Policy. You can change it on less restricted one at any time. Reed more about policy types and scopes on [Microsoft Documentation](https://go.microsoft.com/fwlink/?LinkID=135170) website.
