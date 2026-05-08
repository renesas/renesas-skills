######################
 Mandatory parameters
######################

The mandatory parameters required to connect to the target device are automatically provided to the Renesas GDB server by the adapter. Users do not need to define or send these parameters manually.

+------------+-------------------------------------------------------------------------------------------------+
| Option     | Details                                                                                         |
+------------+-------------------------------------------------------------------------------------------------+
| ``-g``     | |  Description: Specify the emulator type.                                                      |
|            | |  Available Values: E1, E20, E2, E2LITE, EZ, IECUBE, COMPORT, SEGGERJLINKARM, or SEGGERJLINKRX.|
|            | |  Example: ``"serverParameters": { "-g": "E2" }``                                              |
+------------+-------------------------------------------------------------------------------------------------+
| ``-t``     | |  Description: Specify the target device name.                                                 |
|            | |  Available Values: Any valid Target Device name for the selected emulator.                    |
|            | |  Example: ``"serverParameters": { "-t": "R7FA6E10F" }``                                       |
+------------+-------------------------------------------------------------------------------------------------+

#############################
 Renesas GDB Server Settings
#############################

+--------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Option                   | Details                                                                                                                                                                                               |
+--------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-c``                   | |  Description: Add arguments from the specific file to Renesas GDB server.                                                                                                                           |
|                          | |  Default Values: None                                                                                                                                                                               |
|                          | |  Available Values: The path to argument file                                                                                                                                                        |
|                          | |  Example: ``"serverParameters": { "-c": "D:\\argument.txt" }``                                                                                                                                      |
+--------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-p``                   | |  Description: Specify the port number that Renesas GDB server will open.                                                                                                                            |
|                          | |  Default Values: If this parameter is not sent to Renesas GDB server, a random available port will be chosen.                                                                                       |
|                          | |  Available Values: Valid available port number                                                                                                                                                      |
|                          | |  Example: ``"serverParameters": { "-p": 61234 }``                                                                                                                                                   |
+--------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-m``                   | |  Description: Specify whether there is no thread on launch or not.                                                                                                                                  |
|                          | |  Default Values: No                                                                                                                                                                                 |
|                          | |  Available Values: Does not have a value. Sending this parameter means "Yes"                                                                                                                        |
|                          | |  Example: ``"serverParameters": { "-m": true }``                                                                                                                                                    |               
+--------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-n``                   | |  Description: Specify the type of data endian.                                                                                                                                                      |
|                          | |  Default Values: 0                                                                                                                                                                                  |
|                          | |  Available Values: 0: Little Endian, 1: Big Endian                                                                                                                                                  |
|                          | |  Example: ``"serverParameters": { "-n": 0 }``                                                                                                                                                       |
+--------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``--english``            | |  Description: Specify language to English.                                                                                                                                                          |
|                          | |  Example: ``"serverParameters": { "--english": true }``                                                                                                                                             |
+--------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``--japanese``           | |  Description: Specify language to Japanese.                                                                                                                                                         |
|                          | |  Example: ``"serverParameters": { "--japanese": true }``                                                                                                                                            |
+--------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-uConnectionTimeout=`` | |  Description: Specify the connection timeout value in seconds. If the connection from GDB server is not established within the timeout value, Renesas GDB server will cancel the connection process.|
|                          | |  Default Values: 0                                                                                                                                                                                  |
|                          | |  Available Values: Non-negative integer, less than 300 (seconds)                                                                                                                                    |
|                          | |  Example: ``"serverParameters": { "-uConnectionTimeout=": 30 }``                                                                                                                                    |
|                          | |  Note: If value of this parameter is 0, it means Renesas GDB server will wait infinitely.                                                                                                           |
+--------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-uSyncMode=``          | |  Description: Specify the synchronization between cores.                                                                                                                                            |
|                          | |  Default Values: sync                                                                                                                                                                               |
|                          | |  Available Values: sync: Synchronous mode, async: Asynchronous mode                                                                                                                                 |
|                          | |  Example: ``"serverParameters": { "-uSyncMode=": "async" }``                                                                                                                                        |
+--------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-uCore=``              | |  Description: Specify core's information of the target device to Renesas GDB server.                                                                                                                |
|                          | |  Default Values: None                                                                                                                                                                               |
|                          | |  Available Values: Core's information with format: <core name>|<status>|<thread>|<GDB port>                                                                                                         |
|                          | |  Example: ``"serverParameters": { "-uCore=": "CR52CPU0|enabled|264|61234" }``                                                                                                                       |
|                          | |  Note: If the core status value is not sent, this core status will be set to "enabled" as default.                                                                                                  |
+--------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. note::

   **Core's information details**

   - **Format**: ``<core name>|<status>|<thread>|<GDB port>``
   - **core name**:
     - If it is a single core: ``SINGLE_CORE``.
     - Otherwise: ``<cortex family><order of CPU>``, example: ``CR52CPU0`` means cortex CR52, CPU 0.
   - **status**:
     - ``enabled`` – The core is enabled.
     - ``disabled`` – The core is disabled.
     - ``freerunning`` – The core is free running.
   - **thread**:
     - Specify the number of threads the core is running in.
   - **GDB port**:
     - Specify the GDB port number.

