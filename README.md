# groundwork-sim-money-editor
Use this tool to edit your available balance in groundwork simulator


Instructions
Groundwork Simulator: change your starting balance

Use this on single-player saves only, and only after you’ve confirmed it works for you. It was made by reverse-engineering an Early Access build (v0.1), so an update could break it.

1. Install Python (one time)
Get it from python.org. In the installer, tick “Add python.exe to PATH”.

2. Install the helper library (one time)
Open Command Prompt (Win+R, type cmd, Enter) and run:

py -m pip install msgpack

3. Back up your saves
Close the game completely. Copy your whole save folder somewhere safe.

4. Run the script
Put edit_balance.py in the same folder as your save file (for example slot-01.groundwork-simulator). In Command Prompt, go to that folder with cd, then run:

py edit_balance.py slot-01.groundwork-simulator slot-01-new.groundwork-simulator 5000000

The last number is the balance you want. This makes a new file and leaves your original untouched.

5. Swap the file in
Delete or rename your original save, then rename slot-01-new.groundwork-simulator to slot-01.groundwork-simulator. If you use Steam Cloud, turn it off in the game settings first, or Steam may restore the old file.

6. Launch and load that slot

If it doesn’t work: the game moves rejected saves to a file with quarantine in its name. Put your backup back and don’t try again until the game updates.
