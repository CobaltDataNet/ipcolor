# ipcolor - Colorized `ip` Command Output

A Bash script to add ANSI colors to the output of the `ip` command (e.g., `ip link`, `ip addr`) for better readability in terminals like PuTTY.

## Features
- Colors network interface names in **green**.
- Highlights `state UP` in **yellow** and `state DOWN` in **red**.
- Shows MAC addresses (`link/ether`) in **cyan**.
- Leaves other text in the default color (e.g., grey in PuTTY).

## Prerequisites
- Linux system with `ip` command (from `iproute2`).
- Terminal supporting ANSI colors (e.g., PuTTY with `xterm-256color` and "Allow ANSI colours" enabled).
- Bash shell.

## Installation
1. **Clone the Repository**:
   ```bash
   git clone https://github.com/CobaltDataNet/ipcolor.git /tmp/ipcolor-repo
   ```

2. **Install the Script**:
   ```bash
   sudo cp /tmp/ipcolor-repo/ipcolor /usr/local/bin/ipcolor
   sudo chmod +x /usr/local/bin/ipcolor
   rm -rf /tmp/ipcolor-repo
   ```

3. **Optional: Alias `ip`**:
   ```bash
   echo "alias ip='/usr/local/bin/ipcolor'" >> ~/.bashrc
   source ~/.bashrc
   ```

## Usage
Run `ipcolor` with any `ip` command arguments:
- List network interfaces:
  ```bash
  ipcolor link
  ```
- Show IP addresses:
  ```bash
  ipcolor addr
  ```
- View routing table:
  ```bash
  ipcolor route
  ```
- If aliased, replace `ipcolor` with `ip` (e.g., `ip link`).

### Example Output
```bash
ipcolor link
```
```
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: enp2s0f0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP mode DEFAULT group default qlen 1000
    link/ether 40:6c:8f:36:0e:8c brd ff:ff:ff:ff:ff:ff
```
- `lo`, `enp2s0f0` in green.
- `state UP` in yellow.
- `40:6c:8f:36:0e:8c` in cyan.

## Customization
Edit `/usr/local/bin/ipcolor` to change colors:
- `\e[32m`: Green (interfaces).
- `\e[33m`: Yellow (`UP`).
- `\e[31m`: Red (`DOWN`).
- `\e[36m`: Cyan (MAC).
- Example: Change green to blue with `\e[34m`.

## Troubleshooting
- **No Colors?**
  - Ensure `$TERM` is `xterm-256color`: `echo $TERM`.
  - In PuTTY: Connection > Data > Terminal-type string = `xterm-256color`.
  - Check PuTTY: Window > Colours > "Allow terminal to specify ANSI colours".

- **`-bash: /bin/brew: No such file or directory` Error**
  - This error suggests something in your environment (e.g., `~/.bashrc`) is trying to call `brew` (Homebrew package manager), but it's not installed or not located at `/bin/brew`. To fix:
    1. Open your `~/.bashrc`:
       ```bash
       nano ~/.bashrc
       ```
    2. Look for lines referencing `/bin/brew` (e.g., an alias, function, or PATH modification). Comment them out with `#` or remove them.
    3. Save, exit, and reload:
       ```bash
       source ~/.bashrc
       ```
    4. If you want Homebrew, install it: `/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`.

- **Command Not Found?**
  - Verify `/sbin/ip` exists; adjust path in script if needed (e.g., `/bin/ip`).

- **Cannot Stat Error After Deleting Repo**
  - If you see an error like `cp: cannot stat '/tmp/ipcolor-repo/ipcolor'`, it's because `/tmp/ipcolor-repo` was already deleted with `rm -rf`. Re-clone the repo:
    ```bash
    git clone https://github.com/CobaltDataNet/ipcolor.git /tmp/ipcolor-repo
    sudo cp /tmp/ipcolor-repo/ipcolor /usr/local/bin/ipcolor
    sudo chmod +x /usr/local/bin/ipcolor
    rm -rf /tmp/ipcolor-repo
    ```

## Contributing
Fork this repo, make changes, and submit a pull request at `https://github.com/CobaltDataNet/ipcolor`.

## License
This project is licensed under the MIT License. See the LICENSE file for details.
