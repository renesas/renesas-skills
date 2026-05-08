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

Clock
-----

+------------------+-------------------------------------------------------------------------------------------------------+
| Option           | Details                                                                                               |
+==================+=======================================================================================================+
| ``-umFreq=``     | |  Description: Configure the Main Clock Frequency.                                                   |
|                  | |  Default Value: Dependent on target device.                                                         |
|                  | |  Emulator: E1, E2                                                                                   |
|                  | |  Available Values: 0.0001 to 99.9999 (in MHz). Values up to the second decimal place.               |
|                  | |  Example: ``"serverParameters": { "-umFreq=": 16.00 }``                                             |
+------------------+-------------------------------------------------------------------------------------------------------+

Connection
----------

+----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Option               | Details                                                                                                                                                                       |
+======================+===============================================================================================================================================================================+
| ``-sn=``             | |  Description: Specify the emulator serial number to be connected.                                                                                                           |
|                      | |  Default Values: Auto (Emulator will be selected automatically.)                                                                                                            |
|                      | |  Emulator: E1, E2                                                                                                                                                           |
|                      | |  Available Values: Emulator information with format: <emulator type>:<serial number>                                                                                        |
|                      | |  Example: ``"serverParameters": { "-sn=": "e2:_9es002310c" }``                                                                                                              |
+----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-ulpdType=``       | |  Description: Select the LPD mode for the connection.                                                                                                                       |
|                      | |  Default Value: 4 (LPD 4-pin)                                                                                                                                               |
|                      | |  Emulator: E1, E2                                                                                                                                                           |
|                      | |  Available Values:                                                                                                                                                          |
|                      | |    ``-ulpdType= 0``: LPD 1-pin                                                                                                                                              |
|                      | |    ``-ulpdType= 4``: LPD 4-pin                                                                                                                                              |
|                      | |    ``-ulpdType= 15``: JTag (E2 only)                                                                                                                                        |
|                      | |  Example: ``"serverParameters": { "-ulpdType=": 4 }``                                                                                                                       |
+----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-ulpdFreq=``       | |  Description: Configure the LPD Clock Frequency.                                                                                                                            |
|                      | |  Default Value: Default                                                                                                                                                     |
|                      | |  Emulator: E1, E2                                                                                                                                                           |
|                      | |  Available Values: Default, 3000, 5500, 11000, 16500, 20000, 25000, 33000, 40000, or any value from 0 to 99999 (in kHz).                                                    |
|                      | |  Example: ``"serverParameters": { "-ulpdFreq=": 3000 }`` or ``"serverParameters": { "-ulpdFreq=": "Default" }``                                                             |
|                      | |  Note: Not active if LPD 1-pin is selected for Connection Type.                                                                                                             |
+----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-ulpdBaud=``       | |  Description: Configure the LPD Baud Rate.                                                                                                                                  |
|                      | |  Default Value: Default                                                                                                                                                     |
|                      | |  Emulator: E1, E2                                                                                                                                                           |
|                      | |  Available Values: Default, 500, 1000, 2000, or any value from 0 to 9999 (in Kbps).                                                                                         |
|                      | |  Example: ``"serverParameters": { "-ulpdBaud=": 1000 }`` or ``"serverParameters": { "-ulpdBaud=": "Default" }``                                                             |
|                      | |  Note: Not active if LPD 4-pin is selected for Connection Type.                                                                                                             |
+----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-uJTagClockFreq=`` | |  Description: Configure the JTag Clock Frequency.                                                                                                                           |
|                      | |  Default Value: Default                                                                                                                                                     |
|                      | |  Emulator: E2                                                                                                                                                               |
|                      | |  Available Values: Default, 6250, 11000, 16500, 20000, or any value from 0 to 99999 (in kHz).                                                                               |
|                      | |  Example: ``"serverParameters": { "-uJTagClockFreq=": 6250  }`` or ``"serverParameters": { "-uJTagClockFreq=": "Default" }``                                                |
|                      | |  Note: Available when JTag connection type is selected.                                                                                                                     |
+----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-setOptJtagLpd=``  | |  Description: Specify whether to change option bytes to LPD setting before connecting.                                                                                      |
|                      | |  Default Value: 1                                                                                                                                                           |
|                      | |  Emulator: E1, E2                                                                                                                                                           |
|                      | |  Available Values: 0: No, 1: Yes                                                                                                                                            |
|                      | |  Example: ``"serverParameters": { "-setOptJtagLpd=": 1 }``                                                                                                                  |
+----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-setOptJtagJtag=`` | |  Description: Specify whether to change option bytes to JTAG (Nexus I/F) setting before disconnecting. If No is selected, the option bytes after disconnect is the LPD I/F. |
|                      | |  Default Value: 0                                                                                                                                                           |
|                      | |  Emulator: E1, E2                                                                                                                                                           |
|                      | |  Available Values: 0: No, 1: Yes                                                                                                                                            |
|                      | |  Example: ``"serverParameters": { "-setOptJtagJtag=": 0 }``                                                                                                                 |
+----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-uHotPlug=``       | |  Description: Specify if connecting to an already running target system.                                                                                                    |
|                      | |  Default Value: 0                                                                                                                                                           |
|                      | |  Emulator: E2                                                                                                                                                               |
|                      | |  Available Values: 0: No, 1: Yes                                                                                                                                            |
|                      | |  Example: ``"serverParameters": { "-uHotPlug=": 0 }``                                                                                                                       |
+----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-releaseReset=``   | |  Description: Specifies whether to release the RESET before disconnecting from the target system.                                                                           |
|                      | |  Default Value: 0                                                                                                                                                           |
|                      | |  Emulator: E2                                                                                                                                                               |
|                      | |  Available Values: 0: No, 1: Yes                                                                                                                                            |
|                      | |  Example: ``"serverParameters": { "-releaseReset=": 0 }``                                                                                                                   |
+----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-uInitialStop=``   | |  Description: Specifies whether to debug the initial stop/standby mode.                                                                                                     |
|                      | |  Default Value: 0                                                                                                                                                           |
|                      | |  Emulator: E2                                                                                                                                                               |
|                      | |  Available Values: 0: No, 1: Yes                                                                                                                                            |
|                      | |  Example: ``"serverParameters": { "-uInitialStop=": 0 }``                                                                                                                   |
+----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Power
-----

