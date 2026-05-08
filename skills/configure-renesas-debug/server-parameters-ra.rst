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

#####################
 Connection Settings
#####################

J-Link
^^^^^^

+---------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Option                    | Details                                                                                                                                                                                                                                     |
+---------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-uSelect=``             | |  Description: Specify connection type when using J-Link.                                                                                                                                                                                  |
|                           | |  Default Values: None. The J-Link will be selected automatically.                                                                                                                                                                         |
|                           | |  Emulator: J-Link                                                                                                                                                                                                                         |
|                           | |  Available Values: Using USB: USB=<J-Link Serial number>, Using IP via LAN: IP=<Host Name/IP address: port number>, Using IP via tunnel: IP=tunnel                                                                                        |
|                           | |  Example: Select USB with serial number 000051000569: ``"serverParameters": { "-uSelect=": "USB=000051000569" }``, Select IP with address 127.0.0.1 and port 61234: ``"serverParameters": { "-uSelect=": "IP=127.0.0.1:61234" }``         |
|                           | |  Note: If using IP via tunnel, it's needed to specify the following parameters.                                                                                                                                                           |
+---------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-uIpTunnelIdentifier=`` | |  Description: Specify the identifier for J-Link remote server's tunnel mode.                                                                                                                                                              |
|                           | |  Default Values: None                                                                                                                                                                                                                     |
|                           | |  Emulator: J-Link                                                                                                                                                                                                                         |
|                           | |  Available Values: Any valid identifier                                                                                                                                                                                                   |
|                           | |  Example: Set the identifier using serial for J-Link having serial 281100001: ``"serverParameters": { "-uIpTunnelIdentifier=": "281100001" }``                                                                                            |
|                           | |  Note: Identifier should be the serial or nickname of the J-Link. Use this parameter only when connecting via tunnel.                                                                                                                     |
+---------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-uIpTunnelServer=``     | |  Description: Specify the tunnel server for J-Link remote server's tunnel mode.                                                                                                                                                           |
|                           | |  Default Values: None                                                                                                                                                                                                                     |
|                           | |  Emulator: J-Link                                                                                                                                                                                                                         |
|                           | |  Available Values: Any valid tunnel server                                                                                                                                                                                                |
|                           | |  Example: ``"serverParameters": { "-uIpTunnelServer=": "jlink-asia.segger.com" }``                                                                                                                                                        |
|                           | |  Note: Use this parameter only when connecting via tunnel.                                                                                                                                                                                |
+---------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-uIpTunnelPort=``       | |  Description: Specify the port number for J-Link remote server's tunnel mode.                                                                                                                                                             |
|                           | |  Default Values: None                                                                                                                                                                                                                     |
|                           | |  Emulator: J-Link                                                                                                                                                                                                                         |
|                           | |  Available Values: Valid port number                                                                                                                                                                                                      |
|                           | |  Example: ``"serverParameters": { "-uIpTunnelPort=": 61234 }``                                                                                                                                                                            |
|                           | |  Note: Use this parameter only when connecting via tunnel.                                                                                                                                                                                |
+---------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-uIpTunnelPassword=``   | |  Description: Specify the password for J-Link remote server's tunnel mode.                                                                                                                                                                |
|                           | |  Default Values: None                                                                                                                                                                                                                     |
|                           | |  Emulator: J-Link                                                                                                                                                                                                                         |
|                           | |  Available Values: Letter, number, and special characters.                                                                                                                                                                                |
|                           | |  Example: ``"serverParameters": { "-uIpTunnelPassword=": "Pass1234" }``                                                                                                                                                                   |
|                           | |  Note: Use this parameter only when connecting via tunnel.                                                                                                                                                                                |
+---------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-uJLinkSetting=``       | |  Description: Specify the file path containing J-Link settings.                                                                                                                                                                           |
|                           | |  Default Values: None                                                                                                                                                                                                                     |
|                           | |  Emulator: J-Link                                                                                                                                                                                                                         |
|                           | |  Available Values: Any valid path to .jlink file                                                                                                                                                                                          |
|                           | |  Example: ``"serverParameters": { "-uJLinkSetting=": "D:/dev_test/N20220713-1808/workspace/ra6t1/ra6t1 Debug_Flat.jlink" }``                                                                                                              |
+---------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-uJLinkScript=``        | |  Description: Specify the file path containing J-Link script.                                                                                                                                                                             |
|                           | |  Default Values : None                                                                                                                                                                                                                    |
|                           | |  Emulator: J-Link                                                                                                                                                                                                                         |
|                           | |  Available Values: Any valid path to script file                                                                                                                                                                                          |
|                           | |  Example: ``"serverParameters": { "-uJLinkScript=": "D:/dev_test/N20220917-1807/workspace/ra6e1/ra6e1.JLinkScript" }``                                                                                                                    |
+---------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-uJLinkLog=``           | |  Description: Specify the log file for the J-Link to output logging information. If the log file already exists, the contents of the current log file will be overwritten. If the option is empty, no logging information is outputted.   |
|                           | |  Default Values: None                                                                                                                                                                                                                     |
|                           | |  Available Values: Any valid path to log file. The log file path must not contain any blank space characters.                                                                                                                             |
|                           | |  Example: ``"serverParameters": { "-uJLinkLog=": "D:/dev_test/e2studio/workspace/project/JLinkLog.log" }``                                                                                                                                |
|                           | |  Note: J-Link web control panel is prioritized over this configuration. The override option for log file in J-Link web control panel must be disabled before using this parameter.                                                        |
+---------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Clock
^^^^^

