
Description: General Details for Page Format
---

Text can be **bold**, _italic_, or ~~strikethrough~~.

[Link to another page](https://www.youtube.com/channel/UCvScgo6mAvbMEjszK4sSj6g).

**Consists of**
* EDA
* Git related
* Azure Devops
* Adding a timestamp to a file
* Read a file in different formats
* Checkin and Checkout in sharepoint using pnp powershell
* To replace a value in .property file or .config file
* To check characters in a string like A-Z,a-z,0-9

# EDA

| SL.NO | Activities                          | 
|:------|:------------------------------------|
| 1     | List                                |
| 2     | For unique                          |
| 3     | For rename                          |
| 4     | Lambda                              |
| 6     | Split rows/columns                  |
| 7     | Drop rows/columns                   |
| 8     | Replace particular values in column |
| 9     | To handle missing values            |
| 10     | Additional Information              |
| 11     | Replace particular values in column|

#### 1.List
```
a=[1,3,46,7]  //a[0:-1]-input              //1,3,46 -output
a=88%         //a[0:2] or a[0:-1] -input  //88 -output
```

#### 2.For unique
```
df.nunique()   //for all columns
df['column_name'].unique()  //for particular column
df['column_name'].value_counts(normalize=True)   //for particular column
```

#### 3.For rename
```
df.rename(columns={'revol_util':'revol_util_percentage'},inplace=True)
```

#### 4.Lambda
```
df['marks']=loan['earliest_cr_line'].apply(lambda x:x[0:2])  //88%
df['loan_status_num'] = df['loan_status'].apply(lambda x: 1 if x=='Charged Off' else 0)
df['date']=df['issue_d'].apply(lambda x:x.split('-')[0])  //01-01-2020-input
```

#### 5.Split 
! dates type
```
m, y = loan['earliest_cr_line'].str.split('-').str  //Oct-99-input
df['issue_month']=df['issue_d'].apply(lambda x:x.split('-')[0])  
date=loan['earliest_cr_line'].apply(lambda x:x.day/x.month/x.year) [any one] //01-01-2020-input
month=loan['earliest_cr_line'].apply(lambda x:x[0:3])  //oct-02-2948/-input 
```
! percentages
```
df['marks']=loan['earliest_cr_line'].apply(lambda x:x[0:2])  //88%
df['marks']=pd.Series(df['marks']).str.replace('%', '').astype(float)  //88%
df["revol_util"] = pd.to_numeric(df["revol_util"].apply(lambda x : x.split('%')[0]))  //88% 
(Above one pd.to_numeric will basically converts to numeric format)
 ```
#### 6.Drop rows/columns 
! Based on NA values
````
df.dropna(axis='columns/rows/index',how='all/any',inplace=True,subset=None)
[all - if all values in a row/column is null then row/column will be removed
any - if any 1 value in a row/column is null then entire row/column will be removed
inplace - if true then there is no need to assign to other variable, since the changes will be saved
subset - if any column/row name passed then only in that column/row na values will be moved]
````

##### 6.1.Drop few rows or columns on some condition
! Based on unique
```
step1: Assign to other variale of that condition
      a=df.nunique()
step2: Apply that condition by assigning to same variable
      b=a[df.nunique()==1]
step3: As below
      df.drop(columns=list(b.index),axis='columns,inplace=True) 
```

#### 7.Replace particular values in column
```
df['emp_length'].replace(to_replace = ['nan'],value='OTHER',inplace = True)  //categorical column
```

#### 8.To handle missing values
```
df["emp_length"].fillna(df["emp_length"].mode()[0], inplace = True)          //categorical column
df['emp_length'].replace(to_replace = ['nan'],value='OTHER',inplace = True)  //categorical column
df["emp_length"].fillna(df["emp_length"].mean/median()[0], inplace = True)   //numerical column
```
#### 9.Additional information
! To know percentage of each column missing values
```
missing_value_df=pd.DataFrame(list(dict(df.isnull().sum() * 100 / len(df)).items()),columns=['column','missing_percentage'])
```





# Git related

* Fetch Recent files from Git
- git ls-files -z | xargs -0 -n1 -I{} -- git log -1 --format="%ai {}" {} | sort > ./output.txt (where this will store all the uploaded files name data in output.txt file)

# Azure Devops

* Script to move file to Azure Repository
```
git config --global user.email "you@example.com"
git config --global user.name "Your Name"
git config --global user.password "secret"
git checkout rollback
git add  $(Build.SourcesDirectory)/Rollback_Files/** (pushes the files to github repository)
git status
git commit -m "copy_files"
git push origin rollback
```
[Link for the same](https://stackoverflow.com/questions/66323959/how-to-move-azure-git-repos-file-from-one-folder-to-another-folder-using-azure-d)

## Adding a timestamp to a file

> new_path = '%s_%s' % (datetime.now().strftime(FORMAT), i)
> print(new_path)
>
> Using above code,time will be genearated and stored in a variable , using this variable file name can be changed to other file like a.txt to 20211227100001_a.txt

### Reading a file different formats

```python
f = open(path, "r")
while(True):
    #read next line
    line = f.readline()
```

```python
def get_last_created_file(filename):
    with open(filename) as f:
        for line in f:
            pass
    return line.split(" ")[-1]
store_file_location=get_last_created_file("./output.txt")//[2021-11-29 05:21:53 +0000 result_file.py]
```
To read in a csv format(if format is in [2021-11-29 05:21:53 +0000 result_file.py])
```python
def get_last_created_file(filename):
            a=[]
            with open(filename, newline='') as csvfile:
                spamreader = csv.reader(csvfile, delimiter=' ', quotechar='|')
                for row in spamreader:
                    a.append(row[3])
            return a
store_file_location=get_last_created_file("./output.txt") 
```

#### Checkin and Checkout in sharepoint using pnp powershell

Teamname represents project team name
say for example file is present in documents under general under 3 line is the format.(shared document constant)
There should be no space in a team name

* checkin- $targetFile.CheckIn("Checked In at $(Get-Date)","MinorCheckIn")
* checkout- $targetFile.CheckOut()
```
Connect-PnPOnline -Url https://xxxx.sharepoint.com/sites/Teamname -Credentials (Get-Credential)
$clientContext = Get-PnPContext
$ListItem = Get-PnPFile -Url "/Shared Documents/General/filename.xlsx" -AsListItem
$targetFile = $ListItem.File
$clientContext.Load($targetFile)
$clientContext.ExecuteQuery()
$targetFile.CheckOut() (Relplace for checkin or checkout)
$clientContext.ExecuteQuery()
Disconnect-PnPOnline
```
[Link for reference](https://www.codesharepoint.com/powershell/check-in-file-in-sharepoint-using-powershell).

#### To replace a value in .property file or .config file

[Link for reference](https://gist.github.com/jontsai/fcd2d410612990d2e85e44a7bccfa79a)
pass file name, variable where to be changed and the value.
line 18 to 62 follow

#### To check characters in a string like A-Z,a-z,0-9

Tried import re
[link](https://stackoverflow.com/questions/57011986/how-to-check-that-a-string-contains-only-a-z-a-z-and-0-9-characters)

##### Header 5

1.  This is an ordered list following a header.
2.  This is an ordered list following a header.
3.  This is an ordered list following a header.

###### Header 6

| head1        | head two          | three |
|:-------------|:------------------|:------|
| ok           | good swedish fish | nice  |
| out of stock | good and plenty   | nice  |
| ok           | good `oreos`      | hmm   |
| ok           | good `zoute` drop | yumm  |

### There's a horizontal rule below this.

* * *

### Here is an unordered list:

*   Item foo
*   Item bar
*   Item baz
*   Item zip

### And an ordered list:

1.  Item one
1.  Item two
1.  Item three
1.  Item four

### And a nested list:

- level 1 item
  - level 2 item
  - level 2 item
    - level 3 item
    - level 3 item
- level 1 item
  - level 2 item
  - level 2 item
  - level 2 item
- level 1 item
  - level 2 item
  - level 2 item
- level 1 item

### Small image

![Octocat](https://github.githubassets.com/images/icons/emoji/octocat.png)

### Large image

![Branching](https://guides.github.com/activities/hello-world/branching.png)


### Definition lists can be used with HTML syntax.

<dl>
<dt>Name</dt>
<dd>Godzilla</dd>
<dt>Born</dt>
<dd>1952</dd>
<dt>Birthplace</dt>
<dd>Japan</dd>
<dt>Color</dt>
<dd>Green</dd>
</dl>

```
Long, single-line code blocks should not wrap. They should horizontally scroll if they are too long. This line should be long enough to demonstrate this.
```

```
The final element.
```