+----------------------+-----------------------------------------------------------------------------------------------+
| Option               | Details                                                                                       |
+======================+===============================================================================================+
| ``-w``               | |  Description: Select whether to supply power to the target board from emulator (MAX 200mA). |
|                      | |  Default Value: 0                                                                           |
|                      | |  Emulator: E1, E2                                                                           |
|                      | |  Available Values:                                                                          |
|                      | |    ``-w 1``: power target from emulator.                                                    |
|                      | |    ``-w 0``: do not power target from emulator.                                             |
|                      | |  Example: ``"serverParameters": { "-w": 0 }``                                               |
+----------------------+-----------------------------------------------------------------------------------------------+
| ``-usupplyVoltage=`` | |  Description: Select the power voltage supplied to the target board.                        |
|                      | |  Default Value: 0 (3.3 V)                                                                   |
|                      | |  Emulator: E1, E2                                                                           |
|                      | |  Available Values:                                                                          |
|                      | |    ``-usupplyVoltage= 0``: 3.3 V.                                                           |
|                      | |    ``-usupplyVoltage= 1``: 5.5 V.                                                           |
|                      | |  Example: ``"serverParameters": { "-usupplyVoltage=": 0 }``                                 |
+----------------------+-----------------------------------------------------------------------------------------------+

Flash
-----

+---------------------------+----------------------------------------------------------------------------------------------------------+
| Option                    | Details                                                                                                  |
+===========================+==========================================================================================================+
| ``-usecurityID=``         | |  Description: Specify the security ID (32 or 64 hex digits) for device boot.                           |
|                           | |  Default Value: ``FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF`` (dependent on target device).                     |
|                           | |  Emulator: E1, E2                                                                                      |
|                           | |  Available Values: 0 to FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF (32 or 64 hex digits depending on device).    |
|                           | |  Example: ``"serverParameters": { "-usecurityID=": "FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF" }``              |
|                           | |  Note: Depending on the device, switches between 32 digits or 64 digits.                               |
+---------------------------+----------------------------------------------------------------------------------------------------------+
| ``-ucodeFlashID=``        | |  Description: Specify the security ID (64 hex digits) for access to internal code flash memory.        |
|                           | |  Default Value: None                                                                                   |
|                           | |  Emulator: E1, E2                                                                                      |
|                           | |  Available Values: 0 to FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF (64 hex digits).                              |
|                           | |  Example: ``"serverParameters": { "-ucodeFlashID=": "FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF" }``             |
|                           | |  Note: Depending on the device, this option may not be active.                                         |
+---------------------------+----------------------------------------------------------------------------------------------------------+
| ``-udataFlashID=``        | |  Description: Specify the security ID (64 hex digits) for access to internal data flash memory.        |
|                           | |  Default Value: None                                                                                   |
|                           | |  Emulator: E1, E2                                                                                      |
|                           | |  Available Values: 0 to FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF (64 hex digits).                              |
|                           | |  Example: ``"serverParameters": { "-udataFlashID=": "FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF" }``             |
|                           | |  Note: Depending on the device, this option may not be active.                                         |
+---------------------------+----------------------------------------------------------------------------------------------------------+