+---------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------+
| Option                          | Details                                                                                                                                   |
+---------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------+
| ``-uClockSrc=``                 | |  Description: Specify the Main Clock Source.                                                                                            |
|                                 | |  Default Values: 0                                                                                                                      |
|                                 | |  Emulator: E2, E2Lite                                                                                                                   |
|                                 | |  Available Values: 0: Internal, 1: External                                                                                             |
|                                 | |  Example: ``"serverParameters": { "-uClockSrc=": 0 }``                                                                                  |
+---------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------+
| ``-uInputClock=``               | |  Description: Specify External frequency in MHz when using External Clock source.                                                       |
|                                 | |  Default Values: None                                                                                                                   |
|                                 | |  Emulator: E2, E2Lite                                                                                                                   |
|                                 | |  Available Values: Between 0.001 to 99.999                                                                                              |
|                                 | |  Example: ``"serverParameters": { "-uInputClock=": 16.00 }``                                                                            |
+---------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------+
| ``-uAllowClockSourceInternal=`` | |  Description: Specify whether or not to allow manipulation of the main clock source by debugger when internal flash memory is rewritten.|
|                                 | |  Default Values: 1                                                                                                                      |
|                                 | |  Emulator: E2, E2Lite                                                                                                                   |
|                                 | |  Available Values: 0: No, 1: Yes                                                                                                        |
|                                 | |  Example: ``"serverParameters": { "-uAllowClockSourceInternal=": 0 }``                                                                  |
+---------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------+
| ``-uCPUFrequency=``             | |  Description: Specify the operating frequency (ICLK) in Hz. It is used to calculate the timestamp of SWO trace.                         |
|                                 | |  Default Values: None                                                                                                                   |
|                                 | |  Emulator: E2, E2Lite                                                                                                                   |
|                                 | |  Available Values: The frequency in Hz in range 1000 - 999999000                                                                        |
|                                 | |  Example: ``"serverParameters": { "-uCPUFrequency=": 120000000 }``                                                                      |
+---------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------+

Emulator
^^^^^^^^

+------------+-------------------------------------------------------------------------------------------+
| Option     | Details                                                                                   |
+------------+-------------------------------------------------------------------------------------------+
| ``-sn=``   | |  Description: Specify the emulator serial number to be connected.                       |
|            | |  Default Values: Auto (Emulator will be selected automatically.)                        |
|            | |  Emulator: E2, E2Lite                                                                   |
|            | |  Available Values: Emulator information with format: <emulator type>:<serial number>    |
|            | |  Example: ``"serverParameters": { "-sn=": "e2:_9es002310c" }``                          |
+------------+-------------------------------------------------------------------------------------------+

Interface
^^^^^^^^^

+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Option          | Details                                                                                                                                                                        |
+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-uInteface=`` | |  Description: Specify the connection type to be used by the emulator when communicating with the CPU on target system.                                                       |
|                 | |  Default Values: JTAG                                                                                                                                                        |
|                 | |  Emulator: E2, E2Lite, J-Link                                                                                                                                                |
|                 | |  Available Values: SWD: Type SWD, JTAG: Type JTAG                                                                                                                            |
|                 | |  Example: ``"serverParameters": { "-uInteface=": "SWD" }``                                                                                                                   |
+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-uIfSpeed=``  | |  Description: Specify the interface speed in kHz.                                                                                                                            |
|                 | |  Default Values: auto                                                                                                                                                        |
|                 | |  Emulator: E2, E2Lite, J-Link                                                                                                                                                |
|                 | |  Available Values: auto (Selects the appropriate JTAG speed handled by the emulator),                                                                                        |
|                 |                      adaptive (If the target provides the RTCK signal, select the adaptive clocking function to synchronize the clock to the processor clock outside the core),|
|                 |                      From 1 to 100000 (Specify value for interface speed in kHz)                                                                                               |
|                 | |  Example: ``"serverParameters": { "-uIfSpeed=": 4000 }``                                                                                                                     |
|                 | |  Note: "adaptive" option is just available for J-Link                                                                                                                        |
+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Power
^^^^^

