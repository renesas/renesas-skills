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

+---------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Option                    | Details                                                                                                                                                                                                                                     |
+---------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-uSelect=``             | |  Description: Specify connection type when using J-Link.                                                                                                                                                                                  |
|                           | |  Default Values: None. The J-Link will be selected automatically.                                                                                                                                                                         |
|                           | |  Emulator: SEGGERJLINKRX                                                                                                                                                                                                                  |
|                           | |  Available Values: Using USB: USB=<J-Link Serial number>, Using IP via LAN: IP=<Host Name/IP address: port number>, Using IP via tunnel: IP=tunnel                                                                                        |
|                           | |  Example: Select USB with serial number 000051000569: ``"serverParameters": { "-uSelect=": "USB=000051000569" }``, Select IP with address 127.0.0.1 and port 61234: ``"serverParameters": { "-uSelect=": "IP=127.0.0.1:61234" }``         |
|                           | |  Note: If using IP via tunnel, it's needed to specify the following parameters.                                                                                                                                                           |
+---------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-uJLinkLog=``           | |  Description: Specify the log file for the J-Link to output logging information. If the log file already exists, the contents of the current log file will be overwritten. If the option is empty, no logging information is outputted.   |
|                           | |  Default Values: None                                                                                                                                                                                                                     |
|                           | |  Emulator: SEGGERJLINKRX                                                                                                                                                                                                                  |
|                           | |  Available Values: Any valid path to log file. The log file path must not contain any blank space characters.                                                                                                                             |
|                           | |  Example: ``"serverParameters": { "-uJLinkLog=": "D:/dev_test/e2studio/workspace/project/JLinkLog.log" }``                                                                                                                                |
|                           | |  Note: J-Link web control panel is prioritized over this configuration. The override option for log file in J-Link web control panel must be disabled before using this parameter.                                                        |
+---------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Clock
-----

+---------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------+
| Option                          | Details                                                                                                                                   |
+---------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------+
| ``-uClockSrcHoco=``             | |  Description: Specify the Main Clock Source.                                                                                            |
|                                 | |  Default Values: 0                                                                                                                      |
|                                 | |  Emulator: E1, E20, E2, E2LITE, SEGGERJLINKRX                                                                                           |
|                                 | |  Available Values: 0: EXTAL, 1: HOCO                                                                                                    |
|                                 | |  Example: ``"serverParameters": { "-uClockSrcHoco=": 0 }``                                                                              |
+---------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------+
| ``-uInputClock=``               | |  Description: Specify EXTAL frequency in MHz.                                                                                           |
|                                 | |  Default Values: None                                                                                                                   |
|                                 | |  Emulator: E1, E20, E2, E2LITE, SEGGERJLINKRX                                                                                           |
|                                 | |  Available Values: Between 0.001 to 99.999                                                                                              |
|                                 | |  Example: ``"serverParameters": { "-uInputClock=": 27.0 }``                                                                             |
+---------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------+
| ``-uAllowClockSourceInternal=`` | |  Description: Specify whether or not to allow manipulation of the main clock source by debugger when internal flash memory is rewritten.|
|                                 | |  Default Values: 1                                                                                                                      |
|                                 | |  Emulator: E1, E20, E2, E2LITE, SEGGERJLINKRX                                                                                           |
|                                 | |  Available Values: 0: No, 1: Yes                                                                                                        |
|                                 | |  Example: ``"serverParameters": { "-uAllowClockSourceInternal=": 1 }``                                                                  |
+---------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------+
| ``-uPTimerClock=``              | |  Description: Specify the operating frequency which is used to calculate the performance counter.                                       |
|                                 | |  Default Values: None                                                                                                                   |
|                                 | |  Emulator: E1, E20, E2, E2LITE, SEGGERJLINKRX                                                                                           |
|                                 | |  Available Values: The frequency in Hz in range 1000 - 999999000                                                                        |
|                                 | |  Example: ``"serverParameters": { "-uPTimerClock=": 120000000 }``                                                                       |
+---------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------+
| ``-uCpuClkFreq=``               | |  Description: Specify the CPU clock frequency in MHz.                                                                                   |
|                                 | |  Default Values: Depend on the target device                                                                                            |
|                                 | |  Emulator: Simulator                                                                                                                    |
|                                 | |  Available Values: The frequency in MHz                                                                                                 |
|                                 | |  Example: ``"serverParameters": { "-uCpuClkFreq=": 12 }``                                                                               |
+---------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------+
| ``-uPeripheralClkRatio=``       | |  Description: Specifies the clock division ratio applied to the system clock to generate the peripheral clock.                          |
|                                 | |  Default Values: 1                                                                                                                      |
|                                 | |  Emulator: Simulator                                                                                                                    |
|                                 | |  Available Values: 1, 2, 3, 4, 6, 8, 12, 16, 24, 32, 48, 64                                                                             |
|                                 | |  Example: ``"serverParameters": { "-uPeripheralClkRatio=": 1 }``                                                                        |
+---------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------+

