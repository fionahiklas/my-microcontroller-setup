# Test scripts for the Pico

## Overview

This following subdirectories contain test scripts for the Pico with instructions on how to run them


## Python

### OLED Test

This requires the [MicroPython SSD1306](https://pypi.org/project/micropython-ssd1306/) library which Thonny seems to 
struggle with installing as a package.  It's available on [Pypi](https://pypi.org/project/micropython-ssd1306/).

The way I got it to work was to clone the code from github and copy the file across to the Pico using rshell

```
cd ~/wd/3rdparty
git clone https://github.com/stlehmann/micropython-ssd1306.git

# Go back to this project folder
cd -
```

Run rshell as per the specific machine instructions in the [README.md](../README.md) at the prompt run

```
cp ~/wd/3rdparty/micropython-ssd1306/ssd1306.py /pyboard 
ls /pyboard
```

The `ls` command should list the `ssd1306.py` file (and any others you copied across).