+------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Option     | Details                                                                                                                                                                       |
+------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-w``     | |  Description: Specify the power source.                                                                                                                                     |
|            | |  Default Values: 0                                                                                                                                                          |
|            | |  Emulator: E2, E2Lite                                                                                                                                                       |
|            | |  Available Values: 0, 1, 2                                                                                                                                                  |
|            | |  Example: ``"serverParameters": { "-w": 1 }``                                                                                                                               |
|            | |  Note: Value ``2`` is just supported for E2 emulator.                                                                                                                       |
+------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-z``     | |  Description: Specify the power voltage supplied to the target board.                                                                                                       |
|            | |  Default Values: 33                                                                                                                                                         |
|            | |  Emulator: E2, E2Lite                                                                                                                                                       |
|            | |  Available Values: 0: 3.3 V, 1: 5.0 V, From 18 to 50: From 1.8 V to 5.0 V                                                                                                   |
|            | |  Example: ``"serverParameters": { "-z": 0 }``: using for 3.3 V, ``"serverParameters": { "-z": 1 }``: using for 5.0 V, ``"serverParameters": { "-z": 18 }``: using for 1.8 V |
|            | |  Note: E2Lite emulator is just supported 3.3 V. Just E2 emulator allows to specify a arbitrary value from 1.8 V to 5.0 V                                                    |
+------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

JTAG Scan Chain
^^^^^^^^^^^^^^^

+----------------+--------------------------------------------------------------------------------------------------------------------------------+
| Option         | Details                                                                                                                        |
+----------------+--------------------------------------------------------------------------------------------------------------------------------+
| ``-uscIrPre=`` | |  Description: Specify the number of IR-bits of all devices which are closer to TDO than the one we want to communicate with. |
|                | |  Default Values: 0                                                                                                           |
|                | |  Emulator: J-Link                                                                                                            |
|                | |  Available Values: Valid integer number of IR-bits                                                                           |
|                | |  Example: ``"serverParameters": { "-uscIrPre=": 0 }``                                                                        |
+----------------+--------------------------------------------------------------------------------------------------------------------------------+
| ``-uscDrPre=`` | |  Description: Specify the number of devices which are closer to TDO than the one we want to communicate with.                |
|                | |  Default Values: 0                                                                                                           |
|                | |  Emulator: J-Link                                                                                                            |
|                | |  Available Values: Valid integer number                                                                                      |
|                | |  Example: ``"serverParameters": { "-uscDrPre=": 0 }``                                                                        |
+----------------+--------------------------------------------------------------------------------------------------------------------------------+

Connection
^^^^^^^^^^

