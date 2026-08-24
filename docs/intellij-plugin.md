# IntelliJ plugin

Lewis ODB runs an IntelliJ Java Application configuration under the Omniscient Debugger. IntelliJ keeps the Run console and process controls. ODB records the program and opens its own Swing windows.

## Requirements

- IntelliJ IDEA build 252 or later on macOS, Windows, or Linux
- A local JDK 8
- A desktop graphics environment
- A class with `public static void main(String[])`

The selected run configuration must be a local Java Application. Version 1 does not support test, Gradle, Maven, remote, compound, Android, WSL, container, JPMS module-path, or target JDK 9 and later configurations.

## Install

1. Open the [Lewis ODB 1.0.0 release](https://github.com/LewisODB/ODB_IntelliJ/releases/tag/v1.0.0).
2. Download `Lewis-ODB-1.0.0-signed.zip` and `SHA256SUMS`.
3. Verify the ZIP. Its SHA-256 is:

   ```text
   2fce1f0a9b97cb246b554670fc151e7728622ac157d1d1cb2e596a412cc31998
   ```

   On macOS:

   ```shell
   shasum -a 256 Lewis-ODB-1.0.0-signed.zip
   ```

   On Linux:

   ```shell
   sha256sum Lewis-ODB-1.0.0-signed.zip
   ```

   On Windows PowerShell:

   ```powershell
   Get-FileHash .\Lewis-ODB-1.0.0-signed.zip -Algorithm SHA256
   ```

4. In IntelliJ, open **Settings > Plugins**.
5. Open the settings menu, choose **Install Plugin from Disk**, and select `Lewis-ODB-1.0.0-signed.zip`.
6. Restart IntelliJ if prompted.

Do not extract the plugin ZIP.

## Prepare a run configuration

You can use an existing [Application run configuration](https://www.jetbrains.com/help/idea/run-debug-configuration-java-application.html) or create one:

1. Open **Run > Edit Configurations**.
2. Select an existing **Application** configuration, or add a new one.
3. Set **Run on** to the local machine.
4. Select a main class with `public static void main(String[])`.
5. Set **JRE** to a local JDK 8.
6. Set **Use classpath of module** to the application module.

Program arguments, VM options, environment variables, the working directory, and before-launch tasks still come from this configuration. Lewis ODB does not edit it.

## Run with ODB

1. Select the Java Application configuration in the run widget.
2. Open the executor menu beside the widget.
3. Choose **Run with ODB**.
4. Watch the IntelliJ Run console. A successful startup ends with:

   ```text
   ODB recording started.
   ODB debugger ready.
   ```

The plugin starts the application through the bundled ODB launcher. The ODB Controller opens while the application records. When the application ends, or when you choose **Stop Recording** in the controller, the debugger opens with the recording.

IntelliJ passes the selected module's source roots to ODB. Source normally opens without a prompt. If ODB cannot find a file, use its source chooser to select the matching `.java` file.

Use IntelliJ's **Stop** button to end the whole ODB session. This stops the application, closes the ODB windows, and removes the session's temporary state. Closing the project also stops its active ODB session.

## Read a recording

- **Method Traces** shows recorded method calls. Select a call to move to it.
- **Code** shows the current source line. Its arrow buttons move backward or forward through recorded execution, step over, step into or out of calls, and move between repeated visits to a line.
- **Locals** and **this** show values at the selected time.
- **Objects** keeps selected objects available for inspection. Double-click an object value to add it.
- **Console** shows recorded output. Select a line to move to the time when it was written.

For a first pass, select a method in **Method Traces**, choose a variable in **Locals** or **Objects**, then use the variable's back and forward buttons to see when its value changed. The toolbar at the top moves across all recorded timestamps. The [user manual](user-manual/index.md) covers the rest of the debugger.

## Troubleshooting

| Message or symptom | Fix |
| --- | --- |
| **Run with ODB** is missing | Select a Java Application configuration. Other configuration types do not offer this executor. Check that Lewis ODB is enabled under **Settings > Plugins > Installed**. |
| `supports target JDK 8 only` | Open **Run > Edit Configurations** and set the configuration's **JRE** to a local JDK 8. IntelliJ's own runtime can be newer. |
| `supports local Java Application processes only` | Set **Run on** to the local machine. WSL, Docker, SSH, and other remote targets are not supported. |
| `does not support Java module-path applications` | Use a classpath-based Java Application configuration. JPMS module-path launch is not supported. |
| `cannot resolve the selected application's main class` | Select a main class in the Java Application configuration and make sure its module is loaded. |
| `requires public static void main(String[])` | Use a conventional Java main method. Implicit classes are not supported. |
| `requires a local desktop graphics environment` | Run IntelliJ in a desktop session with graphics available. Headless sessions are not supported. |
| `bundled ODB runtime is missing or unreadable` | Reinstall the signed plugin ZIP and restart IntelliJ. |
| ODB asks for a source file | Select the matching `.java` file. Then check that the file belongs to the module selected by the run configuration. |
| The process exits before `ODB debugger ready.` | Read the Lewis ODB notification and Run console. If the target produced no usable recording or instrumentation failed, report the message and target class through [GitHub Issues](https://github.com/LewisODB/ODB_IntelliJ/issues). |

## Update or uninstall

To update, download the newer signed ZIP and checksum from [GitHub Releases](https://github.com/LewisODB/ODB_IntelliJ/releases), then install the ZIP from disk and restart IntelliJ if prompted.

To uninstall, open **Settings > Plugins > Installed**, select **Lewis ODB**, open the disable menu, and choose **Uninstall**.