Security
--------

+---------------------------+----------------------------------------------------------------------------------------------------------+
| Option                    | Details                                                                                                  |
+===========================+==========================================================================================================+
| ``-uneedAuthentication=`` | |  Description: Specify whether to perform authentication in DLL format.                                 |
|                           | |  Default Value: 0                                                                                      |
|                           | |  Emulator: E1, E2                                                                                      |
|                           | |  Available Values: 0: No, 1: Yes                                                                       |
|                           | |  Example: ``"serverParameters": { "-uneedAuthentication=": 0 }``                                       |
+---------------------------+----------------------------------------------------------------------------------------------------------+
| ``-uauthenticationDll=``  | |  Description: Specify the full path of the authentication DLL file.                                    |
|                           | |  Default Value: None                                                                                   |
|                           | |  Emulator: E1, E2                                                                                      |
|                           | |  Available Values: Any valid file path.                                                                |
|                           | |  Example: ``"serverParameters": { "-uauthenticationDll=": "D:/dev_test/auth/authentication.dll" }``    |
+---------------------------+----------------------------------------------------------------------------------------------------------+

SW-OPBT
-------

+----------------------+-------------------------------------------------------------------------------------------+
| Option               | Details                                                                                   |
+======================+===========================================================================================+
| ``-uswOpbtRv=``      | |  Description: Set the Reset Vector for each core.                                       |
|                      | |  Default Value: None                                                                    |
|                      | |  Emulator: E2                                                                           |
|                      | |  Available Values: From 00000000 to FFFFFFFF.                                           |
|                      | |  Example: ``"serverParameters": { "-uswOpbtRv=": "00000000" }``                         |
|                      | |  Note: The number of parameters that need to be set depends on the target device.       |
+----------------------+-------------------------------------------------------------------------------------------+
| ``-uswOpbt=``        | |  Description: Set the OPBT value.                                                       |
|                      | |  Default Value: None                                                                    |
|                      | |  Emulator: E2                                                                           |
|                      | |  Available Values: From 00000000 to FFFFFFFF.                                           |
|                      | |  Example: ``"serverParameters": { "-uswOpbt=": "00000000" }``                           |
|                      | |  Note: The number of parameters that need to be set depends on the target device.       |
+----------------------+-------------------------------------------------------------------------------------------+
| ``-uswOpbtIcum=``    | |  Description: Set the ICUM OPBT value.                                                  |
|                      | |  Default Value: None                                                                    |
|                      | |  Emulator: E2                                                                           |
|                      | |  Available Values: From 00000000 to FFFFFFFF.                                           |
|                      | |  Example: ``"serverParameters": { "-uswOpbtIcum=": "00000000" }``                       |
|                      | |  Note: The number of parameters that need to be set depends on the target device.       |
+----------------------+-------------------------------------------------------------------------------------------+

Debug Tool Settings
===================

General Debug
-------------

+----------------------+-------------------------------------------------------------------------+
| Option               | Details                                                                 |
+======================+=========================================================================+
| ``-uresetOnReload=`` | |  Description: Select whether to reset the program after downloaded.   |
|                      | |  Default Value: 1                                                     |
|                      | |  Emulator: E1, E2                                                     |
|                      | |  Available Values: 0: No, 1: Yes                                      |
|                      | |  Example: ``"serverParameters": { "-uresetOnReload=": 1 }``           |
+----------------------+-------------------------------------------------------------------------+

Break
-----