Connection with Target Board
----------------------------

+----------------------+---------------------------------------------------------------------------------------------------------------------------------+
| Option               | Details                                                                                                                         |
+----------------------+---------------------------------------------------------------------------------------------------------------------------------+
| ``-sn=``             | |  Description: Select the emulator to connect.                                                                                 |
|                      | |  Default Values: Auto (Emulator will be selected automatically.)                                                              |
|                      | |  Emulator: E1, E20, E2, E2Lite                                                                                                |
|                      | |  Available Values: Emulator information with format: <emulator type>:<serial number>                                          |
|                      | |  Example: ``"serverParameters": { "-sn=": "e2:_9es002310c" }``                                                                |
+----------------------+---------------------------------------------------------------------------------------------------------------------------------+
| ``-uUseFine=``       | |  Description: Specifies the connection type to be used when by the emulator when communicating with the CPU on target system. |
|                      | |  Default Values: 0                                                                                                            |
|                      | |  Emulator: E1, E20, E2, E2LITE, SEGGERJLINKRX                                                                                 |
|                      | |  Available Values: 0: JTag, 1: Fine                                                                                           |
|                      | |  Example: ``"serverParameters": { "-uUseFine=": 0 }``                                                                         |
+----------------------+---------------------------------------------------------------------------------------------------------------------------------+
| ``-uJTagClockFreq=`` | |  Description: Specify JTAG communication speed between the emulator and the CPU on target system.                             |
|                      | |  Default Values: Depend on the debugger type                                                                                  |
|                      | |  Emulator: E1, E20, E2, E2LITE, SEGGERJLINKRX                                                                                 |
|                      | |  Available Values:                                                                                                            |
|                      | |                    E2Lite: 6.00, 3.00, 1.50                                                                                   |
|                      | |                    E2: 16.5, 12.5, 6.25, 3.125, 1.562                                                                         |
|                      | |                    E1, E20, SEGGERJLINKRX: 16.5, 12.38, 6.188, 3.094, 1.547                                                   |
|                      | |  Example: ``"serverParameters": { "-uJTagClockFreq=": 6.00 }``                                                                |
+----------------------+---------------------------------------------------------------------------------------------------------------------------------+
| ``-uFineBaudRate=``  | |  Description: Specify FINE communication speed between the emulator and the CPU on target system.                             |
|                      | |  Default Values: Depend on the debugger type                                                                                  |
|                      | |  Emulator: E1, E20, E2, E2LITE, SEGGERJLINKRX                                                                                 |
|                      | |  Available Values:                                                                                                            |
|                      | |                    E2Lite: 1.50, 0.75, 0.50, 0.25                                                                             |
|                      | |                    E1, E20, E2, SEGGERJLINKRX: 2.00, 0.75, 0.5, 0.25                                                          |
|                      | |  Example: ``"serverParameters": { "-uFineBaudRate=": 1.50 }``                                                                 |
+----------------------+---------------------------------------------------------------------------------------------------------------------------------+
| ``-uHotPlug=``       | |  Description: Specify if connecting to an already running target system.                                                      |
|                      | |  Default Values: 0                                                                                                            |
|                      | |  Emulator: E1, E20, E2, E2LITE                                                                                                |
|                      | |  Available Values: 0: No, 1: Yes                                                                                              |
|                      | |  Example: ``"serverParameters": { "-uHotPlug=": 1 }``                                                                         |
+----------------------+---------------------------------------------------------------------------------------------------------------------------------+

Power
-----

+------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Option     | Details                                                                                                                                                                       |
+------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-w``     | |  Description: Select whether to supply power to the target board from emulator.                                                                                             |
|            | |  Default Values: 0                                                                                                                                                          |
|            | |  Emulator: E1, E20, E2, E2Lite                                                                                                                                              |
|            | |  Available Values: 0, 1, 2                                                                                                                                                  |
|            | |  Example: ``"serverParameters": { "-w": 1 }``                                                                                                                               |
|            | |  Note: Value ``2`` is just supported for E2 emulator.                                                                                                                       |
+------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-z``     | |  Description: Specify the power voltage supplied to the target board.                                                                                                       |
|            | |  Default Values: 0                                                                                                                                                          |
|            | |  Emulator: E1, E20, E2, E2Lite                                                                                                                                              |
|            | |  Available Values: 0: 3.3 V, 1: 5.0 V, From 18 to 50: From 1.8 V to 5.0 V                                                                                                   |
|            | |  Example: ``"serverParameters": { "-z": 0 }``: using for 3.3 V, ``"serverParameters": { "-z": 1 }``: using for 5.0 V, ``"serverParameters": { "-z": 18 }``: using for 1.8 V |
|            | |  Note: E2Lite emulator is just supported 3.3 V. Just E2 emulator allows to specify a arbitrary value from 1.8 V to 5.0 V                                                    |
+------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