+--------------------------------+-----------------------------------------------------------------------------------------------------------------+
| Option                         | Details                                                                                                         |
+--------------------------------+-----------------------------------------------------------------------------------------------------------------+
| ``-uResetCon=``                | |  Description: Specify whether to hold reset during the connection progress.                                   |
|                                | |  Default Values: 0                                                                                            |
|                                | |  Emulator: E2, E2Lite, J-Link                                                                                 |
|                                | |  Available Values: 0: No, 1: Yes                                                                              |
|                                | |  Example: ``"serverParameters": { "-uResetCon=": 1 }``                                                        |
+--------------------------------+-----------------------------------------------------------------------------------------------------------------+
| ``-uResetBeginConnection=``    | |  Description: Specify whether the CPU should be reset at the beginning of connection.                         |
|                                | |  Default Values: 1                                                                                            |
|                                | |  Emulator: E2, E2Lite, J-Link                                                                                 |
|                                | |  Available Values: 0: No, 1: Yes                                                                              |
|                                | |  Example: ``"serverParameters": { "-uResetBeginConnection=": 1 }``                                            |
+--------------------------------+-----------------------------------------------------------------------------------------------------------------+
| ``-uNoReset=``                 | |  Description: Specify whether the CPU should not be reset at the end of connection.                           |
|                                | |  Default Values: 1                                                                                            |
|                                | |  Emulator: E2, E2Lite, J-Link                                                                                 |
|                                | |  Available Values: 0: Reset, 1: No Reset                                                                      |
|                                | |  Example: ``"serverParameters": { "-uNoReset=": 1 }``                                                         |
+--------------------------------+-----------------------------------------------------------------------------------------------------------------+
| ``-uIdCodeBytes=``             | |  Description: Specify the ID code to unlock the device (if one has been set previously to protect connection).|
|                                | |  Default Values: None                                                                                         |
|                                | |  Emulator: E2, E2Lite, J-Link                                                                                 |
|                                | |  Available Values: 32 hex digits                                                                              |
|                                | |  Example: ``"serverParameters": { "-uIdCodeBytes=": "FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF" }``                    |
+--------------------------------+-----------------------------------------------------------------------------------------------------------------+
| ``-uLowPower=``                | |  Description: Specify if the program uses low power consumption mode or not.                                  |
|                                | |  Default Values: 0                                                                                            |
|                                | |  Emulator: E2, E2Lite, J-Link                                                                                 |
|                                | |  Available Values: 0: No, 1: Yes                                                                              |
|                                | |  Example: ``"serverParameters": { "-uLowPower=": 0 }``                                                        |
+--------------------------------+-----------------------------------------------------------------------------------------------------------------+
| ``-uInitRegisters=``           | |  Description: Specify if registers should be initialized on connection.                                       |
|                                | |  Default Values: 0                                                                                            |
|                                | |  Emulator: J-Link                                                                                             |
|                                | |  Available Values: 0: No, 1: Yes                                                                              |
|                                | |  Example: ``"serverParameters": { "-uInitRegisters=": 0 }``                                                   |
+--------------------------------+-----------------------------------------------------------------------------------------------------------------+
| ``-uResetPreRun=``             | |  Description: Specify if an additional reset should be applied before the downloaded module is run.           |
|                                | |  Default Values: 0                                                                                            |
|                                | |  Emulator: J-Link                                                                                             |
|                                | |  Available Values: 0: No, 1: Yes                                                                              |
|                                | |  Example: ``"serverParameters": { "-uResetPreRun=": 0 }``                                                     |
+--------------------------------+-----------------------------------------------------------------------------------------------------------------+
| ``-uResetBefDownload=``        | |  Description: Specify if an additional reset should be applied before the module is downloaded.               |
|                                | |  Default Values: 0                                                                                            |
|                                | |  Emulator: J-Link                                                                                             |
|                                | |  Available Values: 0: No, 1: Yes                                                                              |
|                                | |  Example: ``"serverParameters": { "-uResetBefDownload=": 0 }``                                                |
+--------------------------------+-----------------------------------------------------------------------------------------------------------------+
| ``-uSetArmModeAfterDownload=`` | |  Description: Specify if CPSR(5 bit) should be set to ARM mode after the module is downloaded.                |
|                                | |  Default Values: 0                                                                                            |
|                                | |  Emulator: J-Link                                                                                             |
|                                | |  Available Values: 0: No, 1: Yes                                                                              |
|                                | |  Example: ``"serverParameters": { "-uSetArmModeAfterDownload=": 0 }``                                         |
+--------------------------------+-----------------------------------------------------------------------------------------------------------------+
| ``-uReleaseCM3=``              | |  Description: Specify whether prevent run away at launching a debug session for Cortex-M3 core.               |
|                                | |  Default Values: 1                                                                                            |
|                                | |  Emulator: J-Link                                                                                             |
|                                | |  Available Values: 0: No, 1: Yes                                                                              |
|                                | |  Example: ``"serverParameters": { "-uReleaseCM3=": 0 }``                                                      |
+--------------------------------+-----------------------------------------------------------------------------------------------------------------+
| ``-uSecureVectorAddress=``     | |  Description: Specify the Secure Vector address.                                                              |
|                                | |  Default Values: None                                                                                         |
|                                | |  Emulator: E2, E2Lite, J-Link                                                                                 |
|                                | |  Available Values: The valid hex address.                                                                     |
|                                | |  Example: ``"serverParameters": { "-uSecureVectorAddress=": "0x10010110" }``                                  |
+--------------------------------+-----------------------------------------------------------------------------------------------------------------+
| ``-uNonSecureVectorAddress=``  | |  Description: Specify the Non Secure Vector address.                                                          |
|                                | |  Default Values: None                                                                                         |
|                                | |  Emulator: E2, E2Lite, J-Link                                                                                 |
|                                | |  Available Values: The valid hex address.                                                                     |
|                                | |  Example: ``"serverParameters": { "-uNonSecureVectorAddress=": "0x10010110" }``                               |
+--------------------------------+-----------------------------------------------------------------------------------------------------------------+
| ``-uHotPlug=``                 | |  Description: Specify if connecting to an already running target system.                                      |
|                                | |  Default Values: 0                                                                                            |
|                                | |  Emulator: E2, E2Lite, J-Link                                                                                 |
|                                | |  Available Values: 0: Disabled, 1: Enabled                                                                    |
|                                | |  Example: ``"serverParameters": { "-uHotPlug=": 1 }``                                                         |
+--------------------------------+-----------------------------------------------------------------------------------------------------------------+
| ``-uDisconnectionMode=``       | |  Description: Specify the disconnection mode to control the target state after disconnecting.                 |
|                                | |  Default Values: 2                                                                                            |
|                                | |  Emulator: E2, E2Lite, J-Link                                                                                 |
|                                | |  Available Values: 0: No change mode, 1: Stop mode, 2: Continue mode                                          |
|                                | |  Example: ``"serverParameters": { "-uDisconnectionMode=": 1 }``                                               |
+--------------------------------+-----------------------------------------------------------------------------------------------------------------+

SWV
^^^

