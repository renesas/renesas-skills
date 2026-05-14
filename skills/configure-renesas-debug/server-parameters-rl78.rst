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

+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Option               | Details                                                                                                                                                                     |
+======================+=============================================================================================================================================================================+
| ``-umFreq=``         | |  Description: Specify the Main Clock Frequency.                                                                                                                           |
|                      | |  Default Value: 0: Using Internal Clock                                                                                                                                   |
|                      | |  Emulator: E1, E2, E2LITE, EZ, COMPORT, SIMULATOR, IECUBE                                                                                                                 |
|                      | |  Available Values: 0: Using Internal Clock, 2.00, 3.00, 3.57, 4.00, 4.19, 4.91, 5.00, 6.00, 8.00, 8.38, 10.00, 12.00, 16.00, 20.00, or between 0.001 and 99.999 (in MHz). |
|                      | |  Example: ``"serverParameters": { "-umFreq=": 16.00 }``                                                                                                                   |
+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-usFreq=``         | |  Description: Specify the Sub Clock Frequency. When using an external sub clock oscillation,                                                                              |
|                      | |  specify the frequency adjusted via LIOTRM.                                                                                                                               |
|                      | |  Default Value: 0 (Using Internal Clock)                                                                                                                                  |
|                      | |  Emulator: E1, E2, E2LITE, EZ, COMPORT, SIMULATOR, IECUBE                                                                                                                 |
|                      | |  Available Values: 0 (Using Internal Clock), 32.768, 38.40, or between 0.0001 and 99.9999 (in kHz).                                                                       |
|                      | |  Example: ``"serverParameters": { "-usFreq=": 32.768 }``                                                                                                                  |
|                      | |  Note: Using Internal Clock will send value zero for COMPORT.                                                                                                             |
+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-umClock=``        | |  Description: Select a clock for monitor programs to operate while the program is stopped.                                                                                |
|                      | |  Default Value: 1 (System)                                                                                                                                                |
|                      | |  Emulator: E1, E2, E2LITE, EZ, COMPORT                                                                                                                                    |
|                      | |  Available Values: 0: User, 1: System                                                                                                                                     |
|                      | |  Example: ``"serverParameters": { "-umClock=": 1 }``                                                                                                                      |
+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-umainClockMode=`` | |  Description: Select the main clock source for the target board.                                                                                                          |
|                      | |  Default Value: 0 (Generated by emulator)                                                                                                                                 |
|                      | |  Emulator: IECUBE                                                                                                                                                         |
|                      | |  Available Values: 0: Generated by emulator, 1: External, 2: Clock Socket                                                                                                 |
|                      | |  Example: ``"serverParameters": { "-umainClockMode=": 0 }``                                                                                                               |
+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-usubClockMode=``  | |  Description: Select the sub clock source for the target board.                                                                                                           |
|                      | |  Default Value: 0 (Generated by emulator)                                                                                                                                 |
|                      | |  Emulator: IECUBE                                                                                                                                                         |
|                      | |  Available Values: 0: Generated by emulator, 1: External, 2: Clock Socket                                                                                                 |
|                      | |  Example: ``"serverParameters": { "-usubClockMode=": 0 }``                                                                                                                |
+----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Connection with Target Board
----------------------------

+--------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------+
| Option                         | Details                                                                                                                                           |
+================================+===================================================================================================================================================+
| ``-sn``                        | |  Description: Select the emulator to connect.                                                                                                   |
|                                | |  Default Value: (Auto)                                                                                                                          |
|                                | |  Emulator: E1, E2, E2LITE                                                                                                                       |
|                                | |  Available Values: (Auto) or the serial number of the emulator.                                                                                 |
|                                | |  Example: ``"serverParameters": { "-sn": "e2:_9es002310c" }``                                                                                   |
+--------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-ulowVoltageOcdBoard=``      | |  Description: Select whether to use Low voltage OCD board or not.                                                                               |
|                                | |  Default Value: 0                                                                                                                               |
|                                | |  Emulator: E1, E2, E2LITE                                                                                                                       |
|                                | |  Available Values: 0: No, 1: Yes                                                                                                                |
|                                | |  Example: ``"serverParameters": { "-ulowVoltageOcdBoard=": 0 }``                                                                                |
+--------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-w``                         | |  Description: Select whether to supply power to the target board from emulator (MAX 200mA) and the power source.                                |
|                                | |  Default Value: 1                                                                                                                               |
|                                | |  Emulator: E1, E2, E2LITE                                                                                                                       |
|                                | |  Available Values: 0, 1, 2                                                                                                                      |
|                                | |  Example: ``"serverParameters": { "-w": 1 }``                                                                                                   |
|                                | |  Note: ``-w 2``- supply power via E2 expansion interface. (E2 only)                                                                             |
+--------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-usupplyVoltage=``           | |  Description: Select the power voltage supplied to the target board.                                                                            |
|                                | |  Default Value: 0 (3.3 V)                                                                                                                       |
|                                | |  Emulator: E1, E2, E2LITE                                                                                                                       |
|                                | |  Available Values:                                                                                                                              |
|                                | |    ``-usupplyVoltage= 0``: 3.3 V                                                                                                                |
|                                | |    ``-usupplyVoltage= 1``: 5.0 V                                                                                                                |
|                                | |    ``-usupplyVoltage= 18``: 1.8 V                                                                                                               |
|                                | |    ``-usupplyVoltage= 50``: 5.0 V                                                                                                               |
|                                | |  Example: ``"serverParameters": { "-usupplyVoltage=": 0 }``                                                                                     |
|                                | |  Note: Values ``0`` and ``1`` are reserved for 3.3 V and 5.0 V respectively. E2LITE only supports 3.3 V.                                        |
+--------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-uHotPlug=``                 | |  Description: Select whether to connect as hot plug-in.                                                                                         |
|                                | |  Default Value: 0                                                                                                                               |
|                                | |  Emulator: E1, E2, E2LITE                                                                                                                       |
|                                | |  Available Values:  0: No, 1: Yes                                                                                                               |
|                                | |  Example: ``"serverParameters": { "-uHotPlug=": 0 }``                                                                                           |
+--------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-uComPort=``                 | |  Description: Select the COMPORT to connect.                                                                                                    |
|                                | |  Default Value: None                                                                                                                            |
|                                | |  Emulator: COMPORT                                                                                                                              |
|                                | |  Available Values: Any valid COMPORT number.                                                                                                    |
|                                | |  Example: ``"serverParameters": { "-uComPort=": "COM3" }``                                                                                      |
|                                | |  Note: The message "A COMPORT must be selected" is displayed when COMPORT has not been specified.                                               |
+--------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-uResetControl=``            | |  Description: Select the reset control pin.                                                                                                     |
|                                | |  Default Value: 0 (DTR)                                                                                                                         |
|                                | |  Emulator: COMPORT                                                                                                                              |
|                                | |  Available Values:                                                                                                                              |
|                                | |    ``-uResetControl= 0``: DTR.                                                                                                                  |
|                                | |    ``-uResetControl= 1``: RTS.                                                                                                                  |
|                                | |  Example: ``"serverParameters": { "-uResetControl=": 0 }``                                                                                      |
+--------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-uComBaudRate=``             | |  Description: Select data transmission rate.                                                                                                    |
|                                | |  Default Value: 0 (Auto)                                                                                                                        |
|                                | |  Emulator: COMPORT                                                                                                                              |
|                                | |  Available Values:                                                                                                                              |
|                                | |    ``-uComBaudRate= 0``: Auto.                                                                                                                  |
|                                | |    ``-uComBaudRate= 115200``: 115.2K bps.                                                                                                       |
|                                | |    ``-uComBaudRate= 250000``: 250K bps.                                                                                                         |
|                                | |    ``-uComBaudRate= 500000``: 500K bps.                                                                                                         |
|                                | |    ``-uComBaudRate= 1000000``: 1M bps.                                                                                                          |
|                                | |  Example: ``"serverParameters": { "-uComBaudRate=": 0 }``                                                                                       |
+--------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-utargetBoardConnected=``    | |  Description: Specify whether to connect with the target board.                                                                                 |
|                                | |  Default Value: 0                                                                                                                               |
|                                | |  Emulator: IECUBE                                                                                                                               |
|                                | |  Available Values:  0: No, 1: Yes                                                                                                               |
|                                | |  Example: ``"serverParameters": { "-utargetBoardConnected=": 0 }``                                                                              |
+--------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------+