Connection Settings
===================

J-Link
------
+---------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Option                    | Details                                                                                                                                                                                                                  |
+===========================+==========================================================================================================================================================================================================================+
| ``-uSelect=``             | |  Description: Specify the connection type when using J-Link.                                                                                                                                                           |
|                           | |  Default Value: USB (auto-selected)                                                                                                                                                                                    |
|                           | |  Emulator: SEGGERJLINKRISCV                                                                                                                                                                                            |
|                           | |  Available Values: USB, IP                                                                                                                                                                                             |
|                           | |  Example: ``"serverParameters": { "-uSelect=": "USB" }``, ``"serverParameters": { "-uSelect=": "IP" }``                                                                                                                |
+---------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-uJLinkSerial=``        | |  Description: Select the USB-connected SEGGERJLINKRISCV to connect to.                                                                                                                                                 |
|                           | |  Default Value: Auto                                                                                                                                                                                                   |
|                           | |  Emulator: SEGGERJLINKRISCV                                                                                                                                                                                            |
|                           | |  Available Values: Any connected SEGGERJLINKRISCV serial number.                                                                                                                                                       |
|                           | |  Example: ``"serverParameters": { "-uSelect=": "USB=000051000569" }``                                                                                                                                                  |
+---------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-uJLinkSetting=``       | |  Description: Specify the file path to the SEGGERJLINKRISCV settings file. The SEGGERJLINKRISCV debugger will store its default settings to this file.                                                                 |
|                           | |  Default Value: ``.jlink`` file stored in the project folder, named after the active configuration.                                                                                                                    |
|                           | |  Emulator: SEGGERJLINKRISCV                                                                                                                                                                                            |
|                           | |  Available Values: Any valid path to a ``.jlink`` file.                                                                                                                                                                |
|                           | |  Example: ``"serverParameters": { "-uJLinkSetting=": "D:/dev_test/workspace/ra6t1/ra6t1 Debug_Flat.jlink" }``                                                                                                          |
+---------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-uJLinkScript=``        | |  Description: Specify the file path to a SEGGERJLINKRISCV script file to run.                                                                                                                                          |
|                           | |  Default Value: None                                                                                                                                                                                                   |
|                           | |  Emulator: SEGGERJLINKRISCV                                                                                                                                                                                            |
|                           | |  Available Values: Any valid path to a script file.                                                                                                                                                                    |
|                           | |  Example: ``"serverParameters": { "-uJLinkScript=": "D:/dev_test/workspace/riscv/riscv.JLinkScript" }``                                                                                                                |
+---------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-uJLinkLog=``           | |  Description: Specify the log file for SEGGERJLINKRISCV to output logging information. If the log file already exists, its contents will be overwritten. If the option is empty, no logging information is outputted.  |
|                           | |  Default Value: ``JLinkLog.log`` stored in the project folder.                                                                                                                                                         |
|                           | |  Emulator: SEGGERJLINKRISCV                                                                                                                                                                                            |
|                           | |  Available Values: Any valid path to a log file. The path must not contain blank space characters.                                                                                                                     |
|                           | |  Example: ``"serverParameters": { "-uJLinkLog=": "D:/dev_test/e2studio/workspace/project/JLinkLog.log" }``                                                                                                             |
+---------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-uLowPower=``           | |  Description: Enable Low Power Handling mode. Usually the SEGGERJLINKRISCV DLL handles communication losses as errors. Enabling this mode places the DLL in a less restrictive mode.                                   |
|                           | |  Default Value: 0                                                                                                                                                                                                      |
|                           | |  Emulator: SEGGERJLINKRISCV                                                                                                                                                                                            |
|                           | |  Available Values: 0: No, 1: Yes                                                                                                                                                                                       |
|                           | |  Example: ``"serverParameters": { "-uLowPower=": 0 }``                                                                                                                                                                 |
+---------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Host Name/IP Address      | |  Description: Specify the Host Name/IP Address and optional port number for a SEGGERJLINKRISCV remote IP connection.                                                                                                   |
|                           | |  Default Value: None                                                                                                                                                                                                   |
|                           | |  Emulator: SEGGERJLINKRISCV                                                                                                                                                                                            |
|                           | |  Available Values: Any valid Host Name/IP Address, optionally followed by a port number (e.g., ``127.0.0.1:61234``).                                                                                                   |
|                           | |  Example: ``"serverParameters": { "-uSelect=": "IP=127.0.0.1:61234" }``                                                                                                                                                |
+---------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Clock
-----