CPU Operating Mode
------------------

+--------------------------+---------------------------------------------------------------------------------------------------------------+
| Option                   | Details                                                                                                       |
+--------------------------+---------------------------------------------------------------------------------------------------------------+
| ``-uRegisterSetting=``   | |  Description: Specify the operation mode set by registers.                                                  |
|                          | |  Default Values: 0                                                                                          |
|                          | |  Emulator: Simulator, E1, E20, E2, E2LITE, SEGGERJLINKRX                                                    |
|                          | |  Available Values: 0: "Single Chip", 1: "On-Chip ROM Ext. On", 2: "On-Chip ROM Ext. Off"                    |
|                          | |  Example: ``"serverParameters": { "-uRegisterSetting=": 0 }``                                               |
+--------------------------+---------------------------------------------------------------------------------------------------------------+
| ``-uModePin=``           | |  Description: Specify the operation mode set by mode pins.                                                  |
|                          | |  Default Values: 0                                                                                          |
|                          | |  Emulator: Simulator, E1, E20, E2, E2LITE, SEGGERJLINKRX                                                    |
|                          | |  Available Values: 0: Single-chip mode, 1: User boot mode                                                   |
|                          | |  Example: ``"serverParameters": { "-uModePin=": 0 }``                                                       |
+--------------------------+---------------------------------------------------------------------------------------------------------------+
| ``-uChangeStartupBank=`` | |  Description: Specify if the start up bank needs to be changed, when the target device supports "Dual Mode".|
|                          | |  Default Values: 0                                                                                          |
|                          | |  Emulator: E1, E20, E2, E2LITE                                                                              |
|                          | |  Available Values: 0: No, 1: Yes                                                                            |
|                          | |  Example: ``"serverParameters": { "-uChangeStartupBank=": 0 }``                                             |
+--------------------------+---------------------------------------------------------------------------------------------------------------+
| ``-uStartupBank=``       | |  Description: Specify the start up bank when it needs to be changed.                                        |
|                          | |  Default Values: Bank 0                                                                                     |
|                          | |  Emulator: E1, E20, E2, E2LITE                                                                              |
|                          | |  Available Values: Bank 0, Bank 1                                                                           |
|                          | |  Example: ``"serverParameters": { "-uStartupBank=": "BANK_0" }``                                            |
+--------------------------+---------------------------------------------------------------------------------------------------------------+
| ``-uCacheDecode=``       | |  Description: Enables or disables cache decoding.                                                           |
|                          | |  Default Values: 1                                                                                          |
|                          | |  Emulator: Simulator                                                                                        |
|                          | |  Available Values: 0: No, 1: Yes                                                                            |
|                          | |  Example: ``"serverParameters": { "-uCacheDecode=": 1 }``                                                   |
+--------------------------+---------------------------------------------------------------------------------------------------------------+

Communications Mode
-------------------

+-----------------------+-----------------------------------------------------------------------------------------------------+
| Option                | Details                                                                                             |
+-----------------------+-----------------------------------------------------------------------------------------------------+
| ``-uDebugMode=``      | |  Description: Specify the emulation mode.                                                         |
|                       | |  Default Values: 0                                                                                |
|                       | |  Emulator: E1, E20, E2, E2LITE, SEGGERJLINKRX                                                     |
|                       | |  Available Values: 0: "Debug Mode", 1: "Write On Chip Flash Memory"                               |
|                       | |  Example: ``"serverParameters": { "-uDebugMode=": 0 }``                                           |
+-----------------------+-----------------------------------------------------------------------------------------------------+
| ``-uExecuteProgram=`` | |  Description: Specify if the user program should be executed after disconnecting the debugger.    |
|                       | |  Default Values: 0                                                                                |
|                       | |  Emulator: E1, E20, E2, E2LITE, SEGGERJLINKRX                                                     |
|                       | |  Available Values: 0: No, 1: Yes                                                                  |
|                       | |  Example: ``"serverParameters": { "-uExecuteProgram=": 0 }``                                      |
+-----------------------+-----------------------------------------------------------------------------------------------------+

