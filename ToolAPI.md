# Introduction #

The purpose of this page is to outline all available REST API endpoints that can be called upon in the security advisor container tool. This includes what is the expected value(s) as well as how to interpret these values.

- [/api/lynisinfo](#/api/lynisinfo)
- [/api/toolinfo](#/api/toolinfo)

# /api/lynisinfo #

- `lynis_report`: Serves the raw file contents of the Lynis report file. This is equivalent to the contents of `lynis-report.dat`
    - `{'file_status': 'No scan results available'}`
      <details>
      <summary>Example of Lynis report file contents</summary>

      [jRgWKXjd](logs/jRgWKXjd)

      </details>

- `lynis_log`: Serves the raw file contents of the Lynis log file. This is equivalent to the contents of `lynis.log`
    - `{'file_status': 'No scan results available'}`
      <details>
      <summary>Example of Lynis log file contents</summary>

      [B56n1kcR](logs/B56n1kcR)

      </details>

- `lynis_output`: Serves the raw output from the Lynis console output. This is equivalent to just running the Lynis tool normally and seeing the console output.
    - `{'file_status': 'No scan results available'}`
      <details>
      <summary>Example of Lynis console output</summary>
      
      [output.txt](logs/output.txt)

      </details>

- `lynis_scan_start`: Starts the Lynis audit scan in the background.
    - `{'scan_started': 'Scan has started'}`
    - `{'scan_started': 'Scan already in progress'}`

- `lynis_scan_status`: Shows whether there is an ongoing scan or not.
    - `{'scan_status': 'Busy'}`
    - `{'scan_stauts': 'Idle'}`

- `lynis_suggestions`: Returns a json array of json objects for each suggestion that resulted from the **most recent** Lynis scan.
   - `{'scan_results': 'Not available'}`
   -  If there are results available then each element in the json array will follow this format:
      - ```
        {'issue': <some string>, #short summary describing the issue/suggestion
        'code': <some string>, #code which shows which Lynis test triggered this issue/suggestion   
        'detail' : <some string>, #further details on issue/suggestion if available
        'solution': <some string>, #brief description of solution to issue/suggestion if available
        'link': <some string with an http link>} #web link to lynis website with more details on issue/suggestion if available
        ```
- `lynis_scan_history`: Returns a json array of json objects for each scan that has occurred in the recorded history

```
{ 'finished': <boolean for whether this scan event is completed or ongoing>  
  'finished_at': <float value for time when scan finished>  
  'id': <int value to tell apart scan events>  
  'is_current': <boolean value for whether this scan is the most recent or not>  
  'started_at': <float value for time when scan was started>  
  'suggestions': <json array of results tied to this scan event same structure as lynis_suggestions>
``` 
- `lynis_clear_history`: Clears database file that contains scan history
   - `{'0' : true}` Returns a list of id values for all entries cleared from the database

- `lynis_latest_scan_data`: Returns a json object of the latest scan information.
   - Similar to `lynis_scan_history` but only returns one object rather than the entire history

# /api/toolinfo #

- `os_version`:  Serves the content’s of the host systems `/etc/issue` a.k.a. the version and build of the running TorizonCore OS.
   - `{ 'os_version': 'undefined' }`
   - Example valid output: `{ 'os_version': 'TorizonCore 4.0.0-devel-202004+build.3' }` 
- `kernel_version`: Serve the output of running uname -r a.k.a the version of the live kernel on the system.
   - `{ 'kernel_version': 'undefined' }`
   - Example valid output: `{ 'kernel_version': '5.4.24-4.0.0-devel+git.585b24d35ce7' }`
- `tool_version`: Serves the version of the Advisor Tool itself.
   - Example output: `{ 'tool_version': '0.1' }`
- `module_info`: Serves a json object with information about the Toradex module the tool is being run on.
   - Example valid output:
     ``` 
     my_module = {
            'serial': '05228985',
            'product': 'apalis-imx6',
            'revision': 'V1.1'
        }
    ```
    - If any of the above fields can’t be determined then it will default to, `unavailable`, instead. 

- `lynis_version`: Serves the version of Lynis that is being used by the SAT. This is equivalent to the Lynis git branch/tag that was pulled.
   - Example output: `{ 'lynis_version': 'master' }`
- `lynis_license`: Serves the license of the Lynis tool
   - `{ 'lynis_license': 'GPLv3' }`