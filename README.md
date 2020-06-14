## Table of Contents
1. [Installation](#installation)
2. [Configuration](#configuration)
3. [Usage](#usage)
4. [Stopping The Bot](#stopping-the-bot)

## Installation
Before installing packages and running this script, verify that you have Python version 3.7.x (or newer) installed on your system by running the command:
```console
python3 --version
```

You will also need to make sure that you have pip3 installed on your system. You can check for this by running:
```console
which pip3
```

If there is no output, then you need to install pip3. This can be done by running:
```console
sudo apt install python3-venv python3-pip
```

If this fails, you may need to update your system files by running this prior to installing pip3:
```console
apt-get update
```

Once you have verified that pip3 and Python has been installed on your system, open the directory where these files are stored in a command line tool and install the required Python packages using:
```console
pip3 install -r requirements.txt
```

If that method throws any errors, try piping the call to pip through Python using:
```console
python3 -m pip install -r requirements.txt
```

## Configuration

Open [config.conf](/config.conf) in your text editor and change the values to suit your needs.

The variables under **[LOGS]** are files that will be created during runtime to store information. These do not have to be edited.

The variables under **[TOKEN]** are as follows:
<pre>
- ID:			This is the token ID that will be exchanged. The default is PI (1002884)
- PRECISION:		This is the token's precision (will multiply the token amount sent by 10^PRECISION).
- PURCHASE_RATIO:	This is the exchange rate of TRX to tokens. The default is 1 (1 token for every 1 TRX).
- PURCHASE_MIN:		The minimum amount of tokens that can be exchanged. The default is 1.
- PURCHASE_MAX:		The maximum amount of tokens that can be exchanged. The default is 25000.
</pre>

The variables under **[API]** are the urls/IP addresses for the Tron full node, solidity node, and event server (respectively).

The variables under **[BOT]** are as follows:

<pre>
- BOT_PK:		The exchange bot's private key
- BOT_ADDR:		The exchange bot's address
</pre>

## Usage
To run the script, use the [shell start script](/start.sh) with the command:
```console
sh start.sh [-h] [-ctosdk]
```
Command line arguments' details can be found by using the "-h" or "--help" option following the shell command.
Here is a breakdown of the other command line options:

<pre>
-h or --help            Displays these command line arguments.
-c or --continue        Continue Mode: Disables log reset so the exchange bot can continue where it left off
-t or --test            Test Mode: Will do everything except send transactions
-o or --offline         Offline Mode: Will only keep local files. Disables uploads to MongoDB
-s or --silent          Silent Mode: Will only write errors and warnings to log
-d or --debug           Debug Mode: Will output extra debugging information to log
-k or --kill            Kills any running process without starting a new one
</pre>

The above command will run the script as a background process, so you do not need to keep the console open after starting. If you do not want the script to run as a background process (and write to a log file), then do not use the [shell start script](/start.sh). Instead, just run the [Python script](/pi_exchange.py) directly using the same command line arguments above:
```console
python3 pi_exchange.py [-h] [-ctosdk]
```

## Stopping The Bot
Once the script is running, there are a few ways to stop it:
- If you are running it as a foreground process (bot started by running the Python script and not using the [bash start script](/start.sh)), you can kill the script by pressing CTRL+c. 
- If you started it as a background process, you can run the start script again and add the "-k" or "--kill" command line argument and it will search for a PID file in the directory that contains the script's process ID and use that to kill the process. That command will look like this:
  ```console
  sh start.sh -k
  ```
- If, for some reason, the PID file can not be found or was accidentally deleted, you can find the process ID manually by running the following:
  ```console
  ps -ax | grep pi_exchange
  ```
  This will show two processes: the background script and the command you just ran. The background script should be the lower of the two PID values (the numbers on the left). Once you have the PID number, run the following command to manually kill the script:
  ```console
  kill -9 [PID]
  ```
  Where [PID] is the PID number you found from the "ps -ax | grep pi_exchange" command.