+---------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------+
| Option                          | Details                                                                                                                                   |
+=================================+===========================================================================================================================================+
| ``-uClockSrc=``                 | |  Description: Select the Main Clock Source.                                                                                             |
|                                 | |  Default Values: 0                                                                                                                      |
|                                 | |  Emulator: E2, E2LITE                                                                                                                   |
|                                 | |  Available Values: 0: Internal, 1: External                                                                                             |
|                                 | |  Example: ``"serverParameters": { "-uClockSrc=": 0 }``                                                                                  |
+---------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------+
| ``-uInputClock=``               | |  Description: Specify External frequency in MHz.                                                                                        |
|                                 | |  Default Values: None                                                                                                                   |
|                                 | |  Emulator: E2, E2LITE                                                                                                                   |
|                                 | |  Available Values: Between 0.001 to 99.999                                                                                              |
|                                 | |  Example: ``"serverParameters": { "-uInputClock=": 16.00 }``                                                                            |
+---------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------+
| ``-uAllowClockSourceInternal=`` | |  Description: Specify whether or not to allow manipulation of the main clock source by debugger when internal flash memory is rewritten.|
|                                 | |  Default Values: 1                                                                                                                      |
|                                 | |  Emulator: E2, E2LITE                                                                                                                   |
|                                 | |  Available Values: 0: No, 1: Yes                                                                                                        |
|                                 | |  Example: ``"serverParameters": { "-uAllowClockSourceInternal=": 1 }``                                                                  |
+---------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------+

Connection with Target Board
----------------------------

+-----------------+----------------------------------------------------------------------------------------------------------------------------------+
| Option          | Details                                                                                                                          |
+=================+==================================================================================================================================+
| ``-sn=``        | |  Description: Specify the emulator serial number to be connected.                                                              |
|                 | |  Default Values: Auto (Emulator will be selected automatically.)                                                               |
|                 | |  Emulator: E2, E2LITE                                                                                                          |
|                 | |  Available Values: Emulator information with format: <emulator type>:<serial number>                                           |
|                 | |  Example: ``"serverParameters": { "-sn=": "e2:_9es002310c" }``                                                                 |
+-----------------+----------------------------------------------------------------------------------------------------------------------------------+
| ``-uInteface=`` | |  Description: Specifies the connection type to be used by the emulator when communicating with the CPU on target system.       |
|                 | |  Default Values: cJTAG                                                                                                         |
|                 | |  Emulator: E2, E2LITE                                                                                                          |
|                 | |  Available Values: 0: Fixed as cJTAG                                                                                           |
|                 | |  Example: ``"serverParameters": { "-uInteface=": "cJTAG" }``                                                                   |
+-----------------+----------------------------------------------------------------------------------------------------------------------------------+
| ``-uIfSpeed=``  | |  Description: Specifies the speed.                                                                                             |
|                 | |  Default Values: auto                                                                                                          |
|                 | |  Emulator: E2, E2LITE                                                                                                          |
|                 | |  Available Values: Auto, Enter a value (integer value).                                                                        |
|                 | |  Example: ``"serverParameters": { "-uIfSpeed=": "auto" }`` or ``"serverParameters": { "-uIfSpeed=": 1000 }``                   |
+-----------------+----------------------------------------------------------------------------------------------------------------------------------+

Interface
---------