+-------------------+-------------------------------------------------------------------------------------------------------------+
| Option            | Details                                                                                                     |
+===================+=============================================================================================================+
| ``-ustopPeriEmu=``| |  Description: Select whether to terminate peripheral emulation while stopping the program execution.      |
|                   | |  Default Value: 0                                                                                         |
|                   | |  Emulator: E1, E2                                                                                         |
|                   | |  Available Values: 0: No, 1: Yes                                                                          |
|                   | |  Example: ``"serverParameters": { "-ustopPeriEmu=": 0 }``                                                 |
+-------------------+-------------------------------------------------------------------------------------------------------------+

Mask for Input Signal
---------------------

+-------------------+-------------------------------------------------------------------------------------------------------------+
| Option            | Details                                                                                                     |
+===================+=============================================================================================================+
| ``-umaskReset=``  | |  Description: Configure Reset Signal Masking.                                                             |
|                   | |  Default Value: 0                                                                                         |
|                   | |  Emulator: E1, E2                                                                                         |
|                   | |  Available Values: 0: No, 1: Yes                                                                          |
|                   | |  Example: ``"serverParameters": { "-umaskReset=": 0 }``                                                   |
+-------------------+-------------------------------------------------------------------------------------------------------------+
| ``-umaskHldrq=``  | |  Description: Configure the HLDRQ Signal Masking.                                                         |
|                   | |  Default Value: 0                                                                                         |
|                   | |  Emulator: E1, E2                                                                                         |
|                   | |  Available Values: 0: No, 1: Yes                                                                          |
|                   | |  Example: ``"serverParameters": { "-umaskHldrq=": 0 }``                                                   |
+-------------------+-------------------------------------------------------------------------------------------------------------+
| ``-umaskStop=``   | |  Description: Configure STOP Signal Masking.                                                              |
|                   | |  Default Value: 0                                                                                         |
|                   | |  Emulator: E1, E2                                                                                         |
|                   | |  Available Values: 0: No, 1: Yes                                                                          |
|                   | |  Example: ``"serverParameters": { "-umaskStop=": 0 }``                                                    |
+-------------------+-------------------------------------------------------------------------------------------------------------+
| ``-umaskWait=``   | |  Description: Configure WAIT Signal Masking.                                                              |
|                   | |  Default Value: 0                                                                                         |
|                   | |  Emulator: E1, E2                                                                                         |
|                   | |  Available Values: 0: No, 1: Yes                                                                          |
|                   | |  Example: ``"serverParameters": { "-umaskWait=": 0 }``                                                    |
+-------------------+-------------------------------------------------------------------------------------------------------------+

Memory
------

+------------------------------+---------------------------------------------------------------------------------------------+
| Option                       | Details                                                                                     |
+==============================+=============================================================================================+
| ``-uMapMode=``               | |  Description: Configure Memory Map mode.                                                  |
|                              | |  Default Value: 0                                                                         |
|                              | |  Emulator: E2                                                                             |
|                              | |  Available Values: 0: Single map mode, 1: Double map mode                                 |
|                              | |  Example: ``"serverParameters": { "-uMapMode=": 0 }``                                     |
+------------------------------+---------------------------------------------------------------------------------------------+
| ``-uverifyOnWritingMemory=`` | |  Description: Configure if verify on write to memory is enabled.                          |
|                              | |  Default Value: 0                                                                         |
|                              | |  Emulator: E1, E2                                                                         |
|                              | |  Available Values: 0: No, 1: Yes                                                          |
|                              | |  Example: ``"serverParameters": { "-uverifyOnWritingMemory=": 0 }``                       |
+------------------------------+---------------------------------------------------------------------------------------------+
| ``-uWorkRamAddress=``        | |  Description: Specify the first address of the working RAM area for use by the debugger.  |
|                              | |  Default Value: Dependent on target device.                                               |
|                              | |  Emulator: E1, E2                                                                         |
|                              | |  Available Values: Any valid hex address.                                                 |
|                              | |  Example: ``"serverParameters": { "-uWorkRamAddress=": "0x20000000" }``                   |
+------------------------------+---------------------------------------------------------------------------------------------+
| ``-uDebugCodeSRAM=``         | |  Description: Specifies whether to debug with Code SRAM.                                  |
|                              | |  Default Value: 0                                                                         |
|                              | |  Emulator: E1, E2                                                                         |
|                              | |  Available Values: 0: No, 1: Yes                                                          |
|                              | |  Example: ``"serverParameters": { "-uDebugCodeSRAM=": 0 }``                               |
+------------------------------+---------------------------------------------------------------------------------------------+
| ``-uInitialiseRAM=``         | |  Description: Select whether to initialize RAM when connecting.                           |
|                              | |  Default Value: 1                                                                         |
|                              | |  Emulator: E1, E2                                                                         |
|                              | |  Available Values: 0: No, 1: Yes                                                          |
|                              | |  Example: ``"serverParameters": { "-uInitialiseRAM=": 1 }``                               |
+------------------------------+---------------------------------------------------------------------------------------------+