+---------------------+-------------------------------------------------------------------------------------------------------------------------------------+
| Option              | Details                                                                                                                             |
+---------------------+-------------------------------------------------------------------------------------------------------------------------------------+
| ``-uSWOcoreClock=`` | |  Description: Specify the core clock speed in Hz.                                                                                 |
|                     | |  Default Values: 0                                                                                                                |
|                     | |  Emulator: J-Link                                                                                                                 |
|                     | |  Available Values: Valid frequency value in Hz                                                                                    |
|                     | |  Example: With 8 MHz: ``"serverParameters": { "-uSWOcoreClock=": 8000000 }``                                                      |
+---------------------+-------------------------------------------------------------------------------------------------------------------------------------+
| ``-uSWOclockDiv=``  | |  Description: Specify SWO frequency by division value. The SWO frequency is decided by dividing Clock Src of ICLK with this value.|
|                     | |  Default Values: 4                                                                                                                |
|                     | |  Emulator: E2                                                                                                                     |
|                     | |  Available Values: 0 (1), 1 (2), 2 (4), 3 (8), 4 (16), 5 (32), 6 (64), 7 (128), 8 (256), 9 (512), 10 (1024)                       |
|                     | |  Example: Set the SWO frequency to 256: ``"serverParameters": { "-uSWOclockDiv=": 8 }``                                           |
|                     | |  Note: SWO will be supported only for type SWD                                                                                    |
+---------------------+-------------------------------------------------------------------------------------------------------------------------------------+

TrustZone
^^^^^^^^^

+----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+
| Option               | Details                                                                                                                                                 |
+----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-uEnableSciBoot=`` | |  Description: Specify whether the boundaries of memory regions need to be set to separate each memory region when a debugging connection is initiated.|
|                      | |  Default Values: 0                                                                                                                                    |
|                      | |  Emulator: E2, E2Lite, J-Link                                                                                                                         |
|                      | |  Available Values: 0: Disabled, 1: Enabled                                                                                                            |
|                      | |  Example: ``"serverParameters": { "-uEnableSciBoot=": 1 }``                                                                                           |
+----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-uMemRegion=``     | |  Description: Specify the memory region.                                                                                                              |
|                      | |  Default Values: None                                                                                                                                 |
|                      | |  Emulator: E2, E2Lite, J-Link                                                                                                                         |
|                      | |  Available Values: The values use the format: <start address>:<size>:<memory type>:<memory security attribute>                                        |
|                      | |  Example: ``"serverParameters": { "-uMemRegion=": "0x20000000:0x800:RAM:s" }``                                                                        |
+----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-uTz=``            | |  Description: Specify the debugging type of TrustZone.                                                                                                |
|                      | |  Default Values: None                                                                                                                                 |
|                      | |  Emulator: E2, E2Lite, J-Link                                                                                                                         |
|                      | |  Available Values: SSD: Secure Software Development, NSECSD: Non Secure Software Development                                                          |
|                      | |  Example: ``"serverParameters": { "-uTz=": "NSECSD" }``                                                                                               |
+----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-uAuthLevel=``     | |  Description: Specify the authentication level.                                                                                                       |
|                      | |  Default Values: 0                                                                                                                                    |
|                      | |  Emulator: E2, E2Lite, J-Link                                                                                                                         |
|                      | |  Available Values: 0 (AL0: Ignores the key and connect without authentication), 1 (AL1: Authenticate device with key to AL1),                         |
|                      |                      3 (AL2: Authenticate device with key to AL2), 255 (ALERASE: Initialize device back to AL2)                                         |
|                      | |  Example: ``"serverParameters": { "-uAuthLevel=": 3 }``                                                                                               |
|                      | |  Note: Use this parameter only with TrustZone devices that support Authentication Level                                                               |
+----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-uAuthKey=``       | |  Description: Specify the Authentication Key.                                                                                                         |
|                      | |  Default Values: None                                                                                                                                 |
|                      | |  Emulator: E2, E2Lite, J-Link                                                                                                                         |
|                      | |  Available Values: Key for Authentication                                                                                                             |
|                      | |  Example: ``"serverParameters": { "-uAuthKey=": "ACD6B31BF68FE9AE07AEF6B3C3870730" }``                                                                |
|                      | |  Note: Use this parameter only with TrustZone devices that support Authentication Level                                                               |
+----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+

.. note::

   The set of values uses the format: ``<start address>:<size>:<memory type>:<memory security attribute>``

   **Memory type:**
   - ``FLASH``
   - ``DATA_FLASH``
   - ``RAM``
   - ``FLASH_NS``
   - ``DATA_FLASH_NS``
   - ``RAM_NS``
   - Other types are accepted.

   **Memory security attribute:**

   +-----------+-------------------------+
   | Argument  | Actual                  |
   +===========+=========================+
   | n         | Non-Secure              |
   +-----------+-------------------------+
   | c         | Non-Secure Callable     |
   +-----------+-------------------------+
   | e         | Exempted                |
   +-----------+-------------------------+
   | s         | Secure                  |
   +-----------+-------------------------+

Start/Stop Function Setting
^^^^^^^^^^^^^^^^^^^^^^^^^^^