+---------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Option                    | Details                                                                                                                                                                   |
+===========================+===========================================================================================================================================================================+
| ``-uInteface=``           | |  Description: Select the target interface type.                                                                                                                         |
|                           | |  Default Value: cJTAG                                                                                                                                                   |
|                           | |  Emulator: SEGGERJLINKRISCV                                                                                                                                             |
|                           | |  Available Values: cJTAG, JTAG                                                                                                                                          |
|                           | |  **Example:** ``"serverParameters": { "-uInteface=": "cJTAG" }``                                                                                                        |
+---------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-uIfSpeed=``            | |  Description: Set the JTAG interface speed.                                                                                                                             |
|                           | |  Default Value: Auto                                                                                                                                                    |
|                           | |  Emulator: SEGGERJLINKRISCV                                                                                                                                             |
|                           | |  Available Values: Auto, Adaptive, Enter a value (integer value)                                                                                                        |
|                           | |  Example: ``"serverParameters": { "-uIfSpeed=": "auto" }`` or ``"serverParameters": { "-uIfSpeed=": "adaptive" }`` or ``"serverParameters": { "-uIfSpeed=": 4000 }``    |
+---------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Power
-----

+------------+-------------------------------------------------------------------------------------------------------------------------------------------------------+
| Option     | Details                                                                                                                                               |
+============+=======================================================================================================================================================+
| ``-w``     | |  Description: Specify whether to power the target from the emulator (MAX 200mA) and the power source.                                               |
|            | |  Default Value: 0                                                                                                                                   |
|            | |  Emulator: E2, E2LITE                                                                                                                               |
|            | |  Available Values: 0, 1, 2                                                                                                                          |
|            | |  Example: ``"serverParameters": { "-w": 1 }``                                                                                                       |
|            | |  Note: Value ``2`` is only supported for the E2 emulator.                                                                                           |
+------------+-------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-z``     | |  Description: Specify the supply voltage (V) provided to the target board. The value is adjusted to one decimal place (e.g., 3.56 → 3.6).           |
|            | |  Default Value: 33 (3.3 V)                                                                                                                          |
|            | |  Emulator: E2, E2LITE                                                                                                                               |
|            | |  Available Values: 16–55 (corresponding to 1.6 V–5.5 V)                                                                                             |
|            | |  Example:                                                                                                                                           |
|            | |    ``"serverParameters": { "-z": 16 }`` for 1.6 V,                                                                                                  |
|            | |    ``"serverParameters": { "-z": 33 }`` for 3.3 V,                                                                                                  |
|            | |    ``"serverParameters": { "-z": 50 }`` for 5.0 V                                                                                                   |
|            | |  Note: E2LITE emulator only supports 3.3 V. Only the E2 emulator allows specifying an arbitrary voltage from 1.6 V to 5.5 V.                        |
+------------+-------------------------------------------------------------------------------------------------------------------------------------------------------+

Connection
----------

