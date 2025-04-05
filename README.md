# Wifi Password Ejector

This script retrieves the saved WiFi passwords from a Windows operating system and prints them out.

## Usage

1. Clone the repository:
    ```bash
    git clone https://github.com/shreyash0019/Wifi-Password-Ejector.git
    cd Wifi-Password-Ejector
    ```

2. Run the script:
    ```bash
    python main.py
    ```

## Script Explanation

The script uses the `subprocess` module to run the `netsh` command-line tool to retrieve the saved WiFi profiles and their corresponding passwords.

### Code
```python
import subprocess

data = (
    subprocess.check_output(["netsh", "wlan", "show", "profiles"])
    .decode("utf-8")
    .split("\n")
)

profiles = [i.split(":")[1][1:-1] for i in data if "All User Profile" in i]

for i in profiles:
    results = (
        subprocess.check_output(["netsh", "wlan", "show", "profile", i, "key=clear"])
        .decode("utf-8")
        .split("\n")
    )
    results = [b.split(":")[1][1:-1] for b in results if "Key Content" in b]
    try:
        print("{:<30}| {:<}".format(i, results[0]))
    except IndexError:
        print("{:<30}| {:<}".format(i, ""))
```

## License

This project is licensed under the MIT License.
