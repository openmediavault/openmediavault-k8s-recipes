> ⚠️ **Warning:** The `Filebrowser` project is on maintenance-only mode. Since the used Helm Chart has not been updated, newer images cannot be used. Therefore, this recipe only uses version v2.32.3. Alternatively, use the `Filebrowser Quantum` recipe.

The default login is:

**Username** - admin  
**Password** - RANDOM_GENERATED_PASSWORD

To get the random password that is automatically generated on first startup of the pod, go to the `Services | Kubernetes | Resources | Pods` page after the recipe has been installed and check the logs of the filebrowser pod for the line that says: `Generated random admin password for quick setup`.