Flash
-----

+---------------+---------------------------------------------------------------------------------------------+
| Option        | Details                                                                                     |
+---------------+---------------------------------------------------------------------------------------------+
| ``-uIdCode=`` | |  Description: Enter ID code to unlock the flash memory from the protected state.          |
|               | |  Default Values: FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF                                         |
|               | |  Emulator: E1, E20, E2, E2LITE, SEGGERJLINKRX                                             |
|               | |  Available Values: 32 hex digit                                                           |
|               | |  Example: ``"serverParameters": { "-uIdCode=": "FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF" }``     |
+---------------+---------------------------------------------------------------------------------------------+

Debug Tool Settings
===================

General Debug
-------------
+------------------------+------------------------------------------------------------------------+
| Option                 | Details                                                                |
+------------------------+------------------------------------------------------------------------+
| ``-uresetOnReload=``   | |  Description: Select whether to reset the program after downloaded.  |
|                        | |  Default Values: 1                                                   |
|                        | |  Emulator: E1, E20, E2, E2LITE, SEGGERJLINKRX                        |
|                        | |  Available Values: 0: No, 1: Yes                                     |
|                        | |  Example: ``"serverParameters": { "-uresetOnReload=": 1 }``          |
+------------------------+------------------------------------------------------------------------+


Memory
------

+------------------------------+---------------------------------------------------------------------------------------------------+
| Option                       | Details                                                                                           |
+------------------------------+---------------------------------------------------------------------------------------------------+
| ``-uWorkRamAddress=``        | |  Description: Specify the first address of the working RAM area for debugger firmware.          |
|                              | |  Default Values: Depends on the target device                                                   |
|                              | |  Emulator: Simulator, E1, E20, E2, E2LITE, SEGGERJLINKRX                                        |
|                              | |  Available Values: Internal RAM area                                                            |
|                              | |  Example: ``"serverParameters": { "-uWorkRamAddress=": "0x1000" }``                             |
+------------------------------+---------------------------------------------------------------------------------------------------+
| ``-uverifyOnWritingMemory=`` | |  Description: Select whether to verify data when writing to memory.                             |
|                              | |  Default Values: 0                                                                              |
|                              | |  Emulator: E1, E20, E2, E2LITE, SEGGERJLINKRX                                                   |
|                              | |  Available Values: 0: No, 1: Yes                                                                |
|                              | |  Example: ``"serverParameters": { "-uverifyOnWritingMemory=": 0 }``                             |
+------------------------------+---------------------------------------------------------------------------------------------------+

System
------

+--------------------------+---------------------------------------------------------------------------------------------------------------------+
| Option                   | Details                                                                                                             |
+--------------------------+---------------------------------------------------------------------------------------------------------------------+
| ``-uProgReWriteIRom=``   | |  Description: Specify whether or not the program being debugged rewrites the internal program ROM.                |
|                          | |  Default Values: 0                                                                                                |
|                          | |  Emulator: E1, E20, E2, E2LITE, SEGGERJLINKRX                                                                     |
|                          | |  Available Values: 0: No, 1: Yes                                                                                  |
|                          | |  Example: ``"serverParameters": { "-uProgReWriteIRom=": 0 }``                                                     |
+--------------------------+---------------------------------------------------------------------------------------------------------------------+
| ``-uProgReWriteDFlash=`` | |  Description: Specify whether or not to debug a program that involves rewriting the internal data flash.          |
|                          | |  Default Values: 0                                                                                                |
|                          | |  Emulator: E1, E20, E2, E2LITE, SEGGERJLINKRX                                                                     |
|                          | |  Available Values: 0: No, 1: Yes                                                                                  |
|                          | |  Example: ``"serverParameters": { "-uProgReWriteDFlash=": 0 }``                                                   |
+--------------------------+---------------------------------------------------------------------------------------------------------------------+


Start/Stop Function
-------------------