Flash
-----

+---------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Option                                | Details                                                                                                                                                                        |
+=======================================+================================================================================================================================================================================+
| ``-usecurityID=``                     | |  Description: Specify a security ID for reading codes in the internal ROM.                                                                                                   |
|                                       | |  Default Value:                                                                                                                                                              |
|                                       | |    Device with 10 bytes ID code: 00000000000000000000.                                                                                                                       |
|                                       | |    Device with 16 bytes ID code: 00000000000000000000000000000000.                                                                                                           |
|                                       | |  Emulator: E1, E2, E2LITE, EZ, COMPORT                                                                                                                                       |
|                                       | |  Available Values:                                                                                                                                                           |
|                                       | |    Device with 10 bytes ID code: 0x0 to 0xFFFFFFFFFFFFFFFFFFFE.                                                                                                              |
|                                       | |    Device with 16 bytes ID code: 0x0 to 0xFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFE.                                                                                                  |
|                                       | |  Example: ``"serverParameters": { "-usecurityID=": "00000000000000000000" }``                                                                                                |
|                                       | |  Note: Appears only when the selected microcontroller supports the ROM security function. 10 bytes ID code by default; 16 bytes if the selected microcontroller supports it. |
|                                       | |  (refer to user's manual of On-Chip Debug Security ID for more detail).                                                                                                      |
+---------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-uprogrammerSecurityID=``           | |  Description: Specify a serial programming security ID for programming flash memory.                                                                                         |
|                                       | |  Default Value: FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF                                                                                                                             |
|                                       | |  Emulator: E1, E2, E2LITE, EZ, COMPORT                                                                                                                                       |
|                                       | |  Available Values: 0x0 to 0xFFFFFFFFFFFFFFFFFFFFFFFFFFFFF (hexadecimal).                                                                                                     |
|                                       | |  Example: ``"serverParameters": { "-uprogrammerSecurityID=": "FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF" }``                                                                          |
+---------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-uallowWritingProgramerSecurityID=``| |  Description: Select whether the serial programming security ID is written when downloading or not.                                                                          |
|                                       | |  Default Value: 0                                                                                                                                                            |
|                                       | |  Emulator: E1, E2, E2LITE, EZ, COMPORT                                                                                                                                       |
|                                       | |  Available Values: 0: No, 1: Yes                                                                                                                                             |
|                                       | |  Example: ``"serverParameters": { "-uallowWritingProgramerSecurityID=": 1 }``                                                                                                |
+---------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-upermitFlash=``                    | |  Description: Select whether to enable flash rewrite. When No is specified, downloading program image or                                                                     |
|                                       | |  setting software breakpoint is disabled.                                                                                                                                    |
|                                       | |  Default Value: 1                                                                                                                                                            |
|                                       | |  Emulator: E1, E2, E2LITE, EZ, COMPORT                                                                                                                                       |
|                                       | |  Available Values: 0: No, 1: Yes                                                                                                                                             |
|                                       | |  Example: ``"serverParameters": { "-upermitFlash=": 1 }``                                                                                                                    |
+---------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-uuseWideVoltageMode=``             | |  Description: Select whether to rewrite the flash memory in wide voltage mode.                                                                                               |
|                                       | |  Default Value: 1                                                                                                                                                            |
|                                       | |  Emulator: E1, E2, E2LITE, EZ, COMPORT                                                                                                                                       |
|                                       | |  Available Values: 0: No, 1: Yes                                                                                                                                             |
|                                       | |  Example: ``"serverParameters": { "-uuseWideVoltageMode=": 1 }``                                                                                                             |
+---------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-ueraseRom=``                       | |  Description: Select whether to erase the flash ROM when connecting to the debug tool.                                                                                       |
|                                       | |  Please specify Yes for Permit Flash Programming.                                                                                                                            |
|                                       | |  Default Value: 0                                                                                                                                                            |
|                                       | |  Emulator: E1, E2, E2LITE, EZ, COMPORT                                                                                                                                       |
|                                       | |  Available Values: 0: No, 1: Yes                                                                                                                                             |
|                                       | |  Example: ``"serverParameters": { "-ueraseRom=": 0 }``                                                                                                                       |
+---------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-uSelfCodeSet=``                    | |  Description: Specify whether to program the flash memory by the flash self library.                                                                                         |
|                                       | |  This option is enabled only for the device which requires this setting.                                                                                                     |
|                                       | |  Default Value: 0                                                                                                                                                            |
|                                       | |  Emulator: E1, E2, E2LITE, EZ, COMPORT                                                                                                                                       |
|                                       | |  Available Values: 0: No, 1: Yes                                                                                                                                             |
|                                       | |  Example: ``"serverParameters": { "-uSelfCodeSet=": 0 }``                                                                                                                    |
+---------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-uFillUnusedFlashOnRewriting=``     | |  Description: Select whether to allow fill unused area with 0xFF when writing flash memory.                                                                                  |
|                                       | |  Default Value: 0                                                                                                                                                            |
|                                       | |  Emulator: E1, E2, E2LITE, EZ, COMPORT                                                                                                                                       |
|                                       | |  Available Values: 0: No, 1: Yes                                                                                                                                             |
|                                       | |  Example: ``"serverParameters": { "-uFillUnusedFlashOnRewriting=": 0 }``                                                                                                     |
+---------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-ubankSwapEnable=``                 | |  Description: Select whether to modify BTBLS for bank swapping when the program uses bank swapping feature.                                                                  |
|                                       | |  Default Value: 0                                                                                                                                                            |
|                                       | |  Emulator: E2, E2LITE, COMPORT, SIMULATOR                                                                                                                                    |
|                                       | |  Available Values: 0: No, 1: Yes                                                                                                                                             |
|                                       | |  Example: ``"serverParameters": { "-ubankSwapEnable=": 0 }``                                                                                                                 |
+---------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-ubootClusterSize=``                | |  Description: Specify the boot cluster size (in KB) for BTBLS on connection.                                                                                                 |
|                                       | |  Default Value: 16                                                                                                                                                           |
|                                       | |  Emulator: SIMULATOR                                                                                                                                                         |
|                                       | |  Available Values: 16, 32, 64, 128 (depend on the target device)                                                                                                             |
|                                       | |  Example: ``"serverParameters": { "-ubootClusterSize=": 16 }``                                                                                                               |
+---------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Debug Tool Settings
===================

General Debug
-------------

+----------------------+-------------------------------------------------------------------------+
| Option               | Details                                                                 |
+======================+=========================================================================+
| ``-uresetOnReload=`` | |  Description: Select whether to reset the program after downloaded.   |
|                      | |  Default Value: 1                                                     |
|                      | |  Emulator: E1, E2, E2LITE, EZ, COMPORT, SIMULATOR, IECUBE             |
|                      | |  Available Values: 0: No, 1: Yes                                      |
|                      | |  Example: ``"serverParameters": { "-uresetOnReload=": 1 }``           |
+----------------------+-------------------------------------------------------------------------+

Break
-----

+--------------------------------+-------------------------------------------------------------------------------------------------------------+
| Option                         | Details                                                                                                     |
+================================+=============================================================================================================+
| ``-ustopTimerEmu=``            | |  Description: Select whether to terminate the peripheral emulation of timers while stopping.              |
|                                | |  Default Value: 0                                                                                         |
|                                | |  Emulator: E1, E2, E2LITE, EZ, COMPORT, IECUBE                                                            |
|                                | |  Available Values: 0: No, 1: Yes                                                                          |
|                                | |  Example: ``"serverParameters": { "-ustopTimerEmu=": 0 }``                                                |
+--------------------------------+-------------------------------------------------------------------------------------------------------------+
| ``-ustopSerialEmu=``           | |  Description: Select whether to terminate the peripheral emulation of serials while stopping.             |
|                                | |  Default Value: 0                                                                                         |
|                                | |  Emulator: E1, E2, E2LITE, EZ, COMPORT, IECUBE                                                            |
|                                | |  Available Values: 0: No, 1: Yes                                                                          |
|                                | |  Example: ``"serverParameters": { "-ustopSerialEmu=": 0 }``                                               |
+--------------------------------+-------------------------------------------------------------------------------------------------------------+

Mask For Input Signal
---------------------

+--------------------------------+-------------------------------------------------------------------------------------------------------------+
| Option                         | Details                                                                                                     |
+================================+=============================================================================================================+
| ``-umaskTargetResetSignal=``   | |  Description: Configure Target Reset Signal Masking.                                                      |
|                                | |  Default Value: 0                                                                                         |
|                                | |  Emulator: E1, E2, E2LITE, EZ                                                                             |
|                                | |  Available Values: 0: No, 1: Yes                                                                          |
|                                | |  Example: ``"serverParameters": { "-umaskTargetResetSignal=": 0 }``                                       |
+--------------------------------+-------------------------------------------------------------------------------------------------------------+
| ``-umaskInternalResetSignal=`` | |  Description: Configure Internal Reset Signal Masking.                                                    |
|                                | |  Default Value: 0                                                                                         |
|                                | |  Emulator: E1, E2, E2LITE, EZ, COMPORT, IECUBE                                                            |
|                                | |  Available Values: 0: No, 1: Yes                                                                          |
|                                | |  Example: ``"serverParameters": { "-umaskInternalResetSignal=": 0 }``                                     |
+--------------------------------+-------------------------------------------------------------------------------------------------------------+
| ``-umaskNMISignal=``           | |  Description: Configure NMI Signal Masking.                                                               |
|                                | |  Default Value: 0                                                                                         |
|                                | |  Emulator: IECUBE                                                                                         |
|                                | |  Available Values: 0: No, 1: Yes                                                                          |
|                                | |  Example: ``"serverParameters": { "-umaskNMISignal=": 0 }``                                               |
+--------------------------------+-------------------------------------------------------------------------------------------------------------+

Memory
------

+------------------------------+-----------------------------------------------------------------------------------------------------------+
| Option                       | Details                                                                                                   |
+==============================+===========================================================================================================+
| ``-uverifyOnWritingMemory=`` | |  Description: Select whether to verify the value when writing the memory.                               |
|                              | |  Default Value: 1                                                                                       |
|                              | |  Emulator: E1, E2, E2LITE, EZ, COMPORT, IECUBE                                                          |
|                              | |  Available Values: 0: No, 1: Yes                                                                        |
|                              | |  Example: ``"serverParameters": { "-uverifyOnWritingMemory=": 1 }``                                     |
+------------------------------+-----------------------------------------------------------------------------------------------------------+
| ``-uAllowRRMDMM=``           | |  Description: Select whether to allow accessing memory by stopping execution while program is running.  |
|                              | |  Default Value: 1 (Yes) for E1, E2; 0 (No) for COM Port.                                                |     
|                              | |  Emulator: E1, E2, E2LITE, EZ, COMPORT                                                                  |
|                              | |  Available Values: 0: No, 1: Yes                                                                        |
|                              | |  Example: ``"serverParameters": { "-uAllowRRMDMM=": 1 }``                                               |
+------------------------------+-----------------------------------------------------------------------------------------------------------+

Simulator GUI
-------------

+-------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Option                  | Details                                                                                                                                                                                               |
+=========================+=======================================================================================================================================================================================================+
| ``-udisplaySimGUI=``    | |  Description: Select whether to display the Simulator GUI window.                                                                                                                                   |
|                         | |  Default Value: 1                                                                                                                                                                                   |
|                         | |  Emulator: SIMULATOR                                                                                                                                                                                |
|                         | |  Available Values: 0: No, 1: Yes                                                                                                                                                                    |
|                         | |  Example: ``"serverParameters": { "-udisplaySimGUI=": 1 }``                                                                                                                                         |
+-------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-uSimulatorSetting=`` | |  Description: Select Simulator GUI configuration file. Allows the state of each window in the Simulator GUI to be restored on reconnection. This file will be created if it does not already exist. |
|                         | |  Default Value: Simulator configuration (simpj) file stored in the project folder as the active configuration name.                                                                                 |
|                         | |  Emulator: SIMULATOR                                                                                                                                                                                |
|                         | |  Available Values: Any valid file path.                                                                                                                                                             |
|                         | |  Example: ``"serverParameters": { "-uSimulatorSetting=": "${workspace_loc:/${ProjName}}/${LaunchConfigName}.simpj" }``                                                                              |
+-------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Fail-safe Break
---------------

+-------------------------------+-----------------------------------------------------------------------------------+
| Option                        | Details                                                                           |
+===============================+===================================================================================+
| ``-ufsbFetchFromProtected=``  | |  Description: Stop on fetch from fetch-protected area.                          |
|                               | |  Default Value: 1                                                               |
|                               | |  Emulator: IECUBE                                                               |
|                               | |  Available Values: 0: No, 1: Yes                                                |
|                               | |  Example: ``"serverParameters": { "-ufsbFetchFromProtected=": 1 }``             |
+-------------------------------+-----------------------------------------------------------------------------------+
| ``-ufsbWriteToProtected=``    | |  Description: Stop on write to write-protected area.                            |
|                               | |  Default Value: 1                                                               |
|                               | |  Emulator: IECUBE                                                               |
|                               | |  Available Values: 0: No, 1: Yes                                                |
|                               | |  Example: ``"serverParameters": { "-ufsbWriteToProtected=": 1 }``               |
+-------------------------------+-----------------------------------------------------------------------------------+
| ``-ufsbReadFromSFR=``         | |  Description: Stop on read from read-protected SFR.                             |
|                               | |  Default Value: 1                                                               |
|                               | |  Emulator: IECUBE                                                               |
|                               | |  Available Values: 0: No, 1: Yes                                                |
|                               | |  Example: ``"serverParameters": { "-ufsbReadFromSFR=": 1 }``                    |
+-------------------------------+-----------------------------------------------------------------------------------+
| ``-ufsbWriteToSFR=``          | |  Description: Stop on write to write-protected SFR.                             |
|                               | |  Default Value: 1                                                               |
|                               | |  Emulator: IECUBE                                                               |
|                               | |  Available Values: 0: No, 1: Yes                                                |
|                               | |  Example: ``"serverParameters": { "-ufsbWriteToSFR=": 1 }``                     |
+-------------------------------+-----------------------------------------------------------------------------------+
| ``-ufsbStackOverflow=``       | |  Description: Stop on overflow of user stack.                                   |
|                               | |  Default Value: 0                                                               |
|                               | |  Emulator: IECUBE                                                               |
|                               | |  Available Values: 0: No, 1: Yes                                                |
|                               | |  Example: ``"serverParameters": { "-ufsbStackOverflow=": 0 }``                  |
+-------------------------------+-----------------------------------------------------------------------------------+
| ``-ufsbStackTop=``            | |  Description: Specify the user stack top address.                               |
|                               | |  Default Value: 0x0                                                             |
|                               | |  Emulator: IECUBE                                                               |
|                               | |  Available Values: Any valid hex address.                                       |
|                               | |  Example: ``"serverParameters": { "-ufsbStackTop=": "0x0" }``                   |
+-------------------------------+-----------------------------------------------------------------------------------+
| ``-ufsbUnderflowStack=``      | |  Description: Stop on underflow of user stack.                                  |
|                               | |  Default Value: 0                                                               |
|                               | |  Emulator: IECUBE                                                               |
|                               | |  Available Values: 0: No, 1: Yes                                                |
|                               | |  Example: ``"serverParameters": { "-ufsbUnderflowStack=": 0 }``                 |
+-------------------------------+-----------------------------------------------------------------------------------+
| ``-ufsbStackBottom=``         | |  Description: Specify the user stack bottom address.                            |
|                               | |  Default Value: 0x0                                                             |
|                               | |  Emulator: IECUBE                                                               |
|                               | |  Available Values: Any valid hex address.                                       |
|                               | |  Example: ``"serverParameters": { "-ufsbStackBottom=": "0x0" }``                |
+-------------------------------+-----------------------------------------------------------------------------------+
| ``-ufsbUninitStack=``         | |  Description: Stop on uninitialized stack pointer.                              |
|                               | |  Default Value: 1                                                               |
|                               | |  Emulator: IECUBE                                                               |
|                               | |  Available Values: 0: No, 1: Yes                                                |
|                               | |  Example: ``"serverParameters": { "-ufsbUninitStack=": 1 }``                    |
+-------------------------------+-----------------------------------------------------------------------------------+
| ``-ufsbUninitRAM=``           | |  Description: Stop on read from uninitialized RAM.                              |
|                               | |  Default Value: 1                                                               |
|                               | |  Emulator: IECUBE                                                               |
|                               | |  Available Values: 0: No, 1: Yes                                                |
|                               | |  Example: ``"serverParameters": { "-ufsbUninitRAM=": 1 }``                      |
+-------------------------------+-----------------------------------------------------------------------------------+
| ``-ufsbNonMapMem=``           | |  Description: Stop on access to memory in non-mapped area.                      |
|                               | |  Default Value: 1                                                               |
|                               | |  Emulator: IECUBE                                                               |
|                               | |  Available Values: 0: No, 1: Yes                                                |
|                               | |  Example: ``"serverParameters": { "-ufsbNonMapMem=": 1 }``                      |
+-------------------------------+-----------------------------------------------------------------------------------+
| ``-ufsbMisalignWord=``        | |  Description: Stop on misaligned word access.                                   |
|                               | |  Default Value: 1                                                               |
|                               | |  Emulator: IECUBE                                                               |
|                               | |  Available Values: 0: No, 1: Yes                                                |
|                               | |  Example: ``"serverParameters": { "-ufsbMisalignWord=": 1 }``                   |
+-------------------------------+-----------------------------------------------------------------------------------+
| ``-ufsbPeripFSB=``            | |  Description: Stop on fail safe from peripheral.                                |
|                               | |  Default Value: 1                                                               |
|                               | |  Emulator: IECUBE                                                               |
|                               | |  Available Values: 0: No, 1: Yes                                                |
|                               | |  Example: ``"serverParameters": { "-ufsbPeripFSB=": 1 }``                       |
+-------------------------------+-----------------------------------------------------------------------------------+
| ``-ufsbIllegalFlash=``        | |  Description: Stop on illegal flash event.                                      |
|                               | |  Default Value: 0                                                               |
|                               | |  Emulator: IECUBE                                                               |
|                               | |  Available Values: 0: No, 1: Yes                                                |
|                               | |  Example: ``"serverParameters": { "-ufsbIllegalFlash=": 0 }``                   |
+-------------------------------+-----------------------------------------------------------------------------------+

RTOS
----

+----------------------+-------------------------------------------------------------------------------------------+
| Option               | Details                                                                                   |
+======================+===========================================================================================+
| ``-uOSRestriction=`` | |  Description: Enable RTOS Debugging for a large number of threads. Non-executing RTOS   |
|                      | |  threads will display information only if manually suspended.                           |
|                      | |  Default Value: 0                                                                       |
|                      | |  Emulator: E1, E2, E2LITE, EZ, COMPORT, SIMULATOR, IECUBE                               |
|                      | |  Available Values: 0: No, 1: Yes                                                        |
|                      | |  Example: ``"serverParameters": { "-uOSRestriction=": 1 }``                             |
+----------------------+-------------------------------------------------------------------------------------------+

Emulation Settings
==================

Flash
-----

+------------------------+---------------------------------------------------------------------------------------------------------------+
| Option                 | Details                                                                                                       |
+========================+===============================================================================================================+
| ``-ufspEnable=``       | |  Description: Specify whether to use flash self-programming.                                                |
|                        | |  Default Value: 0                                                                                           |
|                        | |  Emulator: IECUBE                                                                                           |
|                        | |  Available Values: 0: No, 1: Yes                                                                            |
|                        | |  Example: ``"serverParameters": { "-ufspEnable=": 0 }``                                                     |
+------------------------+---------------------------------------------------------------------------------------------------------------+
| ``-ufspWriteTime=``    | |  Description: Specify the write time for flash memory.                                                      |
|                        | |  Default Value: 1 (Typical retries assumed by flash specification)                                          |
|                        | |  Emulator: IECUBE                                                                                           |
|                        | |  Available Values:                                                                                          |
|                        | |    0: No retries.                                                                                           |
|                        | |    1: Typical retries assumed by flash specification.                                                       |
|                        | |    2: Maximum retries assumed by flash specification.                                                       |
|                        | |    3: Maximum retries.                                                                                      |
|                        | |  Example: ``"serverParameters": { "-ufspWriteTime=": 1 }``                                                  |
+------------------------+---------------------------------------------------------------------------------------------------------------+
| ``-ufspWriteAddress=`` | |  Description: Specify the address for write time.                                                           |
|                        | |  Default Value: 0x0                                                                                         |
|                        | |  Emulator: IECUBE                                                                                           |
|                        | |  Available Values: Any valid hex address.                                                                   |
|                        | |  Example: ``"serverParameters": { "-ufspWriteAddress=": "0x0" }``                                           |
+------------------------+---------------------------------------------------------------------------------------------------------------+
| ``-ufspEraseTime=``    | |  Description: Specify the erase time for flash memory.                                                      |
|                        | |  Default Value: 1 (Typical retries assumed by flash specification)                                          |
|                        | |  Emulator: IECUBE                                                                                           |
|                        | |  Available Values:                                                                                          |
|                        | |    0: No retries.                                                                                           |
|                        | |    1: Typical retries assumed by flash specification.                                                       |
|                        | |    2: Maximum retries assumed by flash specification.                                                       |
|                        | |    3: Maximum retries.                                                                                      |
|                        | |  Example: ``"serverParameters": { "-ufspEraseTime=": 1 }``                                                  |
+------------------------+---------------------------------------------------------------------------------------------------------------+
| ``-ufspEraseAddress=`` | |  Description: Specify the address for erase time.                                                           |
|                        | |  Default Value: 0x0                                                                                         |
|                        | |  Emulator: IECUBE                                                                                           |
|                        | |  Available Values: Any valid hex address.                                                                   |
|                        | |  Example: ``"serverParameters": { "-ufspEraseAddress=": "0x0" }``                                           |
+------------------------+---------------------------------------------------------------------------------------------------------------+
| ``-ufspMSR0=``         | |  Description: Specify the macro service error 0.                                                            |
|                        | |  Default Value: 0 (None)                                                                                    |
|                        | |  Emulator: IECUBE                                                                                           |
|                        | |  Available Values:                                                                                          |
|                        | |    0: None.                                                                                                 |
|                        | |    1: Erase Error.                                                                                          |
|                        | |    2: Blank Check Error.                                                                                    |
|                        | |    3: Write Error.                                                                                          |
|                        | |    4: Verify Error.                                                                                         |
|                        | |    5: Erase Error (Security/Flash Shield).                                                                  |
|                        | |    6: Write Error (Security/Flash Shield).                                                                  |
|                        | |    7: Verify Error (Security/Flash Shield).                                                                 |
|                        | |  Example: ``"serverParameters": { "-ufspMSR0=": 0 }``                                                       |
+------------------------+---------------------------------------------------------------------------------------------------------------+
| ``-ufspMSR0Address=``  | |  Description: Specify the address for macro service error 0.                                                |
|                        | |  Default Value: 0x0                                                                                         |
|                        | |  Emulator: IECUBE                                                                                           |
|                        | |  Available Values: Any valid hex address.                                                                   |
|                        | |  Example: ``"serverParameters": { "-ufspMSR0Address=": "0x0" }``                                            |
|                        | |  Note: This value will not be sent to GDB server if ``-ufspMSR0=`` is set to 0 (None).                      |
+------------------------+---------------------------------------------------------------------------------------------------------------+
| ``-ufspMSR0Mask=``     | |  Description: Specify the address mask for macro service error 0.                                           |
|                        | |  Default Value: 0x0                                                                                         |
|                        | |  Emulator: IECUBE                                                                                           |
|                        | |  Available Values: Any valid hex address.                                                                   |
|                        | |  Example: ``"serverParameters": { "-ufspMSR0Mask=": "0x0" }``                                               |
|                        | |  Note: This value will not be sent to GDB server if ``-ufspMSR0=`` is set to 0 (None).                      |
+------------------------+---------------------------------------------------------------------------------------------------------------+
| ``-ufspMSR1=``         | |  Description: Specify the macro service error 1.                                                            |
|                        | |  Default Value: 0 (None)                                                                                    |
|                        | |  Emulator: IECUBE                                                                                           |
|                        | |  Available Values:                                                                                          |
|                        | |    0: None.                                                                                                 |
|                        | |    1: Erase Error.                                                                                          |
|                        | |    2: Blank Check Error.                                                                                    |
|                        | |    3: Write Error.                                                                                          |
|                        | |    4: Verify Error.                                                                                         |
|                        | |    5: Erase Error (Security/Flash Shield).                                                                  |
|                        | |    6: Write Error (Security/Flash Shield).                                                                  |
|                        | |    7: Verify Error (Security/Flash Shield).                                                                 |
|                        | |  Example: ``"serverParameters": { "-ufspMSR1=": 0 }``                                                       |
+------------------------+---------------------------------------------------------------------------------------------------------------+
| ``-ufspMSR1Address=``  | |  Description: Specify the address for macro service error 1.                                                |
|                        | |  Default Value: 0x0                                                                                         |
|                        | |  Emulator: IECUBE                                                                                           |
|                        | |  Available Values: Any valid hex address.                                                                   |
|                        | |  Example: ``"serverParameters": { "-ufspMSR1Address=": "0x0" }``                                            |
|                        | |  Note: This value will not be sent to GDB server if ``-ufspMSR1=`` is set to 0 (None).                      |
+------------------------+---------------------------------------------------------------------------------------------------------------+
| ``-ufspMSR1Mask=``     | |  Description: Specify the address mask for macro service error 1.                                           |
|                        | |  Default Value: 0x0                                                                                         |
|                        | |  Emulator: IECUBE                                                                                           |
|                        | |  Available Values: Any valid hex address.                                                                   |
|                        | |  Example: ``"serverParameters": { "-ufspMSR1Mask=": "0x0" }``                                               |
|                        | |  Note: This value will not be sent to GDB server if ``-ufspMSR1=`` is set to 0 (None).                      |
+------------------------+---------------------------------------------------------------------------------------------------------------+
| ``-ufspMSR2=``         | |  Description: Specify the macro service error 2.                                                            |
|                        | |  Default Value: 0 (None)                                                                                    |
|                        | |  Emulator: IECUBE                                                                                           |
|                        | |  Available Values:                                                                                          |
|                        | |    0: None.                                                                                                 |
|                        | |    1: Erase Error.                                                                                          |
|                        | |    2: Blank Check Error.                                                                                    |
|                        | |    3: Write Error.                                                                                          |
|                        | |    4: Verify Error.                                                                                         |
|                        | |    5: Erase Error (Security/Flash Shield).                                                                  |
|                        | |    6: Write Error (Security/Flash Shield).                                                                  |
|                        | |    7: Verify Error (Security/Flash Shield).                                                                 |
|                        | |  Example: ``"serverParameters": { "-ufspMSR2=": 0 }``                                                       |
+------------------------+---------------------------------------------------------------------------------------------------------------+
| ``-ufspMSR2Address=``  | |  Description: Specify the address for macro service error 2.                                                |
|                        | |  Default Value: 0x0                                                                                         |
|                        | |  Emulator: IECUBE                                                                                           |
|                        | |  Available Values: Any valid hex address.                                                                   |
|                        | |  Example: ``"serverParameters": { "-ufspMSR2Address=": "0x0" }``                                            |
|                        | |  Note: This value will not be sent to GDB server if ``-ufspMSR2=`` is set to 0 (None).                      |
+------------------------+---------------------------------------------------------------------------------------------------------------+
| ``-ufspMSR2Mask=``     | |  Description: Specify the address mask for macro service error 2.                                           |
|                        | |  Default Value: 0x0                                                                                         |
|                        | |  Emulator: IECUBE                                                                                           |
|                        | |  Available Values: Any valid hex address.                                                                   |
|                        | |  Example: ``"serverParameters": { "-ufspMSR2Mask=": "0x0" }``                                               |
|                        | |  Note: This value will not be sent to GDB server if ``-ufspMSR2=`` is set to 0 (None).                      |
+------------------------+---------------------------------------------------------------------------------------------------------------+

Flash Shield Window
-------------------

+-----------------------------+-----------------------------------------------------------------------------------+
| Option                      | Details                                                                           |
+=============================+===================================================================================+
| ``-ufspShieldStart=``       | |  Description: Specify the flash shield start block.                             |
|                             | |  Default Value: 0x0                                                             |
|                             | |  Emulator: IECUBE                                                               |
|                             | |  Available Values: Any valid hex block address.                                 |
|                             | |  Example: ``"serverParameters": { "-ufspShieldStart=": "0x0" }``                |
+-----------------------------+-----------------------------------------------------------------------------------+
| ``-ufspShieldEnd=``         | |  Description: Specify the flash shield end block.                               |
|                             | |  Default Value: 0xFFFF                                                          |
|                             | |  Emulator: IECUBE                                                               |
|                             | |  Available Values: Any valid hex block address.                                 |
|                             | |  Example: ``"serverParameters": { "-ufspShieldEnd=": "0xFFFF" }``               |
+-----------------------------+-----------------------------------------------------------------------------------+

Security Flag
-------------

+-----------------------------+-----------------------------------------------------------------------------------+
| Option                      | Details                                                                           |
+=============================+===================================================================================+
| ``-ufspDisableErase=``      | |  Description: Specify whether to disable block erase.                           |
|                             | |  Default Value: 0                                                               |
|                             | |  Emulator: IECUBE                                                               |
|                             | |  Available Values: 0: No, 1: Yes                                                |
|                             | |  Example: ``"serverParameters": { "-ufspDisableErase=": 0 }``                   |
+-----------------------------+-----------------------------------------------------------------------------------+
| ``-ufspDisableProgram=``    | |  Description: Specify whether to disable program.                               |
|                             | |  Default Value: 0                                                               |
|                             | |  Emulator: IECUBE                                                               |
|                             | |  Available Values: 0: No, 1: Yes                                                |
|                             | |  Example: ``"serverParameters": { "-ufspDisableProgram=": 0 }``                 |
+-----------------------------+-----------------------------------------------------------------------------------+
| ``-ufspDisableBootWrite=``  | |  Description: Specify whether to disable boot area rewrite.                     |
|                             | |  Default Value: 0                                                               |
|                             | |  Emulator: IECUBE                                                               |
|                             | |  Available Values: 0: No, 1: Yes                                                |
|                             | |  Example: ``"serverParameters": { "-ufspDisableBootWrite=": 0 }``               |
+-----------------------------+-----------------------------------------------------------------------------------+

Data Flash
----------

+------------------------+---------------------------------------------------------------------------------------------------------------+
| Option                 | Details                                                                                                       |
+========================+===============================================================================================================+
| ``-udfspEnable=``      | |  Description: Specify whether to use DataFlash self-programming.                                            |
|                        | |  Default Value: 0                                                                                           |
|                        | |  Emulator: IECUBE                                                                                           |
|                        | |  Available Values: 0: No, 1: Yes                                                                            |
|                        | |  Example: ``"serverParameters": { "-udfspEnable=": 0 }``                                                    |
+------------------------+---------------------------------------------------------------------------------------------------------------+
| ``-udfspWriteTime=``   | |  Description: Specify the write time for DataFlash memory.                                                  |
|                        | |  Default Value: 1 (Typical retries assumed by flash specification)                                          |
|                        | |  Emulator: IECUBE                                                                                           |
|                        | |  Available Values:                                                                                          |
|                        | |    0: No retries.                                                                                           |
|                        | |    1: Typical retries assumed by flash specification.                                                       |
|                        | |    2: Maximum retries assumed by flash specification.                                                       |
|                        | |    3: Maximum retries.                                                                                      |
|                        | |  Example: ``"serverParameters": { "-udfspWriteTime=": 1 }``                                                 |
+------------------------+---------------------------------------------------------------------------------------------------------------+
| ``-udfspWriteAddress=``| |  Description: Specify the address for DataFlash write time.                                                 |
|                        | |  Default Value: 0xF1000                                                                                     |
|                        | |  Emulator: IECUBE                                                                                           |
|                        | |  Available Values: Any valid hex address.                                                                   |
|                        | |  Example: ``"serverParameters": { "-udfspWriteAddress=": "0xF1000" }``                                      |
+------------------------+---------------------------------------------------------------------------------------------------------------+
| ``-udfspMSR0=``        | |  Description: Specify the macro service error 0 for DataFlash.                                              |
|                        | |  Default Value: 0 (None)                                                                                    |
|                        | |  Emulator: IECUBE                                                                                           |
|                        | |  Available Values:                                                                                          |
|                        | |    0: None.                                                                                                 |
|                        | |    1: Blank Check Error.                                                                                    |
|                        | |    2: Write Error.                                                                                          |
|                        | |    3: Verify Error.                                                                                         |
|                        | |  Example: ``"serverParameters": { "-udfspMSR0=": 0 }``                                                      |
+------------------------+---------------------------------------------------------------------------------------------------------------+
| ``-udfspMSR0Address=`` | |  Description: Specify the address for DataFlash macro service error 0.                                      |
|                        | |  Default Value: 0xF1000                                                                                     |
|                        | |  Emulator: IECUBE                                                                                           |
|                        | |  Available Values: Any valid hex address.                                                                   |
|                        | |  Example: ``"serverParameters": { "-udfspMSR0Address=": "0xF1000" }``                                       |
|                        | |  Note: This value will not be sent to GDB server if ``-udfspMSR0=`` is set to 0 (None).                     |
+------------------------+---------------------------------------------------------------------------------------------------------------+
| ``-udfspMSR0Mask=``    | |  Description: Specify the address mask for DataFlash macro service error 0.                                 |
|                        | |  Default Value: 0xFFFF                                                                                      |
|                        | |  Emulator: IECUBE                                                                                           |
|                        | |  Available Values: Any valid hex address.                                                                   |
|                        | |  Example: ``"serverParameters": { "-udfspMSR0Mask=": "0xFFFF" }``                                           |
|                        | |  Note: This value will not be sent to GDB server if ``-udfspMSR0=`` is set to 0 (None).                     |
+------------------------+---------------------------------------------------------------------------------------------------------------+
| ``-udfspMSR1=``        | |  Description: Specify the macro service error 1 for DataFlash.                                              |
|                        | |  Default Value: 0 (None)                                                                                    |
|                        | |  Emulator: IECUBE                                                                                           |
|                        | |  Available Values:                                                                                          |
|                        | |    0: None.                                                                                                 |
|                        | |    1: Blank Check Error.                                                                                    |
|                        | |    2: Write Error.                                                                                          |
|                        | |    3: Verify Error.                                                                                         |
|                        | |  Example: ``"serverParameters": { "-udfspMSR1=": 0 }``                                                      |
+------------------------+---------------------------------------------------------------------------------------------------------------+
| ``-udfspMSR1Address=`` | |  Description: Specify the address for DataFlash macro service error 1.                                      |
|                        | |  Default Value: 0xF1000                                                                                     |
|                        | |  Emulator: IECUBE                                                                                           |
|                        | |  Available Values: Any valid hex address.                                                                   |
|                        | |  Example: ``"serverParameters": { "-udfspMSR1Address=": "0xF1000" }``                                       |
|                        | |  Note: This value will not be sent to GDB server if ``-udfspMSR1=`` is set to 0 (None).                     |
+------------------------+---------------------------------------------------------------------------------------------------------------+
| ``-udfspMSR1Mask=``    | |  Description: Specify the address mask for DataFlash macro service error 1.                                 |
|                        | |  Default Value: 0xFFFF                                                                                      |
|                        | |  Emulator: IECUBE                                                                                           |
|                        | |  Available Values: Any valid hex address.                                                                   |
|                        | |  Example: ``"serverParameters": { "-udfspMSR1Mask=": "0xFFFF" }``                                           |
|                        | |  Note: This value will not be sent to GDB server if ``-udfspMSR1=`` is set to 0 (None).                     |
+------------------------+---------------------------------------------------------------------------------------------------------------+
| ``-udfspMSR2=``        | |  Description: Specify the macro service error 2 for DataFlash.                                              |
|                        | |  Default Value: 0 (None)                                                                                    |
|                        | |  Emulator: IECUBE                                                                                           |
|                        | |  Available Values:                                                                                          |
|                        | |    0: None.                                                                                                 |
|                        | |    1: Blank Check Error.                                                                                    |
|                        | |    2: Write Error.                                                                                          |
|                        | |    3: Verify Error.                                                                                         |
|                        | |  Example: ``"serverParameters": { "-udfspMSR2=": 0 }``                                                      |
+------------------------+---------------------------------------------------------------------------------------------------------------+
| ``-udfspMSR2Address=`` | |  Description: Specify the address for DataFlash macro service error 2.                                      |
|                        | |  Default Value: 0xF1000                                                                                     |
|                        | |  Emulator: IECUBE                                                                                           |
|                        | |  Available Values: Any valid hex address.                                                                   |
|                        | |  Example: ``"serverParameters": { "-udfspMSR2Address=": "0xF1000" }``                                       |
|                        | |  Note: This value will not be sent to GDB server if ``-udfspMSR2=`` is set to 0 (None).                     |
+------------------------+---------------------------------------------------------------------------------------------------------------+
| ``-udfspMSR2Mask=``    | |  Description: Specify the address mask for DataFlash macro service error 2.                                 |
|                        | |  Default Value: 0xFFFF                                                                                      |
|                        | |  Emulator: IECUBE                                                                                           |
|                        | |  Available Values: Any valid hex address.                                                                   |
|                        | |  Example: ``"serverParameters": { "-udfspMSR2Mask=": "0xFFFF" }``                                           |
|                        | |  Note: This value will not be sent to GDB server if ``-udfspMSR2=`` is set to 0 (None).                     |
+------------------------+---------------------------------------------------------------------------------------------------------------+