+-----------------------------+-----------------------------------------------------------------------------------------------------------------+
| Option                      | Details                                                                                                         |
+=============================+=================================================================================================================+
| ``-uInitRegisters=``        | |  Description: Specify whether registers should be initialized on connection.                                  |
|                             | |  Default Value: 0                                                                                             |
|                             | |  Emulator: SEGGERJLINKRISCV                                                                                   |
|                             | |  Available Values: 0: No, 1: Yes                                                                              |
|                             | |  Example: ``"serverParameters": { "-uInitRegisters=": 0 }``                                                   |
+-----------------------------+-----------------------------------------------------------------------------------------------------------------+
| ``-uResetBeginConnection=`` | |  Description: Specify whether the CPU should be reset at the start of connection.                             |
|                             | |  Default Value: 1                                                                                             |
|                             | |  Emulator: SEGGERJLINKRISCV                                                                                   |
|                             | |  Available Values: 0: No, 1: Yes                                                                              |
|                             | |  Example: ``"serverParameters": { "-uResetBeginConnection=": 0 }``                                            |
+-----------------------------+-----------------------------------------------------------------------------------------------------------------+
| ``-uNoReset=``              | |  Description: Specify whether the CPU should be reset at the end of connection.                               |
|                             | |  Default Value: 0                                                                                             |
|                             | |  Emulator: SEGGERJLINKRISCV                                                                                   |
|                             | |  Available Values: 0: No, 1: Yes                                                                              |
|                             | |  Example: ``"serverParameters": { "-uNoReset=": 1 }``                                                         |
+-----------------------------+-----------------------------------------------------------------------------------------------------------------+
| ``-uResetBefDownload=``     | |  Description: Specify whether an additional reset should be applied before the module is downloaded.          |
|                             | |  Default Value: 0                                                                                             |
|                             | |  Emulator: SEGGERJLINKRISCV                                                                                   |
|                             | |  Available Values: 0: No, 1: Yes                                                                              |
|                             | |  Example: ``"serverParameters": { "-uResetBefDownload=": 1 }``                                                |
+-----------------------------+-----------------------------------------------------------------------------------------------------------------+
| ``-uResetPreRun=``          | |  Description: Specify whether an additional reset should be applied after the module is downloaded.           |
|                             | |  Default Value: 1                                                                                             |
|                             | |  Emulator: SEGGERJLINKRISCV                                                                                   |
|                             | |  Available Values: 0: No, 1: Yes                                                                              |
|                             | |  Example: ``"serverParameters": { "-uResetPreRun=": 1 }``                                                     |
+-----------------------------+-----------------------------------------------------------------------------------------------------------------+
| ``-uIdCodeBytes=``          | |  Description: Specify the ID code to unlock the device (if one has been set previously to protect connection).|
|                             | |  Default Values: FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF                                                             |
|                             | |  Emulator: E2, E2LITE, SEGGERJLINKRISCV                                                                       |
|                             | |  Available Values: 32 hex digits                                                                              |
|                             | |  Example: ``"serverParameters": { "-uIdCodeBytes=": "FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF" }``                    |
+-----------------------------+-----------------------------------------------------------------------------------------------------------------+
| ``-uLowPower=``             | |  Description: Specify if the program uses low power consumption mode or not.                                  |
|                             | |  Default Values: 1                                                                                            |
|                             | |  Emulator: E2, E2LITE                                                                                         |
|                             | |  Available Values: 0: No, 1: Yes                                                                              |
|                             | |  Example: ``"serverParameters": { "-uLowPower=": 1 }``                                                        |
+-----------------------------+-----------------------------------------------------------------------------------------------------------------+

Debug Tool Settings
===================

General Debug
-------------
+------------------------+------------------------------------------------------------------------+
| Option                 | Details                                                                |
+========================+========================================================================+
| ``-uresetOnReload=``   | |  Description: Select whether to reset the program after downloaded.  |
|                        | |  Default Values: 1                                                   |
|                        | |  Emulator: E2, E2LITE, SEGGERJLINKRISCV                              |
|                        | |  Available Values: 0: No, 1: Yes                                     |
|                        | |  Example: ``"serverParameters": { "-uresetOnReload=": 1 }``          |
+------------------------+------------------------------------------------------------------------+

Break
-----

+--------------------+--------------------------------------------------------------------------------------------------------------------------------+
| Option             | Details                                                                                                                        |
+====================+================================================================================================================================+
| ``-uFlashBp=``     | |  Description: Specify whether to use software breakpoints to flash memory or not.                                            |
|                    | |  Default Values: 0                                                                                                           |
|                    | |  Emulator: E2, E2LITE, SEGGERJLINKRISCV                                                                                      |
|                    | |  Available Values: 0: No, 1: Yes                                                                                             |
|                    | |  Example: ``"serverParameters": { "-uFlashBp=": 0 }``                                                                        |
+--------------------+--------------------------------------------------------------------------------------------------------------------------------+
| ``-uSimulation=``  | |  Description: Specify if Instruction Set simulation should be used when setting breakpoints in Flash.                        |
|                    | |  Default Values: 0                                                                                                           |
|                    | |  Emulator: SEGGERJLINKRISCV                                                                                                  |
|                    | |  Available Values: 0: No, 1: Yes                                                                                             |
|                    | |  Example: ``"serverParameters": { "-uSimulation=": 0 }``                                                                     |
+--------------------+--------------------------------------------------------------------------------------------------------------------------------+

Flash
-----