+------------------------------+-------------------------------------------------------------------------------------------------------+
| Option                       | Details                                                                                               |
+------------------------------+-------------------------------------------------------------------------------------------------------+
| ``-uhookEnableStart=``       | |  Description: Specify if a function should be executed before running user program.                 |
|                              | |  Default Values: 0                                                                                  |
|                              | |  Emulator: E2, E2Lite                                                                               |
|                              | |  Available Values: 0: No, 1: Yes                                                                    |
|                              | |  Example: ``"serverParameters": { "-uhookEnableStart=": 0 }``                                       |
+------------------------------+-------------------------------------------------------------------------------------------------------+
| ``-uhookStartFunc=``         | |  Description: Specify the address of the start function.                                            |
|                              | |  Default Values: 0                                                                                  |
|                              | |  Emulator: E2, E2Lite                                                                               |
|                              | |  Available Values: Any valid hex address.                                                           |
|                              | |  Example: ``"serverParameters": { "-uhookStartFunc=": "&R_BSP_ChangeToUserMode" }``                 |
|                              | |  Note: This parameter is just sent when allow to execute function before running user program       |
+------------------------------+-------------------------------------------------------------------------------------------------------+
| ``-uhookEnableStop=``        | |  Description: Specify if a function should be executed after stopping user program.                 |
|                              | |  Default Values: 0                                                                                  |
|                              | |  Emulator: E2, E2Lite                                                                               |
|                              | |  Available Values: 0: No, 1: Yes                                                                    |
|                              | |  Example: ``"serverParameters": { "-uhookEnableStop=": 0 }``                                        |
+------------------------------+-------------------------------------------------------------------------------------------------------+
| ``-uhookStopFunc=``          | |  Description: Specify the address of the stop function.                                             |
|                              | |  Default Values: None                                                                               |
|                              | |  Emulator: E2, E2Lite                                                                               |
|                              | |  Available Values: Any valid hex address.                                                           |
|                              | |  Example: ``"serverParameters": { "-uhookStopFunc=": "&R_BSP_InterruptRead" }``                     |
|                              | |  Note: This parameter is just sent when allow to execute function after stopping user program       |
+------------------------------+-------------------------------------------------------------------------------------------------------+
| ``-uhookWorkRamAddr=``       | |  Description: Specify the first address of the working RAM area for start/stop function.            |
|                              | |  Default Values: 0                                                                                  |
|                              | |  Emulator: E2, E2Lite                                                                               |
|                              | |  Available Values: The address needs to be in internal RAM area and it must be divisible by 0x1000. |
|                              | |  Example: ``"serverParameters": { "-uhookWorkRamAddr=": "0x20001000" }``                            |
+------------------------------+-------------------------------------------------------------------------------------------------------+

####################
 Debug Tool Settings
####################

Break
^^^^^

+--------------------+--------------------------------------------------------------------------------------------------------------------------------+
| Option             | Details                                                                                                                        |
+--------------------+--------------------------------------------------------------------------------------------------------------------------------+
| ``-uFlashBp=``     | |  Description: Specify whether to use software breakpoints to flash memory or not.                                            |
|                    | |  Default Values: 0                                                                                                           |
|                    | |  Emulator: E2, E2Lite, J-Link                                                                                                |
|                    | |  Available Values: 0: No, 1: Yes                                                                                             |
|                    | |  Example: ``"serverParameters": { "-uFlashBp=": 0 }``                                                                        |
|                    | |  Note: This parameter does not need to be specified if ``-uProgReWriteIRom`` or ``-uProgReWriteDFlash`` is sent with value 1.|
+--------------------+--------------------------------------------------------------------------------------------------------------------------------+
| ``-uSimulation=``  | |  Description: Specify if Instruction Set simulation will be used or not.                                                     |
|                    | |  Default Values: 0                                                                                                           |
|                    | |  Emulator: J-Link                                                                                                            |
|                    | |  Available Values: 0: No, 1: Yes                                                                                             |
|                    | |  Example: ``"serverParameters": { "-uSimulation=": 0 }``                                                                     |
|                    | |  Note: This parameter does not need to be specified if ``-uProgReWriteIRom`` or ``-uProgReWriteDFlash`` is sent with value 1.|
+--------------------+--------------------------------------------------------------------------------------------------------------------------------+

Flash
^^^^^