Performance Timer
-----------------

+--------------------+-----------------------------------------------------------------------------------------------------+
| Option             | Details                                                                                             |
+====================+=====================================================================================================+
| ``-uPTimerClock=`` | |  Description: Specify the operating frequency used to calculate the performance counter.          |
|                    | |  Default Value: None                                                                              |
|                    | |  Emulator: E1, E2                                                                                 |
|                    | |  Available Values: Between 0.001 to 999.999 (in MHz). Value is converted to Hz internally.        |
|                    | |  Example: ``"serverParameters": { "-uPTimerClock=": 16.00 }``                                     |
|                    | |  Note: It is possible to specify an empty value.                                                  |
+--------------------+-----------------------------------------------------------------------------------------------------+

Reset With Running
------------------

+--------------------+------------------------------------------------------------------------------+
| Option             | Details                                                                      |
+====================+==============================================================================+
| ``-uForceReset=``  | |  Description: Use forced reset for the reset process during execution      |
|                    | |  Default Value: 0                                                          |
|                    | |  Emulator: E1, E2                                                          |
|                    | |  Available Values: 0: No, 1: Yes                                           |
|                    | |  Example: ``"serverParameters": { "-uForceReset=": 0 }``                   |
+--------------------+------------------------------------------------------------------------------+

Multiple Core Settings
======================

+--------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Option             | Details                                                                                                                                                                                |
+====================+========================================================================================================================================================================================+
| ``-uCore=``        | |  Description: Specify the state of the core. The displayed core name depends on the target device.                                                                                   |
|                    | |  Default Value: Enabled                                                                                                                                                              |
|                    | |  Emulator: E1, E2                                                                                                                                                                    |
|                    | |  Available Values: Enabled, Disabled, Free running.                                                                                                                                  |
|                    | |  Example: ``"serverParameters": { "-uCore=": "PE1|enable|256|main" }``                                                                                                               |
|                    | |  Note: The value sent to GDB server follows the format ``<core name>|<state>|<value>|<type>``                                                                                        |
|                    | |  (e.g. ``PE1|enable|256|main`` when PE1 is set to Enabled).                                                                                                                          |
|                    | |  When multiple cores are enabled, each core is sent as a separate ``-uCore=`` parameter,                                                                                             |
|                    | |  for example:                                                                                                                                                                        |
|                    | |    ``"-uCore=": "PE1|enable|256|main"``                                                                                                                                                   |
|                    | |    ``"-uCore=": "PE2|enable|257|main"``                                                                                                                                                   |
|                    | |  This parameter is not sent to GDB server when Disabled or Free running is selected. Availability dependent on the target and debugger support, and more than one core being enabled.|
+--------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-uSyncMode=``    | |  Description: Specify the synchronization mode of the cores.                                                                                                                         |
|                    | |  Default Value: async                                                                                                                                                                |
|                    | |  Emulator: E1, E2                                                                                                                                                                    |
|                    | |  Available Values: async, sync                                                                                                                                                       |
|                    | |  Example: ``"serverParameters": { "-uSyncMode=": "async" }``                                                                                                                         |
|                    | |  Note: Availability dependent on the target and debugger support, and more than one core being enabled.                                                                              |
+--------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-uTraceCore=``   | |  Description: Specify the trace data capture target. When running with a single core, only the trace information of the specified core is displayed in the Trace view.               |
|                    | |  Default Value: PE1 (Dependent on the target device)                                                                                                                                 |
|                    | |  Emulator: E1, E2                                                                                                                                                                    |
|                    | |  Available Values: Any valid enabled core name (e.g. PE1, PE2).                                                                                                                      |
|                    | |  Example: ``"serverParameters": { "-uTraceCore=": "PE1" }``                                                                                                                          |
+--------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Trace
=====

