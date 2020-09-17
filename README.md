import os
import pyautogui
import requests
import re


def upgrade_pip():
    try:
        os.system("pip install --upgrade pip")
        pyautogui.confirm(text="Upgrade if successfully")
    except:
        error = pyautogui.confirm(
            text="Error: Python tools cannot upgrade your pip!!!", buttons=["Try again", "OK"])
        if error == "Try again":
            upgrade_pip()
    start()


def find_python_ver():
    try:
        os.system("python --version")
        start()
    except:
        error = pyautogui.confirm(
            text="Error: Pyhton tools cannot find your python vetsion!!!", buttons=["Try again", "OK"])
        if error == "Try again":
            find_python_ver()
        start()


def install_lib():
    try:
        libName = pyautogui.prompt(
            text="Enter the library name:", title="Name of lib")
        pypi = requests.get("https://pypi.org/search/?q=" + libName + "&o=")
        pypi = pypi.text
        available = re.compile(
            r"class=\"package-snippet\" href=\"/(\w+)/(\d+|\w+)(\w+)/")
        link = available.search(pypi)
        link = str(link)
        libName = link[87:-3:]
        if len(libName) != 0:
            os.system("pip install " + libName)
            pyautogui.confirm(text="Installing is successfuly!!!")
        else:
            pyautogui.confirm(text="Error: Please Enter valid library!!!")
        start()
    except:
        install_lib()


def start():
    first = pyautogui.confirm(text="What do you want to do?", title="Python tools", buttons=[
                              "Upgrade pip", "My python version", "Installing library", "Exit"])

    if first == "Upgrade pip":
        upgrade_pip()
    elif first == "My python version":
        find_python_ver()
    elif first == "Installing library":
        install_lib()
    elif first == "Exit":
        pass


start()