+-------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Option                        | Details                                                                                                                                                                         |
+-------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-ueraseRomOnDownload=``     | |  Description: Specify whether or not to erase the flash ROM (program ROM) before a download.                                                                                  |
|                               | |  Default Values: 0                                                                                                                                                            |
|                               | |  Emulator: E2, E2Lite, J-Link                                                                                                                                                 |
|                               | |  Available Values: 0: No, 1: Yes                                                                                                                                              |
|                               | |  Example: ``"serverParameters": { "-ueraseRomOnDownload=": 0 }``                                                                                                              |
+-------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-ueraseDataRomOnDownload=`` | |  Description: Specify whether or not to erase the data flash ROM before a download.                                                                                           |
|                               | |  Default Values: 0                                                                                                                                                            |
|                               | |  Emulator: E2, E2Lite, J-Link                                                                                                                                                 |
|                               | |  Available Values: 0: No, 1: Yes                                                                                                                                              |
|                               | |  Example: ``"serverParameters": { "-ueraseDataRomOnDownload=": 0 }``                                                                                                          |
+-------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-ucfiFlashWorkStart=``      | |  Description: Specify the start address of the Working RAM area which is used by JLink during flash programming.                                                              |
|                               | |  Default Values: 0                                                                                                                                                            |
|                               | |  Emulator: J-Link                                                                                                                                                             |
|                               | |  Available Values: Valid address                                                                                                                                              |
|                               | |  Example: ``"serverParameters": { "-ucfiFlashWorkStart=": "0x20001000" }``                                                                                                    |
|                               | |  Note: Start address and end address of Working RAM area have to be set together and start address have to be smaller than end address.                                       |
+-------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-ucfiFlashWorkEnd=``        | |  Description: Specify the end address of the Working RAM area which is used by JLink during flash programming.                                                                |
|                               | |  Default Values: None                                                                                                                                                         |
|                               | |  Emulator: E2, E2Lite                                                                                                                                                         |
|                               | |  Available Values: Valid address                                                                                                                                              |
|                               | |  Example: ``"serverParameters": { "-ucfiFlashWorkEnd=": "0x20004000" }``                                                                                                      |
|                               | |  Note: Start address and end address of Working RAM area have to be set together and end address have to be bigger than start address                                         |
+-------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-ucfiFlashStart=``          | |  Description: Specify the start address of CFI Flash Area.                                                                                                                    |
|                               | |  Default Values: 0                                                                                                                                                            |
|                               | |  Emulator: J-Link                                                                                                                                                             |
|                               | |  Available Values: Any valid hex address.                                                                                                                                     |
|                               | |  Example: ``"serverParameters": { "-ucfiFlashStart=": "&DebugMon_Handler" }``                                                                                                 |
|                               | |  Note: Start address and end address of CFI flash have to be set together and start address have to be smaller than end address.                                              |
|                               | |          The CFI flash address will not be set in Renesas GDB server if Work RAM address is not set before.                                                                   |
+-------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-ucfiFlashEnd=``            | |  Description: Specify the end address of CFI Flash Area.                                                                                                                      |
|                               | |  Default Values: 0                                                                                                                                                            |
|                               | |  Emulator: J-Link                                                                                                                                                             |
|                               | |  Available Values: Any valid hex address.                                                                                                                                     |
|                               | |  Example: ``"serverParameters": { "-ucfiFlashEnd=": "&PendSV_Handler" }``                                                                                                     |
|                               | |  Note: Start address and end address of CFI flash have to be set together and end address have to be bigger than start address.                                               |
|                               |          The CFI flash address will not be set in Renesas GDB server if Work RAM address is not set before.                                                                     |
+-------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-uExcludeFlashCacheRange=`` | |  Description: Specify whether enable and exclude the Flash Cache for a address range.                                                                                         |
|                               | |  Default Values: If this parameter is not sent, this feature will be disabled.                                                                                                |
|                               | |  Emulator: J-Link                                                                                                                                                             |
|                               | |  Available Values: Set of address value with below formats: <Start address>,<End address> or <Start address>-<End address>                                                    |
|                               | |  Example: ``"serverParameters": { "-uExcludeFlashCacheRange=": "0x10000000,0x100FFFFF" }`` or ``"serverParameters": { "-uExcludeFlashCacheRange=": "0x10000000-0x100FFFFF" }``|
+-------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-uflashMemoryType=``        | |  Description: Specify the Flash Memory type.                                                                                                                                  |
|                               | |  Default Values: None                                                                                                                                                         |
|                               | |  Emulator: J-Link                                                                                                                                                             |
|                               | |  Available Values: ``SerialFlash``, ``HyperFlash``, or ``OctaFlash``                                                                                                          |
|                               | |  Example: ``"serverParameters": { "-uflashMemoryType=": "SerialFlash" }``                                                                                                     |
+-------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-uflashBusType=``           | |  Description: Specify the Flash Bus type.                                                                                                                                     |
|                               | |  Default Values: None                                                                                                                                                         |
|                               | |  Emulator: J-Link                                                                                                                                                             |
|                               | |  Available Values: ``SPIBSC``, ``HyperBus``, or ``OctaBus``                                                                                                                   |
|                               | |  Example: ``"serverParameters": { "-uflashBusType=": "SPIBSC" }``                                                                                                             |
+-------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Trace
^^^^^

+---------------------+-----------------------------------------------------------------------------------+
| Option              | Details                                                                           |
+---------------------+-----------------------------------------------------------------------------------+
| ``-uTraceMTB=``     | |  Description: Specify whether or not to use MTB trace.                          |
|                     | |  Default Values: 0                                                              |
|                     | |  Emulator: E2, E2Lite                                                           |
|                     | |  Available Values: 0: No, 1: Yes                                                |
|                     | |  Example: ``"serverParameters": { "-uTraceMTB=": 0 }``                          |
+---------------------+-----------------------------------------------------------------------------------+
| ``-uTraceSizeMTB=`` | |  Description: Specify the size of MTB trace (byte).                             |
|                     | |  Default Values: 0                                                              |
|                     | |  Emulator: E2, E2Lite                                                           |
|                     | |  Available Values: Size in byte: 256, 512, 1024, 2048, 4096, 8192, 16384, 32768 |
|                     | |  Example: ``"serverParameters": { "-uTraceSizeMTB=": 1024 }``                   |
+---------------------+-----------------------------------------------------------------------------------+
| ``-uTraceCache=``   | |  Description: Specify the start address and size for Trace cache.               |
|                     | |  Default Values: None                                                           |
|                     | |  Emulator: J-Link                                                               |
|                     | |  Available Values: <start address>|<size>                                       |
|                     | |  Example: ``"serverParameters": { "-uTraceCache=": "0x00000000|0x10000" }``     |
+---------------------+-----------------------------------------------------------------------------------+

