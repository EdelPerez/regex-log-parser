# Parsing Logs with Python and Regexp


```python
import re
import pandas as pd
```

### What is a regex (regular expression)

A regular expression (shortened as regex or regexp; also referred to as rational expression) is a sequence of characters that specifies a search pattern. Usually such patterns are used by string-searching algorithms for "find" or "find and replace" operations on strings, or for input validation. It is a technique developed in theoretical computer science and formal language theory.<sub>[<a href='https://en.wikipedia.org/wiki/Regular_expression'>Wikipedia</a>]</sub>

<b><a href='https://pythex.org'>Python Regular Expression Editor</a></b> can be used to build regular expression code. The website provides an easy to understand view of regex code selection.


```python
def log_to_data_frame(file: str, regex: str, columns: list) -> pd.DataFrame:
      """Generate a DataFrame object from the provided log file.

    Args:
        file (str): Log file location.
        regex (str): Regular expression
        columns (list): DataFrame Columns

    Returns:
        pd.DataFrame: DataFrame object
    """
    df = pd.DataFrame(columns=columns)

    with open(file, mode='r', encoding='utf8', newline='') as log:
        for line in log:
            data = log.readline().replace('\n', ' ').strip()
            
            try:
                row = re.compile(regex).findall(data)
            except:
                print('Unable to parse string.')
            else:
                df = pd.concat(
                    [
                        df,
                        pd.DataFrame(
                            row,
                            columns=['DateTime', 'Caller', 'Task', 'Code', 'Detail']
                        )
                    ], ignore_index=True
                )

    return df
```


```python
regex = r'(\d{4}-\d{2}-\d{2}\s\d{2}:\d{2}:\d{2}-\d{2})\s([\w-]+)\s([a-zA-Z0-9_ ]*)\[([\d]*)\]:\s(.*)'
columns = ['DateTime', 'Caller', 'Task', 'Code', 'Detail']
file = 'install.log'

df = log_to_data_frame(file, regex, columns)
```

```python
df.head(10)
```

<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>DateTime</th>
      <th>Caller</th>
      <th>Task</th>
      <th>Code</th>
      <th>Detail</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2020-12-14 10:54:21-08</td>
      <td>localhost</td>
      <td>softwareupdate_firstrun_tasks</td>
      <td>198</td>
      <td>Rebuilding Tag-Cache inside of ProductMetadata...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2020-12-14 10:54:22-08</td>
      <td>localhost</td>
      <td>softwareupdated</td>
      <td>230</td>
      <td>Initializing SoftwareUpdateMacController (SUMa...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2020-12-14 10:54:22-08</td>
      <td>localhost</td>
      <td>softwareupdated</td>
      <td>230</td>
      <td>SUOSUAlarmObserver: Setting alarm event stream...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2020-12-14 10:54:23-08</td>
      <td>localhost</td>
      <td>softwareupdated</td>
      <td>230</td>
      <td>SUOSUServiceDaemon: Error reading /var/folders...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2020-12-14 10:54:23-08</td>
      <td>localhost</td>
      <td>softwareupdated</td>
      <td>230</td>
      <td>authorizeWithEmptyAuthorizationForRights: Requ...</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2020-12-14 10:54:23-08</td>
      <td>localhost</td>
      <td>softwareupdated</td>
      <td>230</td>
      <td>Previous System Version : (null), Current Syst...</td>
    </tr>
    <tr>
      <th>6</th>
      <td>2020-12-14 10:54:24-08</td>
      <td>localhost</td>
      <td>softwareupdated</td>
      <td>230</td>
      <td>SUStatisticsManager: Successfully reported sta...</td>
    </tr>
    <tr>
      <th>7</th>
      <td>2020-12-14 10:54:24-08</td>
      <td>localhost</td>
      <td>softwareupdated</td>
      <td>230</td>
      <td>BackgroundActivity: Scheduling one-time backgr...</td>
    </tr>
    <tr>
      <th>8</th>
      <td>2020-12-14 10:54:25-08</td>
      <td>MacBook-Air</td>
      <td>Language Chooser</td>
      <td>192</td>
      <td>LCA: No networks found in Wifi scan.</td>
    </tr>
    <tr>
      <th>9</th>
      <td>2020-12-14 10:54:25-08</td>
      <td>MacBook-Air</td>
      <td>Installer Progress</td>
      <td>52</td>
      <td>IASGetCurrentInstallPhase: Unable to get the c...</td>
    </tr>
  </tbody>
</table>
</div>

<br>

```python
df.to_csv('output.csv',index=False)
```
