# UI

The default login is:

**Username** - admin  
**Password** - <CHECK_THE_LOGS_FOR_RANDOM_GENERATED_PASSWORD>

# Admin password

This recipe does not set an admin password. When FileBrowser Quantum starts
for the first time and no admin user exists yet, it creates one named `admin`
(see `adminUsername` in the recipe) with a randomly generated password and
writes that password to the log of the pod.

To read it, you can either use the openmediavault Workbench or the command
line:

- In the Workbench, go to `Services | Kubernetes | Resources | Pods`, select
  the `filebrowser-quantum` pod and click the `Logs` action button at the top
  of the datatable.
- On the command line, run:

  ```
  kubectl logs -n filebrowser-quantum-app deploy/filebrowser-quantum
  ```

Log in with the password found in the log. Look for the line that says
`Generated initial admin password for user "admin"`; the password is printed
at the end of that line.

The password is only generated once, when the database is created. It is
stored in the database (`database.sqlite`) inside the data shared folder, so it
survives pod restarts. Change it in the web UI after the first login. If you
lose it, you have to remove the database from the data shared folder and let
the application create a new one.
