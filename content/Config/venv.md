## Python flask
```shell
python -m pip install -r .\requirements.txt
python -m pip freeze .\requirements.txt
pip show [package name]
pip list
```
### Linux
```shell
# Axtivate virtual environment
source venv/bin/activate
# Assign PYTHONPATH 
export PYTHONPATH=$(pwd)
```
### Windows
```shell
# Axtivate virtual environment
.\.venv\Scripts\activate
# Assign PYTHONPATH 
$env:PYTHONPATH = (Get-Location).Path
```