+--------------------------------+------------------------------------------------------------------------------------------------+
| Option                         | Details                                                                                        |
+================================+================================================================================================+
| ``-uWorkRamAddress=``          | |  Description: Specify the first address of the working RAM area for debugger firmware.       |
|                                | |  Default Values: None                                                                        |
|                                | |  Emulator: E2, E2LITE                                                                        |
|                                | |  Available Values: Internal RAM area                                                         |
|                                | |  Example: ``"serverParameters": { "-uWorkRamAddress=": "0x0" }``                             |
+--------------------------------+------------------------------------------------------------------------------------------------+
| ``-ueraseRomOnDownload=``      | |  Description: Specify whether or not to erase the flash ROM (program ROM) before a download. |
|                                | |  Default Values: 0                                                                           |
|                                | |  Emulator: E2, E2LITE                                                                        |
|                                | |  Available Values: 0: No, 1: Yes                                                             |
|                                | |  Example: ``"serverParameters": { "-ueraseRomOnDownload=": 0 }``                             |
+--------------------------------+------------------------------------------------------------------------------------------------+
| ``-ueraseDataRomOnDownload=``  | |  Description: Specify whether or not to erase the data flash ROM before a download.          |
|                                | |  Default Values: 0                                                                           |
|                                | |  Emulator: E2, E2LITE                                                                        |
|                                | |  Available Values: 0: No, 1: Yes                                                             |
|                                | |  Example: ``"serverParameters": { "-ueraseDataRomOnDownload=": 0 }``                         |
+--------------------------------+------------------------------------------------------------------------------------------------+
| ``-ucfiFlashWorkStart=``       | |  Description: Specify the start address of the Working RAM area.                             |
|                                | |  Default Values: None                                                                        |
|                                | |  Emulator: SEGGERJLINKRISCV                                                                  |
|                                | |  Available Values: Internal RAM area                                                         |
|                                | |  Example: ``"serverParameters": { "-ucfiFlashWorkStart=": "0x0" }``                          |
+--------------------------------+------------------------------------------------------------------------------------------------+
| ``-ucfiFlashWorkEnd=``         | |  Description: Specify the end address of the Working RAM area.                               |
|                                | |  Default Values: None                                                                        |
|                                | |  Emulator: SEGGERJLINKRISCV                                                                  |
|                                | |  Available Values: Internal RAM area                                                         |
|                                | |  Example: ``"serverParameters": { "-ucfiFlashWorkEnd=": "0x0" }``                            |
+--------------------------------+------------------------------------------------------------------------------------------------+

System
------

+--------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------+
| Option                   | Details                                                                                                                                              |
+==========================+======================================================================================================================================================+
| ``-uProgReWriteIRom=``   | |  Description: Specify whether or not the program being debugged rewrites the internal program ROM.                                                 |
|                          | |  Default Values: 0                                                                                                                                 |
|                          | |  Emulator: E2, E2LITE, SEGGERJLINKRISCV                                                                                                            |
|                          | |  Available Values: 0: No, 1: Yes                                                                                                                   |
|                          | |  Example: ``"serverParameters": { "-uProgReWriteIRom=": 0 }``                                                                                      |
+--------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-uProgReWriteDFlash=`` | |  Description: Specify whether or not to debug a program that involves rewriting the internal data flash.                                           |
|                          | |  Default Values: 0                                                                                                                                 |
|                          | |  Emulator: E2, E2LITE                                                                                                                              |
|                          | |  Available Values: 0: No, 1: Yes                                                                                                                   |
|                          | |  Example: ``"serverParameters": { "-uProgReWriteDFlash=": 0 }``                                                                                    |
+--------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-uAllowCachingFlash=`` | |  Description: If this setting is enabled the debugger will cache the contents of flash to increase read speed                                      |
|                          | |  Default Values: 0                                                                                                                                 |
|                          | |  Emulator: SEGGERJLINKRISCV                                                                                                                        |
|                          | |  Available Values: 0: Yes, 1: No                                                                                                                   |
|                          | |  Example: ``"serverParameters": { "-uAllowCachingFlash=": 0 }``                                                                                    |
+--------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------+

Time Measurement
----------------

+------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------+
| Option                       | Details                                                                                                                                              |
+==============================+======================================================================================================================================================+
| ``-uTimeMeasurementEnable=`` | |  Description: Specify whether or not display the measurement result of run-break on the status bar. Emulator will measure the time with 100us unit.|
|                              | |  Default Values: 1                                                                                                                                 |
|                              | |  Emulator: E2, E2LITE, SEGGERJLINKRISCV                                                                                                            |
|                              | |  Available Values: 0: No, 1: Yes                                                                                                                   |
|                              | |  Example: ``"serverParameters": { "-uTimeMeasurementEnable=": 1 }``                                                                                |
+------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------+