System
^^^^^^

+--------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------+
| Option                   | Details                                                                                                                                              |
+--------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-uProgReWriteIRom=``   | |  Description: Specify whether or not the program being debugged rewrites the internal program ROM.                                                 |
|                          | |  Default Values: 0                                                                                                                                 |
|                          | |  Emulator: E2, E2Lite                                                                                                                              |
|                          | |  Available Values: 0: No, 1: Yes                                                                                                                   |
|                          | |  Example: ``"serverParameters": { "-uProgReWriteIRom=": 0 }``                                                                                      |
+--------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-uProgReWriteDFlash=`` | |  Description: Specify whether or not to debug a program that involves rewriting the internal data flash.                                           |
|                          | |  Default Values: 0                                                                                                                                 |
|                          | |  Emulator: E2, E2Lite                                                                                                                              |
|                          | |  Available Values: 0: No, 1: Yes                                                                                                                   |
|                          | |  Example: ``"serverParameters": { "-uProgReWriteDFlash=": 0 }``                                                                                    |
+--------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-uAllowCachingFlash=`` | |  Description: Specify whether the flash contents are cached by J-Link or not.                                                                      |
|                          | |  Default Values: 0                                                                                                                                 |
|                          | |  Emulator: J-Link                                                                                                                                  |
|                          | |  Available Values: 0: Yes, 1: No                                                                                                                   |
|                          | |  Example: ``"serverParameters": { "-uAllowCachingFlash=": 0 }``                                                                                    |
|                          | |  Note: ``"-uAllowCachingFlash=": 0``: Enable caching for the flash contents, ``"-uAllowCachingFlash=": 1``: Disable caching for the flash contents |
+--------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------+

Time Measurement
^^^^^^^^^^^^^^^^

+------------------+------------------------------------------------------------------------------------------------------+
| Option           | Details                                                                                              |
+------------------+------------------------------------------------------------------------------------------------------+
| ``-uCECycle=``   | |  Description: For Renesas RZ targets, this will set the value for bit[3] of the PMCR register to 0.|
|                  | |  Default Values: 1                                                                                 |
|                  | |  Emulator: J-Link                                                                                  |
|                  | |  Available Values: 0: No, 1: Yes                                                                   |
|                  | |  Example: ``"serverParameters": { "-uCECycle=": 1 }``                                              |
+------------------+------------------------------------------------------------------------------------------------------+
| ``-uDWTEnable=`` | |  Description: The time is measured by the cycle count feature of the device.                       |
|                  | |  Default Values: 0                                                                                 |
|                  | |  Emulator: E2, E2Lite                                                                              |
|                  | |  Available Values: 0: No, 1: Yes                                                                   |
|                  | |  Example: ``"serverParameters": { "-uDWTEnable=": 1 }``                                            |
+------------------+------------------------------------------------------------------------------------------------------+

Reset Behaviour
^^^^^^^^^^^^^^^

+-----------------------+------------------------------------------------------------------------------------+
| Option                | Details                                                                            |
+-----------------------+------------------------------------------------------------------------------------+
| ``-uResetBehavior=``  | |  Description: Specify the behaviour for the reset command.                       |
|                       | |  Default Values: Reset                                                           |
|                       | |  Emulator: J-Link                                                                |
|                       | |  Available Values: ``Reset``, ``ChangePC``                                       |
|                       | |  Example: ``"serverParameters": { "-uResetBehavior=": "Reset" }``                |
+-----------------------+------------------------------------------------------------------------------------+
| ``-uPcOnReset=``      | |  Description: Specify the value which is set to PC.                              |
|                       | |  Default Values: None                                                            |
|                       | |  Emulator: J-Link                                                                |
|                       | |  Available Values: Any valid address or symbol                                   |
|                       | |  Example: ``"serverParameters": { "-uPcOnReset=": "&main" }``                    |
|                       | |  Note: If PC value is not specified, it will be set to the entry point value.    |
+-----------------------+------------------------------------------------------------------------------------+
| ``-uSetCpsrOnReset=`` | |  Description: Specify the value which is set to CPSR.                            |
|                       | |  Default Values: None                                                            |
|                       | |  Emulator: J-Link                                                                |
|                       | |  Available Values: Any valid address                                             |
|                       | |  Example: ``"serverParameters": { "-uSetCpsrOnReset=": 0x1da }``                 |
|                       | |  Note: If CPSR value is not specified, it will keep the original value.          |
+-----------------------+------------------------------------------------------------------------------------+