+------------------------------+-------------------------------------------------------------------------------------------------------+
| Option                       | Details                                                                                               |
+------------------------------+-------------------------------------------------------------------------------------------------------+
| ``-uhookEnableStart=``       | |  Description: Specify if a function should be executed before running user program.                 |
|                              | |  Default Values: 0                                                                                  |
|                              | |  Emulator: E1, E20, E2, E2LITE                                                                      |
|                              | |  Available Values: 0: No, 1: Yes                                                                    |
|                              | |  Example: ``"serverParameters": { "-uhookEnableStart=": 0 }``                                       |
+------------------------------+-------------------------------------------------------------------------------------------------------+
| ``-uhookStartFunc=``         | |  Description: Specify the address of the start function.                                            |
|                              | |  Default Values: 0                                                                                  |
|                              | |  Emulator: E1, E20, E2, E2LITE                                                                      |
|                              | |  Available Values: Any valid hex address.                                                           |
|                              | |  Example: ``"serverParameters": { "-uhookStartFunc=": "&R_BSP_ChangeToUserMode" }``                 |
|                              | |  Note: This parameter is just sent when allow to execute function before running user program       |
+------------------------------+-------------------------------------------------------------------------------------------------------+
| ``-uhookEnableStop=``        | |  Description: Specify if a function should be executed after stopping user program.                 |
|                              | |  Default Values: 0                                                                                  |
|                              | |  Emulator: E1, E20, E2, E2LITE                                                                      |
|                              | |  Available Values: 0: No, 1: Yes                                                                    |
|                              | |  Example: ``"serverParameters": { "-uhookEnableStop=": 0 }``                                        |
+------------------------------+-------------------------------------------------------------------------------------------------------+
| ``-uhookStopFunc=``          | |  Description: Specify the address of the stop function.                                             |
|                              | |  Default Values: None                                                                               |
|                              | |  Emulator: E1, E20, E2, E2LITE                                                                      |
|                              | |  Available Values: Any valid hex address.                                                           |
|                              | |  Example: ``"serverParameters": { "-uhookStopFunc=": "&R_BSP_InterruptRead" }``                     |
|                              | |  Note: This parameter is just sent when allow to execute function after stopping user program       |
+------------------------------+-------------------------------------------------------------------------------------------------------+
| ``-uhookWorkRamAddr=``       | |  Description: Specify the first address of the working RAM area for start/stop function.            |
|                              | |  Default Values: 0                                                                                  |
|                              | |  Emulator: E1, E20, E2, E2LITE                                                                      |
|                              | |  Available Values: Internal RAM area                                                                |
|                              | |  Example: ``"serverParameters": { "-uhookWorkRamAddr=": "0x20001000" }``                            |
+------------------------------+-------------------------------------------------------------------------------------------------------+

Execution Mode
--------------

+------------------------+-------------------------------------------------------------------------------------------------------+
| Option                 | Details                                                                                               |
+------------------------+-------------------------------------------------------------------------------------------------------+
| ``-uEmStopInt=``       | |  Description: Enables to stop execution when an INT instruction is executed.                        |
|                        | |  Default Values: 1                                                                                  |
|                        | |  Emulator: Simulator                                                                                |
|                        | |  Available Values: 0: No, 1: Yes                                                                    |
|                        | |  Example: ``"serverParameters": { "-uEmStopInt=": 1 }``                                             |
+------------------------+-------------------------------------------------------------------------------------------------------+
| ``-uEmStopInterrupt=`` | |  Description: Enables to stop execution when an interrupt is encountered.                           |
|                        | |  Default Values: 1                                                                                  |
|                        | |  Emulator: Simulator                                                                                |
|                        | |  Available Values: 0: No, 1: Yes                                                                    |
|                        | |  Example: ``"serverParameters": { "-uEmStopInterrupt=": 1 }``                                       |
+------------------------+-------------------------------------------------------------------------------------------------------+
| ``-uEmStopFPexp=``     | |  Description: Enables to stop execution when a floating-point exception is encountered.             |
|                        | |  Default Values: 1                                                                                  |
|                        | |  Emulator: Simulator                                                                                |
|                        | |  Available Values: 0: No, 1: Yes                                                                    |
|                        | |  Example: ``"serverParameters": { "-uEmStopFPexp=": 1 }``                                           |
+------------------------+-------------------------------------------------------------------------------------------------------+

RTOS
----

+----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Option               | Details                                                                                                                                                    |
+----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-uOSRestriction=`` | |  Description: When debugging a project with a large number of OS threads the Debug View will always check the status of every thread. Selecting Yes will |
|                      | |  force the view to only check the current thread. This speeds up debugger responsiveness.                                                                |
|                      | |  Default Values: 0                                                                                                                                       |
|                      | |  Emulator: Simulator, E2, E2LITE                                                                                                                         |
|                      | |  Available Values: 0: No, 1: Yes                                                                                                                         |
|                      | |  Example: ``"serverParameters": { "-uOSRestriction=": 1 }``                                                                                              |
|                      | |  Note: ``"-uOSRestriction=": 0``: Do not restrict OS thread checking, ``"-uOSRestriction=": 1``: Restrict OS thread checking to the current thread       |
+----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------+