+---------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Option                    | Details                                                                                                                                                        |
+===========================+================================================================================================================================================================+
| ``-uTraceEnable=``        | |  Description: Select the core for which trace data is collected.                                                                                             |
|                           | |  The displayed core name depends on the target device.                                                                                                       |
|                           | |  Default Value: Enabled                                                                                                                                      |
|                           | |  Emulator: E1, E2                                                                                                                                            |
|                           | |  Available Values: Enabled, Disabled.                                                                                                                        |
|                           | |  Example: ``"serverParameters": { "-uTraceEnable=": "PE1|PE2" }``                                                                                            |
|                           | |  Note: The parameter value contains the list of enabled cores separated by ``|``. Only cores set to Enabled are included in the parameter value.             |
|                           | |  For example, if PE1 and PE2 are both Enabled, the value is ``"PE1|PE2"``. If only PE1 is Enabled, the value is ``"PE1"``.                                   |
+---------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-uTraceClock=``         | |  Description: Set the CPU clock frequency in MHz to calculate trace timestamps.                                                                              |
|                           | |  The displayed core name depends on the target device.                                                                                                       |
|                           | |  Default Value: Depends on target device.                                                                                                                    |
|                           | |  Emulator: E1, E2                                                                                                                                            |
|                           | |  Available Values: 1.0 to 9999.999 (in MHz).                                                                                                                 |
|                           | |  Example: ``"serverParameters": { "-uTraceClock=": "PE1|400.0|PE2|400.0" }``                                                                                 |
|                           | |  Note: The parameter value contains pairs of core name and clock frequency separated by ``|``. Only cores set to Enabled are included in the parameter value.|
|                           | |  For example, if PE1 is 400.0 MHz and PE2 is 200.0 MHz, the value is ``"PE1|400.0|PE2|200.0"``. If only PE1 is Enabled, the value is ``"PE1|400.0"``.        |
+---------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-uUseSyncTrace=``       | |  Description: Synchronize trace data across cores.                                                                                                           |
|                           | |  Default Value: 0                                                                                                                                            |
|                           | |  Emulator: E1, E2                                                                                                                                            |
|                           | |  Available Values:                                                                                                                                           |
|                           | |    ``-uUseSyncTrace= 1``: Yes — synchronize trace data across cores.                                                                                         |
|                           | |    ``-uUseSyncTrace= 0``: No — do not synchronize trace data.                                                                                                |
|                           | |  Example: ``"serverParameters": { "-uUseSyncTrace=": 0 }``                                                                                                   |
+---------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-uSyncTraceStandard=``  | |  Description: Select the synchronization trace target standard for cross-core trace data.                                                                    |
|                           | |  Default Value: 0 - E2 Expansion (master)                                                                                                                    |
|                           | |  Emulator: E1, E2                                                                                                                                            |
|                           | |  Available Values:                                                                                                                                           |
|                           | |    ``-uSyncTraceStandard= 0``: E2 Expansion (master).                                                                                                        |
|                           | |    ``-uSyncTraceStandard= 1``: E2 Expansion (slave).                                                                                                         |
|                           | |    ``-uSyncTraceStandard= 2``: STM.                                                                                                                          |
|                           | |  Example: ``"serverParameters": { "-uSyncTraceStandard=": 0 }``                                                                                              |
+---------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-uUseEramTrace=``       | |  Description: Configure whether to use ERAM trace.                                                                                                           |
|                           | |  Default Value: 0                                                                                                                                            |
|                           | |  Emulator: E1, E2                                                                                                                                            |
|                           | |  Available Values: 0: No, 1: Yes                                                                                                                             |
|                           | |  Example: ``"serverParameters": { "-uUseEramTrace=": 0 }``                                                                                                   |
|                           | |  Note: This option is supported only for RH850 U2B (without E) device                                                                                        |
+---------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-uEramAddress=``        | |  Description: Set the start address of the ERAM area used for ERAM trace.                                                                                    |
|                           | |  Default Value: Depends on target device.                                                                                                                    |
|                           | |  Emulator: E1, E2                                                                                                                                            |
|                           | |  Available Values: ERAM areas enabled on the target device.                                                                                                  |
|                           | |  Example: ``"serverParameters": { "-uEramAddress=": "0x20000000" }``                                                                                         |
|                           | |  Note: This option is supported only for RH850 U2B (without E) device                                                                                        |
+---------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------+
