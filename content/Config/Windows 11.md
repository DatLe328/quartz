### Virtual Desktop

| Tác vụ                        | Phím tắt          | Mô tả chức năng                                     |
| ----------------------------- | ----------------- | --------------------------------------------------- |
| Tạo Virtual Desktop mới       | `Win + Ctrl + D`  | Tạo một desktop ảo mới và chuyển sang nó            |
| Đóng Virtual Desktop hiện tại | `Win + Ctrl + F4` | Xóa desktop ảo hiện tại và quay về desktop trước đó |
| Chuyển sang desktop bên trái  | `Win + Ctrl + ←`  | Chuyển sang desktop ảo bên trái                     |
| Chuyển sang desktop bên phải  | `Win + Ctrl + →`  | Chuyển sang desktop ảo bên phải                     |
| Mở chế độ xem Task View       | `Win + Tab`       | Xem tất cả desktop ảo và các cửa sổ đang mở         |

### Disable 'Search the Web'

Use the following steps:

- Open the Registry Editor by searching for "regedit" in the Start menu and clicking the top result.
- Click yes if prompted by User Account Control.
- Navigate to HKEY_CURRENT_USER\Software\Policies\Microsoft\Windows\Explorer. If the Explorer key does not exist, right-click on Windows and create a new key called Explorer.
- Create a new DWORD (32-bit) registry key and name it DisableSearchBoxSuggestions.
- You can create a new registry key by right-clicking in the right window pane and selecting New->DWORD.
- Double-click on DisableSearchBoxSuggestions to edit it and set the Value data field to 1 and click OK.
- Close the Registry Editor and reboot your computer.

Alternatively, you can use the Group Policy Editor to disable web search in Windows 11. This method is recommended for users running Windows 11 Pro or higher. Here are the steps:

- Press Windows + R, type in gpedit.msc, and press Enter.
- Navigate to User Configuration > Administrative Templates > Windows Components > File Explorer.
and enable the "Turn off display of recent search entries in the File Explorer search box" policy.

## Powershell

### Custom profile(keymap, alias, import,...)

```ps1
mkdir .\.config\powershell
nvim user_profile.ps1
```

Add this

```ps1
Import-Module Terminal-Icons
oh-my-posh init pwsh --config "$env:POSH_THEMES_PATH/catppuccin_latte.omp.json" | Invoke-Expression

# PSReadLine Note
# "Menu" completion (somewhat like Intellisense, select completion with arrows) via Ctrl+Space
# <F2> To toogle PSReadLine view
Set-PSReadLineOption -PredictionSource History
Set-PSReadlineOption -PredictionViewStyle ListView

#Set-Alias ll ls
```

Then save file

```ps1
nvim $PROFILE.CurrentUserCurrentHost
```

Add this line

```ps1
. $env:USERPROFILE\.config\powershell\user_profile.ps1
```

## WSL2

- If you have error can't use `wsl.exe --install` or error while install linux distro you should go to microsoft store then install manually.

